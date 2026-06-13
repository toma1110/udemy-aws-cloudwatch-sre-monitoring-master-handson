# Section 3 実行コマンド集

この手順は教材用AWSアカウントで実行する想定です。本番環境では実行しないでください。

## 事前変数

PowerShell:

```powershell
Set-Location .\courses\c002-aws-cloudwatch-sre-monitoring-master-course\handson\s3-monitoring-foundation-prep

$env:AWS_REGION = "ap-northeast-1"
$StackName = "c002-cw-sre-handson"
$ProjectPrefix = "c002-cw-sre"
$ServiceName = "checkout-api"
$MetricNamespace = "Udemy/CloudWatchSREHandson"
$Template = ".\cloudformation\cloudwatch_sre_monitoring_lab.yaml"
```

## 作成前確認

```powershell
aws sts get-caller-identity
aws configure get region
aws cloudformation validate-template --template-body "file://$Template"
```

期待結果:

- 自分の学習用AWSアカウントであることを確認できる
- リージョンが `ap-northeast-1` など意図した値になっている
- CloudFormation template validationが成功する

## スタック作成

メール通知を使わない場合:

```powershell
aws cloudformation deploy `
  --stack-name $StackName `
  --template-file $Template `
  --parameter-overrides ProjectPrefix=$ProjectPrefix ServiceName=$ServiceName CustomMetricNamespace=$MetricNamespace `
  --tags Course=c002 Purpose=udemy-handson
```

メール通知を使う場合:

```powershell
$NotificationEmail = "your-email@example.com"

aws cloudformation deploy `
  --stack-name $StackName `
  --template-file $Template `
  --parameter-overrides ProjectPrefix=$ProjectPrefix ServiceName=$ServiceName CustomMetricNamespace=$MetricNamespace NotificationEmail=$NotificationEmail `
  --tags Course=c002 Purpose=udemy-handson
```

注意:

- メール通知を使う場合はSNS確認メールの承認が必要です。
- 確認メールを承認するまで、SNS SubscriptionはPending状態になることがあります。

## 出力確認

```powershell
aws cloudformation describe-stacks `
  --stack-name $StackName `
  --query "Stacks[0].Outputs" `
  --output table
```

期待結果:

- LogGroupName
- AlertTopicArn
- DashboardName
- HighErrorRateAlarmName
- HighLatencyAlarmName
- CompositeServiceAlarmName

## サンプルログ投入

```powershell
$LogGroupName = "/$ProjectPrefix/application"
$LogStreamName = "sample-run-$(Get-Date -Format yyyyMMddHHmmss)"

aws logs create-log-stream `
  --log-group-name $LogGroupName `
  --log-stream-name $LogStreamName

$now = [DateTimeOffset]::UtcNow.ToUnixTimeMilliseconds()
$events = @(
  @{
    timestamp = $now - 30000
    message = '{"level":"INFO","service":"checkout-api","path":"/checkout","status":200,"latency_ms":180,"request_id":"req-001"}'
  },
  @{
    timestamp = $now - 20000
    message = '{"level":"ERROR","service":"checkout-api","path":"/checkout","status":500,"latency_ms":940,"request_id":"req-002","error":"payment-timeout"}'
  },
  @{
    timestamp = $now - 10000
    message = '{"level":"ERROR","service":"checkout-api","path":"/checkout","status":502,"latency_ms":1200,"request_id":"req-003","error":"upstream-bad-gateway"}'
  }
)

$tmpEvents = Join-Path $env:TEMP "c002-cw-sre-log-events.json"
[System.IO.File]::WriteAllText($tmpEvents, ($events | ConvertTo-Json -Depth 4))

aws logs put-log-events `
  --log-group-name $LogGroupName `
  --log-stream-name $LogStreamName `
  --log-events "file://$tmpEvents"
```

期待結果:

- CloudWatch Logsの対象Log Groupに3件のログイベントが入る
- Logs InsightsでERRORログを抽出できる

## サンプルメトリクス投入

```powershell
aws cloudwatch put-metric-data `
  --namespace $MetricNamespace `
  --metric-data file://.\sample-data\custom_metrics.json
```

期待結果:

- DashboardにRequestCount、ErrorCount、LatencyP95が表示される
- ErrorCountとLatencyP95のAlarmがALARM状態になる可能性がある

## 初期状態確認

```powershell
aws cloudwatch describe-alarms `
  --alarm-name-prefix "$ProjectPrefix-$ServiceName" `
  --query "MetricAlarms[].{Name:AlarmName,State:StateValue,Reason:StateReason}" `
  --output table

aws cloudwatch describe-alarms `
  --alarm-types CompositeAlarm `
  --alarm-name-prefix "$ProjectPrefix-$ServiceName" `
  --query "CompositeAlarms[].{Name:AlarmName,State:StateValue,Rule:AlarmRule}" `
  --output table
```

期待結果:

- Metric AlarmとComposite Alarmの状態を確認できる
- ALARMになった場合も、教材用メトリクスに反応していることを説明できる

## 参照したAWS公式ドキュメント

- CloudFormation CloudWatch snippets: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-cloudwatch.html
- CloudWatch Logs log groups and streams: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html

# Section 3 実行コマンド集

この手順は、AWS CloudShellで実行する想定です。CloudShellを使うことで、AWS CLIや認証情報のローカル環境差異を減らせます。本番環境や会社アカウントでは実行しないでください。

## 事前準備

AWS CloudShellを開き、教材リポジトリを取得して対象ディレクトリへ移動します。

```bash
git clone https://github.com/toma1110/udemy-aws-cloudwatch-sre-monitoring-master-handson.git
cd udemy-aws-cloudwatch-sre-monitoring-master-handson/handson/s3-monitoring-foundation-prep
```

すでに取得済みの場合は、最新化してから移動します。

```bash
cd ~/udemy-aws-cloudwatch-sre-monitoring-master-handson
git pull
cd handson/s3-monitoring-foundation-prep
```

## 事前変数

```bash
export AWS_REGION="ap-northeast-1"
export AWS_DEFAULT_REGION="$AWS_REGION"

STACK_NAME="c002-cw-sre-handson"
PROJECT_PREFIX="c002-cw-sre"
SERVICE_NAME="checkout-api"
METRIC_NAMESPACE="Udemy/CloudWatchSREHandson"
TEMPLATE="cloudformation/cloudwatch_sre_monitoring_lab.yaml"
```

## 作成前確認

```bash
aws sts get-caller-identity
aws configure get region
aws cloudformation validate-template --template-body "file://$TEMPLATE"
```

期待結果:

- 自分の学習用AWSアカウントであることを確認できる
- リージョンが `ap-northeast-1` など意図した値になっている
- CloudFormation template validationが成功する

## スタック作成

メール通知を使わない場合:

```bash
aws cloudformation deploy \
  --stack-name "$STACK_NAME" \
  --template-file "$TEMPLATE" \
  --parameter-overrides \
    ProjectPrefix="$PROJECT_PREFIX" \
    ServiceName="$SERVICE_NAME" \
    CustomMetricNamespace="$METRIC_NAMESPACE" \
  --tags Course=c002 Purpose=udemy-handson
```

メール通知を使う場合:

```bash
NOTIFICATION_EMAIL="your-email@example.com"

aws cloudformation deploy \
  --stack-name "$STACK_NAME" \
  --template-file "$TEMPLATE" \
  --parameter-overrides \
    ProjectPrefix="$PROJECT_PREFIX" \
    ServiceName="$SERVICE_NAME" \
    CustomMetricNamespace="$METRIC_NAMESPACE" \
    NotificationEmail="$NOTIFICATION_EMAIL" \
  --tags Course=c002 Purpose=udemy-handson
```

注意:

- メール通知を使う場合はSNS確認メールの承認が必要です。
- 確認メールを承認するまで、SNS SubscriptionはPending状態になることがあります。

## 出力確認

```bash
aws cloudformation describe-stacks \
  --stack-name "$STACK_NAME" \
  --query "Stacks[0].Outputs" \
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

```bash
LOG_GROUP_NAME="/$PROJECT_PREFIX/application"
LOG_STREAM_NAME="sample-run-$(date +%Y%m%d%H%M%S)"

aws logs create-log-stream \
  --log-group-name "$LOG_GROUP_NAME" \
  --log-stream-name "$LOG_STREAM_NAME"

NOW_MS=$(date +%s%3N)
cat > /tmp/c002-cw-sre-log-events.json <<JSON
[
  {
    "timestamp": $((NOW_MS - 30000)),
    "message": "{\"level\":\"INFO\",\"service\":\"checkout-api\",\"path\":\"/checkout\",\"status\":200,\"latency_ms\":180,\"request_id\":\"req-001\"}"
  },
  {
    "timestamp": $((NOW_MS - 20000)),
    "message": "{\"level\":\"ERROR\",\"service\":\"checkout-api\",\"path\":\"/checkout\",\"status\":500,\"latency_ms\":940,\"request_id\":\"req-002\",\"error\":\"payment-timeout\"}"
  },
  {
    "timestamp": $((NOW_MS - 10000)),
    "message": "{\"level\":\"ERROR\",\"service\":\"checkout-api\",\"path\":\"/checkout\",\"status\":502,\"latency_ms\":1200,\"request_id\":\"req-003\",\"error\":\"upstream-bad-gateway\"}"
  }
]
JSON

aws logs put-log-events \
  --log-group-name "$LOG_GROUP_NAME" \
  --log-stream-name "$LOG_STREAM_NAME" \
  --log-events file:///tmp/c002-cw-sre-log-events.json
```

期待結果:

- CloudWatch Logsの対象Log Groupに3件のログイベントが入る
- Logs InsightsでERRORログを抽出できる

## サンプルメトリクス投入

```bash
aws cloudwatch put-metric-data \
  --namespace "$METRIC_NAMESPACE" \
  --metric-data file://sample-data/custom_metrics.json
```

期待結果:

- DashboardにRequestCount、ErrorCount、LatencyP95が表示される
- ErrorCountとLatencyP95のAlarmがALARM状態になる可能性がある

## 初期状態確認

```bash
aws cloudwatch describe-alarms \
  --alarm-name-prefix "$PROJECT_PREFIX-$SERVICE_NAME" \
  --query "MetricAlarms[].{Name:AlarmName,State:StateValue,Reason:StateReason}" \
  --output table

aws cloudwatch describe-alarms \
  --alarm-types CompositeAlarm \
  --alarm-name-prefix "$PROJECT_PREFIX-$SERVICE_NAME" \
  --query "CompositeAlarms[].{Name:AlarmName,State:StateValue,Rule:AlarmRule}" \
  --output table
```

期待結果:

- Metric AlarmとComposite Alarmの状態を確認できる
- ALARMになった場合も、教材用メトリクスに反応していることを説明できる

## 参照したAWS公式ドキュメント

- [CloudFormation CloudWatch snippets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-cloudwatch.html)
- [CloudWatch Logs log groups and streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html)

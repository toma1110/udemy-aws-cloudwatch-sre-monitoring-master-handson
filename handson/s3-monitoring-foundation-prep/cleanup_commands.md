# クリーンアップ手順

AWSリソースを作成した場合は、演習後に必ず削除します。

## 変数

```powershell
Set-Location .\courses\c002-aws-cloudwatch-sre-monitoring-master-course\handson\s3-monitoring-foundation-prep

$env:AWS_REGION = "ap-northeast-1"
$StackName = "c002-cw-sre-handson"
$ProjectPrefix = "c002-cw-sre"
$ServiceName = "checkout-api"
```

## 削除

```powershell
aws cloudformation delete-stack --stack-name $StackName

aws cloudformation wait stack-delete-complete --stack-name $StackName
```

期待結果:

- CloudFormation Stackが `DELETE_COMPLETE` になる
- Stack配下のLog Group、SNS Topic、Dashboard、Alarm、Composite Alarmが削除される

## 削除後確認

```powershell
aws cloudformation describe-stacks --stack-name $StackName
```

期待結果:

- Stackが見つからないエラーになる

```powershell
aws logs describe-log-groups `
  --log-group-name-prefix "/$ProjectPrefix/" `
  --query "logGroups[].logGroupName" `
  --output table

aws cloudwatch list-dashboards `
  --dashboard-name-prefix "$ProjectPrefix" `
  --query "DashboardEntries[].DashboardName" `
  --output table

aws cloudwatch describe-alarms `
  --alarm-name-prefix "$ProjectPrefix-$ServiceName" `
  --query "MetricAlarms[].AlarmName" `
  --output table

aws cloudwatch describe-alarms `
  --alarm-types CompositeAlarm `
  --alarm-name-prefix "$ProjectPrefix-$ServiceName" `
  --query "CompositeAlarms[].AlarmName" `
  --output table

aws sns list-topics `
  --query "Topics[?contains(TopicArn, '$ProjectPrefix-alerts')].TopicArn" `
  --output table
```

期待結果:

- 教材用Log Group、Dashboard、Alarm、Composite Alarm、SNS Topicが残っていない

## 料金確認

- CloudWatch Metricsの履歴データは一定期間見えることがあります。履歴表示と稼働リソースを混同しないでください。
- BillingやCost Explorerの反映には遅れがあります。学習日と翌日に確認予定を記録してください。

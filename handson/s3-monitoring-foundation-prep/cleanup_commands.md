# クリーンアップ手順

AWSリソースを作成した場合は、演習後に必ず削除します。この手順はAWS CloudShellで実行する想定です。

## 変数

```bash
cd ~/udemy-aws-cloudwatch-sre-monitoring-master-handson/handson/s3-monitoring-foundation-prep

export AWS_REGION="ap-northeast-1"
export AWS_DEFAULT_REGION="$AWS_REGION"

STACK_NAME="c002-cw-sre-handson"
PROJECT_PREFIX="c002-cw-sre"
SERVICE_NAME="checkout-api"
```

## 削除

```bash
aws cloudformation delete-stack --stack-name "$STACK_NAME"

aws cloudformation wait stack-delete-complete --stack-name "$STACK_NAME"
```

期待結果:

- CloudFormation Stackが `DELETE_COMPLETE` になる
- Stack配下のLog Group、SNS Topic、Dashboard、Alarm、Composite Alarmが削除される

## 削除後確認

```bash
aws cloudformation describe-stacks --stack-name "$STACK_NAME"
```

期待結果:

- Stackが見つからないエラーになる

```bash
aws logs describe-log-groups \
  --log-group-name-prefix "/$PROJECT_PREFIX/" \
  --query "logGroups[].logGroupName" \
  --output table

aws cloudwatch list-dashboards \
  --dashboard-name-prefix "$PROJECT_PREFIX" \
  --query "DashboardEntries[].DashboardName" \
  --output table

aws cloudwatch describe-alarms \
  --alarm-name-prefix "$PROJECT_PREFIX-$SERVICE_NAME" \
  --query "MetricAlarms[].AlarmName" \
  --output table

aws cloudwatch describe-alarms \
  --alarm-types CompositeAlarm \
  --alarm-name-prefix "$PROJECT_PREFIX-$SERVICE_NAME" \
  --query "CompositeAlarms[].AlarmName" \
  --output table

aws sns list-topics \
  --query "Topics[?contains(TopicArn, '$PROJECT_PREFIX-alerts')].TopicArn" \
  --output table
```

期待結果:

- 教材用Log Group、Dashboard、Alarm、Composite Alarm、SNS Topicが残っていない

## 料金確認

- CloudWatch Metricsの履歴データは一定期間見えることがあります。履歴表示と稼働リソースを混同しないでください。
- BillingやCost Explorerの反映には遅れがあります。学習日と翌日に確認予定を記録してください。

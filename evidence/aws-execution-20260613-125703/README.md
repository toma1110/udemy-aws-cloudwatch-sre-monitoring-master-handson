# AWS実行証跡

- Run ID: `20260613-125703`
- Region: `ap-northeast-1`
- Stack name: `c002-cw-sre-0613125703`
- Project prefix: `c002-cw-0613125703`
- Service name: `checkout-api`
- Namespace: `Udemy/CloudWatchSREHandson`
- Deploy status: `completed`
- Cleanup status: `stack-delete-complete`

## 証跡ファイル

- [`03_validate_template.json`](03_validate_template.json): CloudFormationテンプレート検証結果
- [`04_cloudformation_deploy.txt`](04_cloudformation_deploy.txt): スタック作成結果
- [`05_stack_describe_outputs.json`](05_stack_describe_outputs.json): スタック出力値
- [`07_put_log_events.txt`](07_put_log_events.txt): サンプルログ投入結果
- [`08_put_metric_data.txt`](08_put_metric_data.txt): サンプルカスタムメトリクス投入結果
- [`10_logs_insights_results.json`](10_logs_insights_results.json): Logs Insights実行結果
- [`11_metric_alarms.json`](11_metric_alarms.json): スタックで作成されたMetric Alarm確認結果
- [`12_composite_alarms.json`](12_composite_alarms.json): スタックで作成されたComposite Alarm確認結果
- [`13_dashboards.json`](13_dashboards.json): スタックで作成されたDashboard確認結果
- [`90_delete_stack.txt`](90_delete_stack.txt): スタック削除実行結果
- [`91_wait_stack_delete_complete.txt`](91_wait_stack_delete_complete.txt): スタック削除完了待ち結果
- [`92_post_cleanup_stack_describe.txt`](92_post_cleanup_stack_describe.txt): 削除後のスタック不存在確認
- [`93_post_cleanup_log_groups.json`](93_post_cleanup_log_groups.json): 削除後のロググループ残存確認
- [`94_post_cleanup_dashboards.json`](94_post_cleanup_dashboards.json): 削除後のダッシュボード残存確認
- [`95_post_cleanup_metric_alarms.json`](95_post_cleanup_metric_alarms.json): 削除後のMetric Alarm残存確認
- [`96_post_cleanup_composite_alarms.json`](96_post_cleanup_composite_alarms.json): 削除後のComposite Alarm残存確認
- [`97_post_cleanup_sns_topics.json`](97_post_cleanup_sns_topics.json): 削除後のSNS Topic残存確認

## クリーンアップ結果

CloudFormationスタックは同一実行内で削除済みです。削除後チェックでは、スタック、ロググループ、ダッシュボード、Metric Alarm、Composite Alarm、SNS Topicが残っていないことを確認しています。

CloudWatchのカスタムメトリクス履歴は、AWS側の保持期間中に表示される場合があります。これは稼働中リソースではありません。

公開証跡では、AWSアカウントIDとIAMユーザー情報をマスクしています。

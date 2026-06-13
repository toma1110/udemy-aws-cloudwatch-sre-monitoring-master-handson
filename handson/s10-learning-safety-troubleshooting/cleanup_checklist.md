# クリーンアップチェックリスト

## 削除確認

| 対象 | 確認場所 | 期待状態 | 結果 | 証跡 |
| --- | --- | --- | --- | --- |
| CloudFormation Stack | CloudFormation console / CLI | DELETE_COMPLETEまたは対象なし |  |  |
| SNS Topic / Subscription | SNS console | 教材用通知先なし |  |  |
| CloudWatch Dashboard | CloudWatch console | 教材用Dashboardなし |  |  |
| Log Group | CloudWatch Logs | 教材用Log Groupなし、または保持理由あり |  |  |
| Alarm / Composite Alarm | CloudWatch Alarms | 教材用Alarmなし |  |  |
| Custom Metric | CloudWatch Metrics | 新規送信停止を確認 |  |  |
| Billing | Billing console | 反映遅れを前提に確認 |  |  |

## 学習メモ

- 作れたもの:
- 詰まった場所:
- 削除確認結果:
- 費用確認結果:
- 次回改善:

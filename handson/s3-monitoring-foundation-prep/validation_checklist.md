# 初期状態チェック

## Dashboard

| 確認項目 | 期待結果 | 実結果 | 証跡 |
| --- | --- | --- | --- |
| Dashboardが表示できる | `c002-cw-sre-checkout-api-overview` が開ける |  |  |
| RequestCountが表示される | `120` 付近のデータ点が見える |  |  |
| ErrorCountが表示される | `5` 付近のデータ点が見える |  |  |
| LatencyP95が表示される | `950` 付近のデータ点が見える |  |  |

## Alarm

| 確認項目 | 期待結果 | 実結果 | 証跡 |
| --- | --- | --- | --- |
| HighErrorRateAlarm | OKまたはALARMとして存在する |  |  |
| HighLatencyAlarm | OKまたはALARMとして存在する |  |  |
| CompositeServiceAlarm | Metric Alarmを参照して存在する |  |  |
| SNS Topic | 教材用Topicが存在する |  |  |

## Logs Insights

| 確認項目 | 期待結果 | 実結果 | 証跡 |
| --- | --- | --- | --- |
| ERROR抽出 | 2件以上のERRORログを確認できる |  |  |
| status別集計 | 200、500、502の件数を確認できる |  |  |
| Runbook用集計 | pathとerror別に件数を説明できる |  |  |

## 学習メモ

- どの指標で気づいたか:
- どのログで原因仮説を作ったか:
- 通知するべき条件:
- Dashboardで見るだけにする条件:
- Runbookへ転記するクエリ:

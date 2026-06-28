# Section 8 Handson: Error Budget and Burn Rate Design

このディレクトリは、Section 8 の演習用メモです。AWSリソースは作成しません。

## 目的

- SLO目標値からエラーバジェットを計算する
- 分母、許容失敗数、実際の失敗数、残り予算を説明できる
- バーンレート通知の短期窓、長期窓、重要度、通知先、Runbookを設計する

## ファイル

- [`error_budget_sheet.csv`](error_budget_sheet.csv): エラーバジェット計算シート
- [`burn_rate_alarm_design.md`](burn_rate_alarm_design.md): バーンレート通知設計シート

## 注意

この演習は設計演習です。CloudWatch AlarmやApplication Signals SLOを実際に作成する場合は、対象アカウント、リージョン、料金、削除手順を事前に確認してください。

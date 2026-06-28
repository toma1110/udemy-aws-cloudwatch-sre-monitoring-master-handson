# Section 9 Handson: Runbook and First Response

このディレクトリは、Section 9 の演習用テンプレートです。AWSリソースは作成しません。

## 目的

- アラート後5分、15分、30分の行動をRunbookに落とす
- 影響確認、証跡保存、調査、暫定対応、復旧確認、エスカレーションを整理する
- 実行後レビューで不足手順と自動化候補を改善バックログへ入れる

## ファイル

- [`five_minute_first_response_runbook.md`](five_minute_first_response_runbook.md): 5分初動Runbook
- [`fifteen_minute_investigation_runbook.md`](fifteen_minute_investigation_runbook.md): 15分調査Runbook
- [`thirty_minute_decision_runbook.md`](thirty_minute_decision_runbook.md): 30分判断Runbook
- [`runbook_review_sheet.md`](runbook_review_sheet.md): Runbookレビューシート

## 注意

この演習は運用設計演習です。CloudWatch Alarmや通知先を実際に変更する場合は、対象アカウント、通知影響、料金、削除手順を事前に確認してください。

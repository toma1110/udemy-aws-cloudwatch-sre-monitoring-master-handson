# Supplemental Handson: AWS学習安全とトラブルシュート

このディレクトリは、Section 1、Section 3、各AWS操作前後で参照する安全確認テンプレートです。AWSリソースは作成しません。

## 目的

- ハンズオン前に課金、安全、リージョン、削除手順を確認する
- CloudFormation rollback、IAM AccessDenied、失敗時の証跡保存を手順化する
- 学習環境を安全に閉じたことを確認できる形にする

## ファイル

- [`cost_safety_checklist.md`](cost_safety_checklist.md): コスト安全チェック
- [`rollback_event_reading_sheet.md`](rollback_event_reading_sheet.md): rollback読解演習
- [`access_denied_triage_sheet.md`](access_denied_triage_sheet.md): AccessDenied切り分けシート
- [`evidence_capture_template.md`](evidence_capture_template.md): 証跡保存テンプレート
- [`cleanup_checklist.md`](cleanup_checklist.md): クリーンアップチェックリスト

## 注意

この演習は安全確認とトラブルシュート設計です。AWSリソースの作成、更新、削除、予算通知の変更、IAM権限変更は、対象アカウント、料金、影響範囲、削除手順を確認してから実行してください。

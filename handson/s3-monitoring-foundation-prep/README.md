# Section 3: 監視基盤ハンズオンの準備

このディレクトリは、旗艦講座のSection 3で使う教材環境の作成、サンプル投入、初期確認、削除確認のSource of Truthです。

## Purpose

後続のMetrics、Logs、Alarm、Runbook、Capstone演習に入る前に、教材環境の作成物、確認順序、削除条件を先にそろえる。

この章はCloudFormationの文法解説章ではない。CloudFormationは受講者が同じ教材環境を再現しやすくするための手段として扱う。

## Files

| Path | Role |
| --- | --- |
| `cloudformation/cloudwatch_sre_monitoring_lab.yaml` | Log Group、SNS Topic、Metric Alarm、Composite Alarm、Dashboardを作る教材用CloudFormationテンプレート |
| `setup_commands.md` | 作成前確認、CloudFormation deploy、サンプルログ投入、Custom Metrics投入、Alarm確認のPowerShell手順 |
| `sample-data/custom_metrics.json` | `aws cloudwatch put-metric-data` で投入するサンプルメトリクス |
| `logs_insights_queries.md` | ERROR抽出、parse、stats、Runbook用集計のLogs Insightsクエリ |
| `validation_checklist.md` | Dashboard、Alarm、Logs Insightsの初期状態チェック表 |
| `cleanup_commands.md` | CloudFormation delete-stackと削除後確認コマンド |

## Lectures

| Lecture | Title | Role |
| --- | --- | --- |
| `s3-l1` | 統合ハンズオンで作る監視運用の全体像 | 作成する監視部品と初動対応での使い方をつなげる |
| `s3-l2` | 教材環境をREADME通りに準備する | リージョン、認証情報、作成物、削除手順を先に確認する |
| `s3-l3` | サンプルログとメトリクスを投入する | 後続演習で使うログとCustom Metricsの意味を確認する |
| `s3-l4` | DashboardとAlarmの初期状態を確認する | 正常時のDashboard、Alarm、SNS、Log Groupを確認する |
| `s3-l5` | 通知先とRunbookリンクを確認する | Owner、Severity、通知先、Runbookリンクを運用部品として扱う |
| `s3-l6` | 削除漏れを防ぐ終了条件を先に読む | Stack、Dashboard、Alarm、SNS、Log Group、料金画面の終了条件を確認する |

## Standard Flow

1. READMEを通して読む
2. リージョン、プロファイル、Stack名、Prefixを決める
3. 作成物一覧を確認する
4. `setup_commands.md` の作成前確認を実行する
5. CloudFormationテンプレートをvalidateする
6. 教材環境を作成する
7. サンプルログとメトリクスを投入する
8. `logs_insights_queries.md` のクエリでログを確認する
9. `validation_checklist.md` でDashboard、Alarm、SNS、Log Groupの初期状態を確認する
10. 後続章へ進む前に、`cleanup_commands.md` と削除予定時刻を確認する

## Cleanup Checklist

- CloudFormation stackが削除済みである
- Dashboardが残っていない、または教材用として残す理由が記録されている
- AlarmとComposite Alarmが残っていない
- SNS TopicとSubscriptionが残っていない
- Log Groupの保持期間または削除方針が確認されている
- Custom Metricsの履歴表示と稼働リソースを混同していない
- Billing / Cost Explorerの確認予定日が記録されている

## Cost And Safety Notes

- CloudWatch Logs、Custom Metrics、Alarm、Dashboard、SNSには料金が発生する場合があります。
- Logs Insightsは対象Log Groupと時間範囲を絞って実行します。
- メール通知を使う場合、SNS確認メールの承認操作が必要です。
- 本番アカウント、会社アカウント、既存本番通知先では実行しません。
- AWSリソースを作成した場合は、同じ学習セッション内で `cleanup_commands.md` を実行します。

## Production Notes

- 新S3の台本ドラフトは `scripts/rebalanced/s3-handson-prep/` に配置する
- 新S3のGPT-Image2プロンプトは `slides/rebalanced/s3-handson-prep/` に配置する
- 最終制作時に本番 `scripts/s3-l*_script.*` へ昇格する
- 既存の旧S3は新S4へ再利用するため、昇格前にリネーム方針を確認する

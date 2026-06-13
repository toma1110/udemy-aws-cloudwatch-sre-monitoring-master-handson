# AWS CloudWatch/SRE監視実践マスター ハンズオン

このディレクトリは、講座内で配布するハンズオン素材のSource of Truthです。学習者は、AWSリソースを作る前にこのREADMEで目的、作成物、料金注意、削除手順を確認します。

## 目的

- CloudWatch Logs、Metrics、Alarm、Dashboard、Runbook、SLI/SLOを一続きの監視運用として扱う
- 教材環境の作成前に、リージョン、権限、料金、通知先、削除条件を確認する
- Logs Insights、アラート設計、Runbook、SLOレビューをテンプレートに沿って練習する
- 実運用ではCDKまたはTerraformへ移す前提で、教材用CloudFormationを再現手段として使う

## 前提

- AWSアカウント、AWS CLI、対象リージョン、実行プロファイルを自分で確認できること
- CloudWatch Logs、CloudWatch Metrics、CloudWatch Alarm、SNS、Dashboardの名前を聞いたことがあること
- AWSリソース作成前に、削除手順と料金発生ポイントを先に読めること
- 本番環境や会社アカウントで試す場合は、所属組織の変更管理と権限ルールを優先すること

## 教材一覧

| 最終カリキュラム | リソース | 役割 |
| --- | --- | --- |
| Section 3 | `s3-monitoring-foundation-prep/` | 統合ハンズオンの準備、作成物、初期確認、削除条件 |
| Section 8 | `s8-error-budget-burn-rate/` | エラーバジェット計算とバーンレート通知設計 |
| Section 9 | `s9-runbook-first-response/` | 5分、15分、30分の初動Runbookとレビュー |
| Section 10 | `s11-capstone-monitoring-operations/` | Capstone要求定義、SLI、Logs Insights、監視設計、インシデント演習、SLOレビュー |
| 補助教材 | `s10-learning-safety-troubleshooting/` | 課金安全、AccessDenied、rollback、証跡保存、cleanup確認 |

`s10-learning-safety-troubleshooting/` と `s11-capstone-monitoring-operations/` は過去の制作時フォルダ名を保持しています。Udemy上の最終対応は上の表をSource of Truthにします。

## セットアップ

1. `curriculum.md` で対象レクチャーとハンズオンリソースタイトルを確認する。
2. このREADMEと対象ディレクトリのREADMEを最後まで読む。
3. AWS操作を伴う場合は、リージョン、プロファイル、Stack名、Prefix、通知先メール、削除予定時刻を記録する。
4. `s10-learning-safety-troubleshooting/cost_safety_checklist.md` を作業前に記入する。
5. AWSリソース作成を行う場合は、教材用アカウントで実施し、本番環境に適用しない。

## 実施手順

### Section 3 統合ハンズオン準備

1. `s3-monitoring-foundation-prep/README.md` で作成物と削除条件を確認する。
2. `s3-monitoring-foundation-prep/setup_commands.md` に沿って作成前確認とテンプレート検証を行う。
3. 実際にAWSリソースを作成する場合は、`s3-monitoring-foundation-prep/cloudformation/cloudwatch_sre_monitoring_lab.yaml` だけを使う。
4. サンプルログ、Custom Metrics、Dashboard、Alarm、SNS、Runbookリンクの役割を整理する。
5. `s3-monitoring-foundation-prep/logs_insights_queries.md` と `validation_checklist.md` で初期状態を確認する。
6. 演習後は `s3-monitoring-foundation-prep/cleanup_commands.md` で削除確認を行う。

### Section 8 SLI/SLO演習

1. `s8-error-budget-burn-rate/error_budget_sheet.csv` にSLO目標、測定期間、失敗数を入力する。
2. `s8-error-budget-burn-rate/burn_rate_alarm_design.md` に短期窓、長期窓、重要度、通知先、Runbookを記入する。
3. CloudWatch AlarmやApplication Signals SLOを実作成する場合は、料金と削除条件を事前確認する。

### Section 9 Runbook演習

1. `s9-runbook-first-response/five_minute_first_response_runbook.md` に通知直後の確認事項を書く。
2. `s9-runbook-first-response/fifteen_minute_investigation_runbook.md` にLogs Insights、Dashboard、Application Signalsの調査順を書く。
3. `s9-runbook-first-response/thirty_minute_decision_runbook.md` に暫定対応、告知、rollback、継続監視の判断条件を書く。
4. `s9-runbook-first-response/runbook_review_sheet.md` で不足手順と自動化候補を整理する。

### Section 10 Capstone

1. `s11-capstone-monitoring-operations/capstone_requirements.md` でサンプルサービスの要求を定義する。
2. `s11-capstone-monitoring-operations/sli_design_worksheet.md` でSLI候補と最初のSLO案を作る。
3. `s11-capstone-monitoring-operations/logs_insights_exercise.md` で調査クエリを整理する。
4. `s11-capstone-monitoring-operations/monitoring_design_sheet.md` にAlarm、Dashboard、Runbookを統合する。
5. `s11-capstone-monitoring-operations/incident_simulation_sheet.md` と `slo_review_minutes.md` で障害調査とレビューを一周する。

## 期待結果

- 作成前に料金、リージョン、権限、削除手順を説明できる
- Logs Insights、Alarm、Dashboard、Runbook、SLI/SLOを別々ではなく運用の流れとして接続できる
- アラート後5分、15分、30分で何を見るかをテンプレートに記録できる
- Capstoneで要求定義からSLOレビューまでの監視運用設計を説明できる

## クリーンアップ

AWSリソースを作成した場合は、演習後に次を確認します。

- CloudFormation Stackが削除済み、または保持理由が記録されている
- SNS TopicとSubscriptionが残っていない
- CloudWatch Dashboardが残っていない、または保持理由が記録されている
- AlarmとComposite Alarmが残っていない
- Log Groupの保持期間または削除方針が確認済み
- Custom Metricsは履歴表示と稼働リソースを混同していない
- BillingまたはCost Explorerの確認予定日を記録している

## コスト注意

- CloudWatch Logs、Logs Insights、Custom Metrics、Alarm、Dashboard、SNS、Application Signals、SLOには料金が発生する場合があります。
- Logs Insightsは対象Log Groupと時間範囲を必ず絞ります。
- SNS通知先メール、Application Signals、SLO、Custom Metricsは作成数と使用量に注意します。
- 本READMEの静的ワークシート演習だけならAWSリソースは作成しません。

## トラブルシュート

- AccessDeniedが出る場合は `s10-learning-safety-troubleshooting/access_denied_triage_sheet.md` に主体、Action、Resource、Conditionを分解します。
- CloudFormation rollbackが起きた場合は `rollback_event_reading_sheet.md` で最初の失敗イベントを確認します。
- 何を実行したか分からなくなった場合は `evidence_capture_template.md` に時刻、リージョン、期待結果、実結果を記録します。
- 削除確認は `cleanup_checklist.md` を使い、確認できないリソースを残したまま作業完了にしません。

## Public Repository

公開配布用リポジトリは未作成です。Phase4時点では、この `handson/` 配下をprivate Source of Truthとしてステージし、公開作成やvisibility変更は別Issueの明示承認後に行います。

# AWS CloudWatch/SRE監視実践マスター ハンズオン

このリポジトリは、Udemy講座「AWS CloudWatch/SRE監視実践マスター: Logs Insights・Alarm・SLO・運用改善」の受講者向けハンズオン教材です。

## 内容

- [`handson/s3-monitoring-foundation-prep/`](handson/s3-monitoring-foundation-prep/): CloudFormationでCloudWatch Logs、Custom Metrics、Alarm、Composite Alarm、Dashboardを作成する実行ハンズオン
- [`handson/s8-error-budget-burn-rate/`](handson/s8-error-budget-burn-rate/): エラーバジェットとBurn Rateの演習
- [`handson/s9-runbook-first-response/`](handson/s9-runbook-first-response/): 初動Runbook演習
- [`handson/s10-learning-safety-troubleshooting/`](handson/s10-learning-safety-troubleshooting/): 学習安全性とトラブルシュート演習
- [`handson/s10-integrated-monitoring-operations/`](handson/s10-integrated-monitoring-operations/): 監視運用の総合演習
- [`evidence/aws-execution-20260613-125703/`](evidence/aws-execution-20260613-125703/): 教材検証時のAWS実行・削除確認の証跡

## AWS利用時の注意

AWSリソースを作成するハンズオンは、短時間で削除する前提です。作業後は必ず [`handson/s3-monitoring-foundation-prep/cleanup_commands.md`](handson/s3-monitoring-foundation-prep/cleanup_commands.md) と [`handson/s10-learning-safety-troubleshooting/cleanup_checklist.md`](handson/s10-learning-safety-troubleshooting/cleanup_checklist.md) を確認し、CloudFormationスタック、ロググループ、ダッシュボード、アラーム、SNS Topicが残っていないことを確認してください。

CloudWatchのカスタムメトリクス履歴は、リソース削除後もしばらく参照できる場合があります。これは稼働中リソースではありません。

## 実行証跡

教材検証時の確認では、`ap-northeast-1` に一時CloudFormationスタックを作成し、ログ投入、メトリクス投入、Logs Insights、アラーム、ダッシュボードを確認したうえで、同一実行内でスタック削除と削除後チェックを完了しています。

証跡は [`evidence/aws-execution-20260613-125703/`](evidence/aws-execution-20260613-125703/) にあります。公開用証跡ではAWSアカウントIDとIAMユーザー情報をマスクしています。

## ライセンス

この教材はUdemy講座の受講者が個人学習のために利用する前提で公開しています。再配布、販売、講座・研修への組み込み利用は許可していません。詳細は [`LICENSE`](LICENSE) を確認してください。

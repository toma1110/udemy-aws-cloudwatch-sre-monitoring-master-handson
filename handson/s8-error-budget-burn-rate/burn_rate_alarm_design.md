# Burn Rate Alarm Design Sheet

| 項目 | 記入例 |
| --- | --- |
| 対象サービス | checkout-api |
| 対象SLI | 主要リクエストの成功率 |
| SLO目標 | 99.9% |
| 測定期間 | 30日 |
| 短期窓 | 5分 |
| 長期窓 | 1時間 |
| 緊急通知条件 | 短期Burn Rateが高く、同時に長期でも悪化している |
| レビュー条件 | 長期Burn Rateが継続して高い |
| 重要度 | Sev2 / Sev3 |
| 通知先 | on-call / reliability-review |
| Runbook | 影響範囲、直近変更、依存先、ログクエリを確認する |
| 削除・停止確認 | 演習後に作成したAlarmがあれば削除する |

# Logs Insightsクエリ集

対象Log Group:

```text
/c002-cw-sre/application
```

検索時間範囲は、サンプルログ投入後の直近15分から始めます。不要な料金を避けるため、広い時間範囲で検索しないでください。

## フィールド確認

```sql
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

期待結果:

- 投入したINFO/ERRORログを時系列で確認できる

## ERRORログ抽出

```sql
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

期待結果:

- `payment-timeout` と `upstream-bad-gateway` を含むログを確認できる

## JSONログのparse

```sql
fields @timestamp, @message
| parse @message '"service":"*"' as service
| parse @message '"path":"*"' as path
| parse @message '"status":*,' as status
| parse @message '"latency_ms":*,' as latency_ms
| display @timestamp, service, path, status, latency_ms
| sort @timestamp desc
| limit 20
```

期待結果:

- service、path、status、latency_msを列として確認できる

## status別集計

```sql
fields @timestamp, @message
| parse @message '"status":*,' as status
| stats count(*) as count by status
| sort count desc
```

期待結果:

- 200、500、502の件数を比較できる

## Runbook用の初動クエリ

```sql
fields @timestamp, @message
| filter @message like /ERROR/
| parse @message '"path":"*"' as path
| parse @message '"error":"*"' as error
| stats count(*) as errors by path, error
| sort errors desc
```

期待結果:

- どのpathでどのerrorが出ているかをRunbookに転記できる

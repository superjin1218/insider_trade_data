# InsiderPulse — daily Form 4 archive

Auto-pushed daily by the InsiderPulse Fly.io machine at ~03:30 UTC (12:30 KST,
after US market close + Form 4 settle time). One commit per US trading day.

## Layout

```
csv/
  insider_buys_365d.csv     all buys (P-code) in the last rolling 365 days
  daily/                    one CSV per trading day, all P-code transactions
  by_ticker/                one CSV per active ticker, full 1-year insider history
```

## Source

- SEC EDGAR Form 4 (XML, parsed)
- Active US-listed common stocks: NASDAQ + NYSE + AMEX
- 1-year rolling window
- Code-paths in [insider-pulse](https://github.com/superjin1218) (private)

## Schema (highlights)

| Column | Meaning |
|---|---|
| `filing_date` | when the Form 4 hit EDGAR |
| `transaction_date` | when the trade actually happened |
| `ticker`, `cik`, `company_name` | issuer identifiers |
| `transaction_code` | `P`=open-market buy (the only thing this archive keeps) |
| `shares`, `price_per_share`, `value_usd` | sized in USD |
| `market_cap_usd` | snapshot from yfinance at ingest time |
| `value_to_market_cap_ratio` | the headline metric — buy size relative to float |
| `sic_code`, `sic_label`, `sic_major_group` | SEC industry classification |

## Why a separate repo?

The Fly volume can be lost (machine recreation, region migration, or just
fat-fingering `fly volumes destroy`). Keeping the CSV exports in git means
the archive is durable, auditable, and trivially re-importable on a fresh
deploy via `git clone` — no DuckDB backups, no SFTP juggling.

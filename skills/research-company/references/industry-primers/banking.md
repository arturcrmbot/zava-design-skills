# Industry Primer — Banking (FSI)

> **Canon — see [AGENTS.md § 2.2](../../../../AGENTS.md#22--reference-data-files-are-canon--do-not-normalize).**
> *Stub — to be expanded next time `research-company` is run against a bank target.*

## Sub-segments

- Universal / G-SIB (HSBC, JPMC, Citi, Santander, Deutsche, BNP, UBS)
- Retail / high-street (Lloyds, RBS, BBVA, ING)
- Investment-only (Goldman, Morgan Stanley, Jefferies)
- Challenger / neobank (Monzo, Starling, Revolut, N26, NuBank)
- Custodian (BNY Mellon, State Street, Northern Trust)

## Canonical vertical entity kinds

`Product`, `Cohort`, `Mandate`, `TreasuryBook`, `Counterparty`,
`Branch`, `Mortgage`, `Card`, `Position`, `Limit`, `Account`, `Trade`,
`Settlement`.

## Canonical regulators

| id | country | domain |
|---|---|---|
| `pra` | GB | banking |
| `fca` | GB | finance |
| `ecb-ssm` | EU | banking |
| `bafin` | DE | banking |
| `acpr` | FR | banking |
| `finma` | CH | banking |
| `fed` | US | banking |
| `occ` | US | banking |
| `sec` | US | securities |
| `finra` | US | securities |
| `hkma` | HK | banking |
| `mas` | SG | banking |

## Canonical KPIs

`cet1_ratio`, `nim`, `cost_income_ratio`, `npl_ratio`, `lcr`, `nsfr`,
`fraud_loss_bps`, `aml_alert_volume`, `kyc_cycle_days`.

## TODO

Expand on first bank run.

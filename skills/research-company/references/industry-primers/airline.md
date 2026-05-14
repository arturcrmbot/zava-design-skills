# Industry Primer — Airline

> **Canon — see [AGENTS.md § 2.2](../../../../AGENTS.md#22--reference-data-files-are-canon--do-not-normalize).**
> *Stub — to be expanded next time `research-company` is run against an airline target.*

## Sub-segments

| Sub-segment | Examples | Notes |
|---|---|---|
| **Legacy / network carrier** | British Airways, Lufthansa, Air France-KLM, Delta, ANA | Hub-and-spoke, alliance member. |
| **Low-cost carrier (LCC)** | Ryanair, easyJet, Wizz Air, Southwest | Point-to-point, secondary airports, ancillaries. |
| **Ultra low-cost (ULCC)** | Spirit, Frontier, Volaris | Unbundled to the maximum. |
| **Charter / leisure** | TUI, Jet2 | Tour-operator-led. |
| **Cargo-only** | Cargolux, Atlas Air | No passengers. |
| **Regional** | Loganair, Aurigny | Sub-200 routes. |

## Canonical vertical entity kinds

`Route`, `Sector`, `Slot`, `Roster`, `Aircraft`, `Crew`, `Pairing`,
`Booking`, `Bay`, `Gate`, `MROOrder`, `Spare`, `FuelHedge`, `Licence`,
`FrequentFlyer`.

## Canonical regulators

| id | country | domain |
|---|---|---|
| `caa-uk` | GB | aviation |
| `easa` | EU | aviation |
| `faa` | US | aviation |
| `caas` | SG | aviation |
| `dgca-in` | IN | aviation |
| `casa` | AU | aviation |
| `anac-br` | BR | aviation |

## Canonical KPIs

`otp_d0`, `load_factor`, `ask_growth`, `rask`, `cancellation_rate`,
`mro_cycle_time`, `crew_utilisation`, `slot_compliance`, `nps`.

## TODO

Expand on first airline run (likely candidates: Ryanair, easyJet,
British Airways).

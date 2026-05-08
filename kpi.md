# .claude/rules/kpi.md — KPI Operator Rules

## Clinic KPI Benchmarks
- SAW (Signed Annual Wellness): Benchmark = 91 | Critical below: 80 | Direction: Higher is better
- Daily NV Target: 18.2 new visits/day (calculated as SAW / operating days in tracking period)
- Operating days: 6 per week (Mon-Sat), closed Sunday

## The 13 KPIs

| KPI | Formula | Direction | Critical Flag Threshold |
|-----|---------|-----------|------------------------|
| SAW | Count of active annual wellness agreements | Higher | < 80 |
| NV_TODAY | New visits today | Higher | < 10 |
| NV_WEEK | New visits this week | Higher | < 50 |
| NV_MONTH | New visits this month | Higher | < 180 |
| NV_PACE | (NV_MONTH / operating_days_elapsed) | Higher | < 14 |
| FORECAST | NV_PACE x operating_days_in_month | Higher | < SAW - 15 |
| CONVERSION_RATE | SAW signed / NV_MONTH | Higher | < 0.30 |
| RETENTION_RATE | Active members month-over-month | Higher | < 0.85 |
| ATTRITION | Members cancelled this month | Lower | > 8 |
| ARV | Average revenue per visit | Higher | < 35.00 |
| COLLECTIONS | Total collections this month | Higher | < $12,000 |
| MEMBER_COUNT | Total active wellness plan members | Higher | < 85 |
| DROP_INS | Non-member visits this month | Higher | N/A |

## Metric Direction Logic
- For metrics where "higher is better": Flag when actual < threshold
- For metrics where "lower is better": Flag when actual > threshold
- CRITICAL flag should be bold/highlighted in all pre-huddle outputs
- Pre-huddle tables show: KPI | Target | Actual | Pace% | Status (CRITICAL/OK/AT RISK)

## Axis Data Notes
- Axis is the clinic management system. Exports are CSV format.
- KPI calculations use Axis data as source of truth.
- Date parsing: Axis exports use MM/DD/YYYY format.
- NV counts: Use "New Patient" visit type only — excludes re-activations unless explicitly noted.
- SAW count: Live from Axis, not a calculation. Use the number as reported.
- ARV calculation: Total collections / total visits (all types).

## Pace Calculation
pace_pct = (nv_actual / (nv_target_daily * operating_days_elapsed)) * 100
- At pace = 100%
- Ahead of pace > 100%
- Behind pace < 100%
- Critical threshold: pace_pct < 75% triggers CRITICAL flag

## Forecast Calculation
forecast = nv_pace_daily * operating_days_remaining_in_period
- Where nv_pace_daily = nv_actual / operating_days_elapsed
- Round down to nearest integer for conservative estimate

## Pre-Huddle Table Format
Output should be formatted as a markdown table with columns:
KPI | Target | Actual | Pace% | Status

Status values: CRITICAL | AT RISK | ON PACE | AHEAD
- CRITICAL: pace < 75% or below critical flag threshold
- AT RISK: pace 75-89%
- ON PACE: pace 90-105%
- AHEAD: pace > 105%

## Critical Flag Actions
When a KPI is CRITICAL, the output must include:
1. The flagged KPI in bold in the table
2. A "Critical Items" section below the table
3. Specific action recommendation for the WC

## Data Freshness
Always note the data timestamp in KPI output headers.
If data is >24 hours old, flag as STALE and note the staleness.

# Salary Projection Methodology

This document explains the calculation logic used by the CT Salary Dashboard.

## Purpose

The dashboard estimates salary progression when recurring salary increases are applied over time. It also estimates gross retroactive pay when one or more increases should have taken effect earlier than they were reflected in payroll.

The model is designed for planning and analysis. It is not an official payroll system and should not be treated as a payroll determination.

## Core Inputs

| Input | Description |
|---|---|
| Current annual salary | The salary before the delayed COLA and STEP increases are applied. |
| COLA rate | The cost-of-living adjustment percentage. |
| STEP rate | The standard pay increase percentage. |
| First delayed COLA effective date | The first date on which the COLA should have applied. |
| First delayed STEP effective date | The first date on which the STEP should have applied. |
| Corrected salary reflected in payroll | The date the corrected compounded salary is expected to appear in payroll. |
| Retroactive lump-sum payout date | The expected date for retroactive payout. This is displayed for reference. |
| Projection end year | The final year shown in the salary projection. |
| Days per year | Used to estimate retroactive pay by calendar day. The default is 365. |

## Salary Compounding Logic

Salary increases are applied sequentially by effective date.

For each raise event:

```text
salary_after = salary_before × (1 + raise_rate)
```

The next raise is applied to the prior corrected salary, not the original salary.

Example:

```text
Starting salary = $104,000
COLA = 2.5%
STEP = 3.0%

7/1/2025 COLA:
$104,000 × 1.025 = $106,600

1/1/2026 STEP:
$106,600 × 1.03 = $109,798
```

## Raise Schedule Logic

The model currently assumes:

- COLA occurs annually in July.
- STEP occurs annually in January.
- The first eligible COLA and STEP dates can be adjusted by the user.
- Events are sorted chronologically before calculation.

This allows the dashboard to support future salary projections without requiring each row to be manually entered.

## Retroactive Pay Logic

The retroactive estimate compares the original annual salary against the corrected annual salary that should have applied during each retroactive period.

The retroactive period ends the day before the corrected salary is reflected in payroll.

```text
retro_end_date = corrected_payroll_date - 1 day
```

For each eligible retroactive segment:

```text
annual_difference = corrected_annual_salary - original_annual_salary
retro_gross = annual_difference ÷ days_per_year × eligible_days
```

Example with a $104,000 starting salary:

| Period | Corrected Salary | Original Salary | Annual Difference | Days | Estimated Gross Retro |
|---|---:|---:|---:|---:|---:|
| 7/1/2025–12/31/2025 | $106,600 | $104,000 | $2,600 | 184 | $1,310.68 |
| 1/1/2026–6/11/2026 | $109,798 | $104,000 | $5,798 | 162 | $2,573.36 |
| Total |  |  |  | 346 | $3,884.04 |

## Why Retroactive Pay Is Separated from Salary Projection

The dashboard treats salary projection and retroactive pay as related but separate calculations.

Salary projection answers:

```text
What should the salary be after each compounded increase?
```

Retroactive pay answers:

```text
How much additional gross pay may be owed for prior periods when the corrected salary was not yet reflected in payroll?
```

This separation makes the model easier to audit and improves transparency.

## Limitations

The current model uses a calendar-day estimate. Actual payroll calculations may differ based on:

- official pay-period start and end dates,
- check dates,
- workdays rather than calendar days,
- partial pay-period rules,
- bargaining unit provisions,
- agency or State payroll processing rules,
- deductions and taxes,
- retirement contributions,
- benefit deductions,
- system rounding.

## Recommended Future Enhancement

The most important enhancement would be to add an official pay-period calendar.

A future pay-period table could include:

| Field | Purpose |
|---|---|
| Pay period start date | Identifies the beginning of each pay period. |
| Pay period end date | Identifies the end of each pay period. |
| Pay date | Identifies when the pay period was paid. |
| Actual paid annual salary | Confirms the salary actually used for the paycheck. |
| Corrected annual salary | Calculates what should have been paid. |
| Gross difference | Calculates the retroactive amount by pay period. |

This would allow a more precise retroactive pay estimate and would support a future toggle between:

- calendar-day estimate,
- workday estimate,
- pay-period estimate.

## Disclaimer

This project is an unofficial estimator for planning and informational purposes only. It does not replace official payroll guidance, collective bargaining agreements, agency payroll processing, Core-CT calculations, or human resources determinations.

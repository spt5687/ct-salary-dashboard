# CT Salary Dashboard

A self-contained HTML5 salary projection and retroactive pay estimator for modeling compounded COLA and STEP increases.

## Version 1.1.0 Enhancements

This version adds a paycheck-oriented view alongside the existing annual salary calculations:

- Enter the starting amount as either **annual salary** or **gross bi-weekly pay**.
- Select the number of **pay periods per year**; the default is 26.
- View the equivalent annual or gross bi-weekly value immediately when entering salary.
- View annual and gross bi-weekly amounts before and after every COLA or STEP event.
- View corrected gross bi-weekly pay and gross bi-weekly difference in the retroactive pay breakdown.
- Retain the **Type** field in both tables to identify each COLA or STEP event.

## Overview

The dashboard allows a user to model:

- COLA increases
- STEP increases
- Compounded salary growth over time
- Corrected salary after delayed increases are implemented
- Estimated gross retroactive pay
- Future projected salary through a selected end year
- Gross bi-weekly pay for each projected salary step

The current version is a single-file static web application contained in `index.html`. It does not require a backend, database, build process, package manager, or external JavaScript libraries.

## Current Default Assumptions

| Input | Default Value |
|---|---:|
| Starting salary | $100,000 annually |
| Starting gross bi-weekly pay equivalent | $3,846.15 |
| Salary input method | Annual salary |
| Pay periods per year | 26 |
| COLA rate assumption | 2.5% |
| STEP rate assumption | 3.0% |
| First delayed COLA effective date | July 1, 2025 |
| First delayed STEP effective date | January 1, 2026 |
| Corrected salary reflected in payroll | June 12, 2026 |
| Retroactive lump-sum payout date | August 12, 2026 |
| Projection end year | 2028 |
| Days per year | 365 |

## Salary Input Methods

### Annual Salary

Enter the annual gross salary before delayed increases. The dashboard calculates the equivalent gross bi-weekly amount as:

```text
Annual salary ÷ pay periods per year
```

### Gross Bi-Weekly Pay

Enter the gross amount displayed before deductions on a bi-weekly paycheck or paystub. The dashboard annualizes the entry as:

```text
Gross bi-weekly pay × pay periods per year
```

The annualized salary is then used consistently for the projection and retroactive-pay calculations.

## How the Retroactive Estimate Works

The dashboard calculates retroactive pay by comparing the original annualized salary against the corrected compounded salary that should have applied during each retroactive period.

For example, with a starting salary of $100,000:

1. July 1, 2025 COLA applies: `$100,000 × 1.025 = $102,500`
2. January 1, 2026 STEP applies to the corrected salary: `$102,500 × 1.03 = $105,575`
3. Retroactive pay is estimated for each period before the corrected salary is reflected in payroll.

The current calendar-day method uses:

```text
Annual salary difference ÷ days per year × eligible retroactive days
```

The gross bi-weekly view uses:

```text
Annual salary ÷ selected pay periods per year
```

## Schedule and Retroactive Tables

### Compounded Raise Schedule

The schedule retains the **Type** column to indicate whether each event is a COLA or STEP increase, and displays:

- Annual salary before the increase
- Gross bi-weekly pay before the increase
- Annual increase amount
- Annual salary after the increase
- Gross bi-weekly pay after the increase

### Retroactive Pay Breakdown

The retroactive table retains the **Type** column and displays:

- Retroactive period
- Eligible calendar days
- COLA or STEP type
- Corrected annual salary
- Corrected gross bi-weekly pay
- Gross bi-weekly difference from the original pay
- Estimated gross retroactive pay

CSV exports include both annual and bi-weekly fields for additional detail.

## Features

- Dynamic annual or gross bi-weekly salary entry
- Editable pay periods per year, defaulted to 26
- Dynamic salary, COLA, STEP, date, and projection-year inputs
- Reset defaults button
- Print / Save PDF option
- CSV export for the raise schedule and retroactive pay estimate
- Salary growth chart with hover/focus tooltips
- Compounded raise schedule with annual and gross bi-weekly salary values
- Footer with version, last-updated, and unofficial-estimator language

## Important Disclaimer

This tool is an unofficial salary projection and retroactive pay estimator. It is intended for planning and informational purposes only. Actual payroll results and paycheck amounts may differ based on official payroll rules, pay-period calendars, deductions, taxes, bargaining unit provisions, unpaid time, agency or State payroll processing, and payroll-system-specific rounding.

## Running Locally

Open `index.html` directly in a browser.

No install step is required.

## Publishing with GitHub Pages

This project is designed to work with GitHub Pages.

Suggested setup:

1. Go to repository **Settings**.
2. Open **Pages**.
3. Set the source to deploy from the `main` branch.
4. Select the repository root as the publishing folder.
5. Save the settings.

Once GitHub Pages finishes publishing, the dashboard should be available in a browser through the GitHub Pages URL.

## Project Structure

```text
ct-salary-dashboard/
├── index.html
├── README.md
└── docs/
    └── methodology.md
```

## Recommended Next Accuracy Enhancement

The next meaningful accuracy enhancement would be official pay-period calendar support. This would allow the tool to estimate retroactive pay by exact pay period rather than calendar day.

## Development Notes

The current dashboard intentionally keeps all HTML, CSS, and JavaScript in one file. This makes it simple to test, host, archive, and share. If the project expands, the next logical step would be to split the app into:

```text
assets/css/styles.css
assets/js/app.js
index.html
```

For now, the single-file approach is the simplest and most portable option.

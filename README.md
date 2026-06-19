# CT Salary Dashboard

A self-contained HTML5 salary projection and retroactive pay estimator for modeling compounded COLA and STEP increases.

## Overview

This dashboard allows a user to enter a current annual salary and model:

- COLA increases
- STEP increases
- Compounded salary growth over time
- Corrected salary after delayed increases are implemented
- Estimated gross retroactive pay
- Future projected salary through a selected end year
- Estimated gross bi-weekly pay for each projected salary step

The current version is a single-file static web application contained in `index.html`. It does not require a backend, database, build process, package manager, or external JavaScript libraries.

## Current Default Assumptions

| Input | Default Value |
|---|---:|
| Starting salary | $100,000 |
| COLA rate assumption | 2.5% |
| STEP rate assumption | 3.0% |
| First delayed COLA effective date | July 1, 2025 |
| First delayed STEP effective date | January 1, 2026 |
| Corrected salary reflected in payroll | June 12, 2026 |
| Retroactive lump-sum payout date | August 12, 2026 |
| Projection end year | 2028 |
| Days per year | 365 |

## How the Retroactive Estimate Works

The dashboard calculates retroactive pay by comparing the original salary against the corrected compounded salary that should have applied during each retroactive period.

For example, with a starting salary of $100,000:

1. July 1, 2025 COLA applies: `$100,000 × 1.025 = $102,500`
2. January 1, 2026 STEP applies to the corrected salary: `$102,500 × 1.03 = $105,575`
3. Retroactive pay is estimated for each period before the corrected salary is reflected in payroll.

The current method uses a calendar-day estimate:

```text
Annual salary difference ÷ days per year × eligible retroactive days
```

The gross bi-weekly pay estimate uses:

```text
Corrected annual salary ÷ 26 pay periods
```

## Features

- Dynamic salary, COLA, STEP, date, and projection-year inputs
- Reset defaults button
- Print / Save PDF option
- CSV export for the raise schedule
- CSV export for the retroactive pay estimate
- Salary growth chart with hover/focus tooltips
- Compounded raise schedule with annual salary and gross bi-weekly pay
- Footer with last-updated and unofficial-estimator language

## Important Disclaimer

This tool is an unofficial salary projection and retroactive pay estimator. It is intended for planning and informational purposes only. Actual payroll results may differ based on official payroll rules, pay-period calendars, deductions, taxes, bargaining unit provisions, agency or State payroll processing, and payroll-system-specific rounding.

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

## Planned Enhancements

Potential future improvements include:

- Official pay-period calendar support
- Workday-based retroactive calculation option
- Save/load scenarios
- Multiple salary comparison scenarios
- More detailed methodology notes
- Separate CSS and JavaScript files if the project grows beyond a single-file prototype

## Development Notes

The current dashboard intentionally keeps all HTML, CSS, and JavaScript in one file. This makes it simple to test, host, archive, and share. If the project expands, the next logical step would be to split the app into:

```text
assets/css/styles.css
assets/js/app.js
index.html
```

For now, the single-file approach is the simplest and most portable option.

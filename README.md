# Banking Dashboard

A lightweight, interactive dashboard for monitoring and analysing a bank's loan and deposit portfolios. This project focuses on two primary analysis areas:

- Loan — performance, concentration, risk, and repayment behaviour.
- Deposit — balances, inflows/outflows, account segmentation, and liquidity.

Use this README to understand the main capabilities, recommended KPIs and visualizations, data schema conventions, and quick guidance for operating the dashboard.

---

## Key features

- Overview page with high-level KPIs for loans and deposits.
- Loan analysis: portfolio composition, delinquency & NPA trends, exposure by product and customer segment.
- Deposit analysis: balance trends, net flows, account growth and concentration.
- Time-series charts, cohort/ageing analysis, and drill-down filters (branch, product, customer segment, tenor).
- Exportable charts and CSV export of filtered data for further analysis.

---

## Loan analysis — what to show & why

Primary objective: detect emerging credit risk, concentration issues, and measure lending performance.

Important KPIs
- Total outstanding loan principal
- Number/value of active loans
- Average outstanding per borrower
- Non-performing loans (NPL) count & NPL ratio (NPLs / total outstanding)
- Delinquency buckets (30/60/90/120+ days)
- New loans originated (period)
- Loan loss provisions & coverage ratio
- Interest income (period)

Recommended visualizations
- Time-series: total outstanding and new originations by month.
- Stacked bar or sunburst: loan portfolio by product (home, auto, personal, SME).
- Heatmap / table: delinquency ageing buckets by product or branch.
- Pareto chart: top borrowers/branches by exposure (identify concentration).
- Cohort chart: performance (default & delinquency) by origination month.

Interpretation tips
- Watch growing share of 90+/120+ day loans and rising provision requirements.
- Rapid growth concentrated in a few branches/products = higher concentration risk.
- Declining originations + stable outstanding may indicate repayment pressures.

---

## Deposit analysis — what to show & why

Primary objective: monitor funding stability, liquidity, and customer behaviour.

Important KPIs
- Total deposit balance
- Net deposits (inflows − outflows) per period
- Number of deposit accounts
- Average balance per account
- Deposit concentration (top 10 accounts / total deposits)
- CASA ratio (Current + Savings / Total deposits)
- Time deposit rollovers & maturity profile

Recommended visualizations
- Time-series: total deposits and net flows by month.
- Waterfall or area chart: inflows and outflows stacked to show net effect.
- Bar chart: deposit balances by product (savings, current, fixed/term deposits).
- Maturity ladder: upcoming maturities by bucket (0–30, 31–90, 91–365, 365+ days).
- Distribution: customer balance distribution (histogram) to identify concentration.

Interpretation tips
- Negative net deposits over consecutive periods may signal funding strain.
- High proportion of term deposits maturing within short window increases rollover risk.
- Low CASA with heavy reliance on term deposits increases funding cost sensitivity.

---

## Suggested dashboard pages / layout

1. Home / Summary
   - Snapshot KPIs: total loans, total deposits, NPL ratio, CASA, net inflow (period).
   - Quick alerts (e.g., NPL > threshold, deposit concentration > threshold).

2. Loans
   - Portfolio composition, delinquency ageing, origination trends, top exposures.

3. Deposits
   - Balance trends, net flows, maturity profile, account growth & concentration.

4. Drill-down
   - Filters by time range, branch, product, customer segment, currency.

5. Exports & Reports
   - Export current filters to CSV / PDF for reporting.

---

## Data schema (recommended CSV / table columns)

Loans table (loans.csv)
- loan_id
- customer_id
- branch_id
- product (e.g., home, auto, personal, SME)
- origination_date
- maturity_date
- principal_amount
- outstanding_balance
- interest_rate
- status (active, closed, default)
- days_past_due
- provision_amount
- currency

Deposits table (deposits.csv)
- account_id
- customer_id
- branch_id
- product (savings, current, term)
- opened_date
- last_activity_date
- balance
- currency
- maturity_date (if term)
- account_status (active, closed)

Transactions table (optional, for flow analysis)
- tx_id
- account_id / loan_id
- date
- type (deposit, withdrawal, repayment, disbursement)
- amount
- channel (branch, internet, ATM)

---

## Example queries / calculations

- NPL ratio = SUM(outstanding_balance WHERE status = 'default') / SUM(outstanding_balance)
- Net deposits (month) = SUM(deposits inflows) - SUM(deposits outflows)
- Delinquency bucket counts:
  - bucket_30 = COUNT(*) WHERE days_past_due BETWEEN 1 AND 30
  - bucket_90 = COUNT(*) WHERE days_past_due BETWEEN 61 AND 90, etc.

Sample SQL (total outstanding by product)
SELECT product, SUM(outstanding_balance) AS total_outstanding
FROM loans
GROUP BY product
ORDER BY total_outstanding DESC;

---

## How to use (quick)

1. Prepare your data files (see "Data schema").
2. Load CSVs or connect to your database in the dashboard configuration.
3. Use time-range and branch/product filters to slice the portfolio.
4. Export snapshots and generate periodic reports from the "Exports" page.

(If this repo contains the dashboard code, add deployment/run instructions here — e.g., "npm install && npm start" or "docker-compose up" — tailored to your stack.)

---

## Suggestions for enhancements

- Add automated alerting (email or Slack) for configurable thresholds (rising NPLs, deposit concentration).
- Add scenario simulations: stress test deposit runoff / loan default rates.
- Integrate customer-level analytics for cross-sell / risk scoring.
- Add authentication and role-based views (branch manager, credit risk, treasury).

---

## Contributing

Contributions are welcome. Please open issues to request features or report bugs, and submit pull requests for improvements.

---

## License

Specify a license (e.g., MIT) in a LICENSE file. If unsure, add: "See LICENSE file."

---

If you want, I can:
- tailor the README with exact run/deploy commands for your stack (React/Flask/Django/Node) — tell me which tech you used; or
- produce example CSV fixtures or sample charts (images or Vega/Chart config) for the loan and deposit analyses.

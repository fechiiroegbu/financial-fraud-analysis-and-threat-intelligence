# Financial Fraud Analysis & Threat Intelligence

**Live Dashboard:** [Fraud Transaction Intelligence — Tableau Public](https://public.tableau.com/app/profile/fechi.iroegbu/viz/FinancialFraudAnalysisThreatIntelligenceDashboard/Dashboard1)

## Executive Summary

This project analyzes ~5 million Nigerian retail transactions (Jan 2023 – Dec 2024, worth ₦328.11B) to identify where and how financial fraud is concentrated. Of the transactions analyzed, 40,000 (0.81%) were flagged as fraudulent, totaling ₦2.66B in fraud value. Fraud is heaviest on **mobile** and **POS** channels, and is concentrated in a small number of states — **Kano, Lagos, Rivers, and Kaduna** account for the largest share of fraud value. The findings point to mobile and POS channels, along with these high-risk states, as priority areas for stronger fraud controls.

## Business Problem

Financial institutions process millions of transactions daily, and fraud — even at a small percentage — can represent significant financial loss and risk exposure. Without visibility into *where* and *how* fraud occurs (which channels, transaction types, and locations are most affected), fraud prevention teams are left reacting to individual cases rather than addressing systemic risk areas. This project simulates that analytical challenge: turning a large raw transaction dataset into clear, actionable fraud intelligence.

## Objective

- Clean and prepare a large raw transactions dataset for analysis
- Identify fraud patterns across transaction channel, type, and geography
- Quantify the scale of fraud (count, value, rate) relative to total transaction volume
- Present findings in an interactive dashboard that non-technical stakeholders (e.g., risk or compliance teams) can explore

## Dataset

- **Source:** Synthetic dataset of Nigerian retail transactions
- **Format:** Parquet file, loaded and queried directly with DuckDB (no traditional database needed)
- **Size:** ~5,000,000 rows
- **Time period:** January 2023 – December 2024
- **Key fields:** `transaction_id`, `account_id`, `customer_id`, `timestamp`, `amount_ngn`, `balance_before_ngn`, `balance_after_ngn`, `transaction_type`, `channel`, `merchant_category_code`, `merchant_name`, `location_lga`, fraud/status flags

## Tools

- **Python** — scripting and workflow orchestration
- **DuckDB** — fast, in-process SQL engine used to query the Parquet file directly and perform all cleaning/aggregation
- **SQL** — data quality checks, transformations, and exploratory aggregations
- **Tableau Public** — interactive dashboard and geographic visualization

## Data Cleaning

Before analysis, the raw dataset was checked and cleaned using DuckDB SQL:

- **Duplicate check:** Verified no duplicate `transaction_id` values existed
- **Missing value check:** Counted nulls across all key fields (IDs, timestamps, amounts, channel, merchant fields, location)
- **Type correction:** Cast `amount_ngn`, `balance_before_ngn`, and `balance_after_ngn` from decimal to `BIGINT` for consistency, since the currency has no sub-unit precision relevant to this analysis
- **Missing merchant data:** Filled blank `merchant_category_code` values with `'N/A'` and blank `merchant_name` values with `'Non_merchant'`, so downstream aggregations and filters in Tableau don't silently drop or misrepresent these rows
- **Export:** Cleaned data written out to `cleaned_retail_transactions.csv` for use in Tableau

## Methodology

1. **Load & Explore** — Read the Parquet file directly with DuckDB; inspected schema and row count
2. **Data Quality Checks** — Ran SQL checks for duplicates and missing values before trusting any aggregation
3. **Exploratory Analysis** — Broke down transaction volume by channel, transaction type, and location (LGA) to understand the shape of the data
4. **Cleaning** — Applied the corrections above and validated the cleaned output
5. **Export & Visualize** — Exported the cleaned CSV and built a Tableau dashboard covering:
   - KPI summary (total transactions, value, fraud count, fraud value, fraud rate)
   - Fraud trend over time
   - Fraud value by channel
   - Fraud by geography (choropleth map, top 10 locations)
   - Top fraudulent locations ranked by value

## Key Findings

- **Total transactions:** 5,000,000, worth ₦328.11B
- **Fraud count:** 40,000 (0.81% fraud rate)
- **Total fraud value:** ₦2.66B
- **Highest-risk channels:** Mobile (₦939M), POS (₦794M), ATM (₦550M), Web (₦245M)
- **Top fraud hotspots by value:** Kano (₦663M), Lagos (₦277M), Rivers (₦233M), Kaduna (₦169M)

## Files

- `fraud_analysis_cleaned.ipynb` — full data cleaning and exploration workflow (Python + DuckDB SQL)
- `cleaned_retail_transactions.csv` — cleaned dataset powering the Tableau dashboard *(include if you're uploading the data file)*

## Next Steps

- Investigate *why* mobile and POS channels carry disproportionately higher fraud value — e.g., transaction size, merchant category, or time-of-day patterns
- Build a time-series breakdown by state to see whether fraud hotspots are stable or shifting month to month
- Add a transaction-type view to see whether fraud clusters around specific transaction types (e.g., transfers vs. withdrawals)
- Explore a simple risk-scoring rule (e.g., flag transactions above a certain amount on high-risk channels/locations) as a possible early-warning layer

## Dashboard Preview

*<img width="893" height="369" alt="fraud screenshoot" src="https://github.com/user-attachments/assets/6b7a859c-34fe-455f-b212-15b45a2ea04c" />
*

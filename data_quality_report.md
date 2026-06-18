# Data Quality Report — D2C Churn Retention Project

This report documents the anomalies, missing fields, and inconsistencies identified in the raw datasets, along with recommended treatment plans for modeling and business analysis.

---

## 1. Summary of Identified Data Quality Issues

| File | Column(s) | Issue Type | Detail / Metric | Modeling Impact |
|---|---|---|---|---|
| `orders.csv` | `order_id` | Duplicate-like records | 12 orders have duplicate rows ending with the `_DUP` suffix. | Inflates purchase frequency and monetary values if not deduplicated. |
| `orders.csv` | `gross_amount` | Extreme Outliers | 7 orders exceed ₹5,000 (up to ₹24,789). Specifically, customer `CUST01295` has an order of ₹10,643.82 in `orders.csv`, which is cleaned to ₹967.62 in `rfm_modeling_snapshot.csv` (exactly 11x smaller). | Skews monetary spend averages and features. Must be winsorized, capped, or corrected. |
| `orders.csv` | `rating` | Missing Values | 80 orders have null satisfaction ratings. | Average ratings for these customers could skew or be NaN. |
| `customers.csv` | `loyalty_tier` | Missing Values | 1,386 customers (~57.7%) have null values. | Nulls indicate customers are not enrolled. Can be filled with a "Not Enrolled" category. |
| `customers.csv` | `skin_type` | Missing Values | 401 customers (~16.7%) have null values. | Self-reported skin type is missing. Fill with "Not Disclosed" or "Unknown". |
| `orders.csv` | `order_date` | Future Leakage | Orders extend up to `2025-11-29`, which is post-snapshot (`2025-09-30`). | **CRITICAL:** Orders placed after `2025-09-30` represent target window actions and must not be used for model features. |
| `intervention_history.csv` | `last_campaign_cost` | Logging Inconsistency | ~80% of customers with `last_campaign_received == 'none'` have non-zero campaign costs (₹12, ₹18, ₹25, ₹40). | Indicates a tracking or recording error in campaign cost logging. |

---

## 2. In-Depth Technical Details & Evidence

### A. Duplicate-Like Entries in `orders.csv`
We identified exactly **12 duplicate rows** that end with the `_DUP` suffix. A side-by-side comparison of the base record and the duplicated record (e.g. `ORD008249` and `ORD008249_DUP`) reveals that all attributes—including quantities, gross amounts, order dates, satisfaction ratings, and customer IDs—are identical.
*   *Example:*
    *   `ORD008249` for `CUST00153` on `2025-11-04`: Gross amount ₹753.30, Delivery Days: 8, Rating: 3.0.
    *   `ORD008249_DUP` for `CUST00153` on `2025-11-04`: Identical values.
*   *Action:* Deduplicate by dropping all rows containing `_DUP` in `order_id` before feature aggregation.

### B. Severe Gross Amount Outliers in `orders.csv`
There are 7 transactions with extremely high gross values that do not align with typical personal-care order sizes (which have a median of ₹597):
1.  `ORD000701` (`CUST00211`): ₹22,719.45
2.  `ORD004428` (`CUST01295`): ₹10,643.82
3.  `ORD004650` (`CUST01360`): ₹8,777.20
4.  `ORD005399` (`CUST01584`): ₹8,022.50
5.  `ORD006374` (`CUST01868`): ₹24,789.38
6.  `ORD007206` (`CUST02106`): ₹15,957.48
7.  `ORD009649` (`CUST01988`): ₹12,312.12 (Post-snapshot)

*   *Key Cleaned Case:* Customer `CUST01295` has a pre-snapshot order `ORD004428` of ₹10,643.82. However, in the pre-built `rfm_modeling_snapshot.csv`, their `monetary_180d` is ₹1,979.11. Summing their other pre-snapshot orders in the 180-day window (`ORD004426` at ₹677.05 and `ORD004427` at ₹334.44) equals ₹1,011.49. The remaining amount is ₹967.62, which is exactly `10,643.82 / 11`. This indicates that the raw gross amount of ₹10,643.82 was a logging error (decimal error or typing slip) and should have been ₹967.62.
*   *Action:* For any custom RFM computations, winsorize or scale down these outliers to avoid skewed monetary features.

### C. Campaign Cost Logging Inconsistencies
In `intervention_history.csv`, we expect `last_campaign_cost` to be 0 for customers who did not receive any campaign (`last_campaign_received == 'none'`). However:
*   Only **103** out of **507** customers with campaign type `'none'` have a cost of ₹0.
*   The remaining **404** customers with `'none'` campaign have costs distributed across ₹12, ₹18, ₹25, and ₹40.
*   Additionally, active campaigns (like `welcome_offer` or `free_shipping`) are recorded with a cost of ₹0 in ~20% of their cases.
*   *Action:* Treat `last_campaign_cost` as unreliable or handle it as a noisy signal. For segmentation and campaign design, rely on `last_campaign_received` categories instead of the reported cost.

### D. Leakage Prevention (Dates Consistency)
`orders.csv` contains orders placed up to `2025-11-29`, whereas the feature snapshot date is `2025-09-30`.
*   *Critical Rule:* All orders with `order_date > 2025-09-30` must be excluded when building features. They should only be used to compute the label `churn_next_60d` (i.e. checking if a customer ordered in the target window of 60 days following the snapshot date).

---

## 3. Data Ingestion & Join Verifications
*   **Total Customers:** 2,400 (corresponds to `customers.csv`, `churn_labels.csv`, `web_events_snapshot.csv`, and `intervention_history.csv` rows).
*   **Join Keys:** `customer_id` is the primary key across all files. Left-joining from `customers` is 100% successful.
*   **Support Tickets Integration:** `support_tickets.csv` contains 1,921 tickets covering 1,247 unique customers. This means 1,153 customers have zero support tickets. These null values are expected and represent zero support complaints. They should be filled with 0 for ticket counts.

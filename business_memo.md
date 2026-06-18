# Business Memo — D2C Customer Retention Strategy Priorities

**To:** Leadership Team, D2C Personal-Care Brand  
**From:** Data Analytics Team  
**Date:** June 16, 2026  
**Subject:** Grounded Churn Risk Insights & Retention Priorities

---

## 1. Executive Summary
An audit of our customer database (2,400 customers, 10,009 orders) reveals that our overall churn rate stands at a critical **47.0%**. Customers are defined as "churned" if they failed to make a purchase in the 60 days following the snapshot date of September 30, 2025. 

To improve retention without eroding margin through blanket discounting, we must prioritize interventions based on three core insights:
1.  **Digital Engagement Decay** is the strongest leading indicator of churn.
2.  **The "Silent Churner" Paradox:** Customers who raise complaints are active and engaged. The customers at greatest risk are those who have completely disengaged digitally and stop placing orders without ever filing a support ticket.
3.  **Customer Satisfaction & Support Bottlenecks:** When active high-value customers *do* contact support, long resolution hours and poor ratings (e.g., ratings of 1 or 2) are highly predictive of immediate churn.

---

## 2. Key Dataset-Backed Findings

### A. Digital Engagement is a Leading Indicator
Web/app activity in the 30 days prior to the snapshot date shows a strong inverse correlation with churn. 
*   **The Trend:** Customers with 0 sessions in the last 30 days experience a **48.9%** churn rate. In contrast, customers with 21 or more sessions have a churn rate of only **15.1%**.
*   **Action:** Digital touchpoints (e.g., email opens, app sessions) must be tracked in real-time. A drop in session frequency is a trigger to re-engage the customer before they officially churn.

### B. The Silent Churner Paradox
Support ticket data reveals a surprising trend: customers who raised support tickets in the last 90 days had a churn rate of **46.8%**, which is slightly *lower* than the **47.2%** churn rate of customers who raised no tickets.
*   **Interpretation:** Raising a ticket shows a customer is still invested in our brand. Customers who churn "silently"—those who experience delivery or product issues but do not reach out—are far more difficult to retain.
*   **VIP Case Study:** Customer `CUST00332` is a high-value customer (spend of ₹2,059.80) who returned 50% of their orders and gave a satisfaction rating of 1.5. Despite filing two tickets with highly negative sentiment, they had 0 web sessions in the last 30 days. This customer is at extreme risk but has not churned yet, representing a critical, manual VIP outreach opportunity.

### C. Category-Specific Risk Profiles
Churn is not uniform across product categories:
*   **Baby Care:** This category has the highest churn rate at **50.7%**. This is common in D2C as babies outgrow sizes/needs, but it suggests we are failing to transition these parents to family/wellness categories.
*   **Fragrance:** This category has our lowest churn rate at **45.3%**, showing higher product loyalty.

---

## 3. Recommended Investigations Before Launching Campaigns

Before launching any promotional retention campaigns, we recommend investigating:
1.  **Order Delivery Delays:** Multiple customers (e.g., `CUST00438`) experienced long support resolution times (36+ hours) and returned 100% of their orders. We must audit our shipping partners to resolve late deliveries.
2.  **Discount Cannibalization:** Average discount percentage is negatively correlated with churn, but new signups (e.g., `CUST00010` and `CUST00049`) buy exclusively when discounts exceed 45%. We must investigate if we are training customers to only purchase on deep discounts.
3.  **UX Checkout Funnel:** Customers like `CUST00084` have high web activity (11 sessions, 60 product views, 5 cart adds) but zero orders in the last 180 days. We must run a user experience audit to see why customers are abandoning carts.

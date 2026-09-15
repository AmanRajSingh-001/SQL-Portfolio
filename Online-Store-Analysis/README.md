

# Online Store Analysis

**Dataset:** 1200 e-commerce orders  
**Date:** September 14, 2024  
**Tool:** SQLite

---

## 📊 Dataset Overview

- **Total Orders:** 1,200
- **Unique Customers:** 1,189
- **Products:** 7 (Monitor, Phone, Tablet, Chair, Printer, Laptop, Desk)
- **Total Revenue:** ₹12,64,762
- **Average Order Value:** ₹1,054

---

## 🎯 Business Questions Answered

1. How many total orders and unique customers?
2. What's the total revenue and AOV?
3. What are the highest and lowest value orders?
4. How many orders are delivered vs cancelled?
5. Which products generate the most revenue?
6. Why does Chair outperform Phone despite lower pricing?

---

## 🚨 Critical Findings

### 1. Cancellation Crisis
```sql
SELECT OrderStatus, COUNT(*), 
       ROUND(COUNT(*) * 100.0 / 1200, 2) AS Percentage
FROM "Online-Store-Orders"
GROUP BY OrderStatus
ORDER BY COUNT(*) DESC;
Result:

Cancelled: 250 (20.83%) ❌
Delivered: 231 (19.25%) ✅
Gap: More orders cancelled than delivered!
Impact: ₹2,63,000 potential revenue loss

2. Product Performance — The Volume Story
SQL

SELECT 
    Product,
    COUNT(*) AS Orders,
    ROUND(AVG(Quantity), 2) AS Avg_Qty_Per_Order,
    SUM(Quantity) AS Total_Units,
    SUM(TotalPrice) AS Revenue
FROM "Online-Store-Orders"
GROUP BY Product
ORDER BY Revenue DESC;
Top Performers:

Chair — ₹1,95,620

Avg qty/order: 3.16 (highest)
Total units: 562 (bulk buying behavior)
Printer — ₹1,95,613

Avg qty/order: 2.99
Total units: 542
Underperformer:

Phone — ₹1,51,722
Highest unit price (₹375) but lowest volume
Avg qty/order: 2.63 (lowest)
Total units: 411 (lowest)
Key Insight:
Revenue = Orders × Unit Price × Quantity Per Order
→ Chair wins on volume despite mid-range pricing

3. Customer Retention Issue
SQL

SELECT 
    COUNT(*) AS Total_Orders,
    COUNT(DISTINCT CustomerID) AS Unique_Customers
FROM "Online-Store-Orders";
Result:

Total orders: 1,200
Unique customers: 1,189
Repeat customers: Only 11 (0.9%)
💡 Recommendations
Immediate (0-30 days):
Investigate cancellations — GROUP BY PaymentMethod, Product, ShippingAddress
Contact pending orders (237 orders) — if >7 days, follow up or auto-cancel
Strategic (30-90 days):
Replicate Chair's success — promote bundle deals on other products
Fix Phone's volume problem — family packs or B2B bulk sales
Customer retention program — loyalty rewards for 2nd purchase
Expected Impact:
Reduce cancellation 20.8% → 10% = save ₹1,32,000
Increase repeat customers 0.9% → 5% = +48 orders = ₹50,592
🛠️ SQL Concepts Used
✅ SELECT, WHERE, LIMIT
✅ Aggregations: COUNT, SUM, AVG, MIN, MAX
✅ COUNT(DISTINCT) for unique values
✅ GROUP BY (single & multi-column)
✅ ORDER BY DESC/ASC
✅ ROUND() for formatting
✅ Calculated fields (percentages, ratios)
✅ Multi-metric analysis

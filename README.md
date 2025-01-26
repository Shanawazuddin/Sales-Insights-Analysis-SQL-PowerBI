### **Sales-Insights-Analysis-SQL-PowerBI**

This project demonstrates an analysis of sales data using **SQL** for data querying and **Power BI** for creating insightful and interactive dashboards. The goal was to derive actionable insights into sales trends, revenue patterns, profit margins, and market performance to support strategic decision-making.

---

## Features of the Project  
- 📊 **Data Analysis Using SQL**: Querying customer data, transactions, and revenue metrics.  
- 💻 **Dynamic Dashboards in Power BI**: Visualizing KPIs such as total revenue, profit margins, sales targets, and YoY comparisons.  
- 💡 **Key Insights**: Identified revenue contributions, profit margin trends, and market-specific performance indicators.  
- 🔍 **Currency Normalization**: Standardized sales amounts to INR for accurate comparisons.  
- 📈 **Target and Variance Analysis**: Compared actual performance against defined targets.  

---

## Project Goals  
1. Analyze customer and transaction data to uncover meaningful insights.  
2. Create an interactive Power BI dashboard for visualizing sales trends and KPIs.  
3. Use DAX formulas for advanced calculations like profit margins, revenue contribution, and targets.  

---

## **SQL Scripts Used**  

### 1. Show all customer records  
```sql
SELECT * FROM customers;
```

### 2. Show total number of customers  
```sql
SELECT COUNT(*) FROM customers;
```

### 3. Show transactions for Chennai market (Market Code: Mark001)  
```sql
SELECT * FROM transactions WHERE market_code = 'Mark001';
```

### 4. Show distinct product codes sold in Chennai  
```sql
SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
```

### 5. Show transactions where currency is US Dollars  
```sql
SELECT * FROM transactions WHERE currency = 'USD';
```

### 6. Show transactions for the year 2020 (Join with Date Table)  
```sql
SELECT transactions.*, date.* 
FROM transactions 
INNER JOIN date ON transactions.order_date = date.date 
WHERE date.year = 2020;
```

### 7. Show total revenue in 2020  
```sql
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date = date.date 
WHERE date.year = 2020 
  AND (transactions.currency = 'INR\r' OR transactions.currency = 'USD\r');
```

### 8. Show total revenue in January 2020  
```sql
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date = date.date 
WHERE date.year = 2020 
  AND date.month_name = 'January' 
  AND (transactions.currency = 'INR\r' OR transactions.currency = 'USD\r');
```

### 9. Show total revenue in 2020 for Chennai  
```sql
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date = date.date 
WHERE date.year = 2020 
  AND transactions.market_code = 'Mark001';
```

---

## **Power BI Formulas and Measures (DAX)**  

### 1. **Normalizing Sales Amounts to INR**  
```DAX
norm_amount = Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)
```

### 2. **Total Revenue**  
```DAX
Revenue = SUM('Sales transactions'[norm_sales_amount])
```

### 3. **Profit Margin %**  
```DAX
Profit Margin % = DIVIDE([Total Profit Margin],[Revenue],0)
```

### 4. **Profit Margin Contribution %**  
```DAX
Profit Margin Contribution % = DIVIDE([Total Profit Margin],CALCULATE([Total Profit Margin],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
```

### 5. **Total Profit Margin**  
```DAX
Total Profit Margin = SUM('Sales transactions'[Profit_Margin])
```

### 6. **Revenue Contribution %**  
```DAX
Revenue Contribution % = DIVIDE([Revenue],CALCULATE([Revenue],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
```

### 7. **Revenue for Last Year**  
```DAX
Revenue LY = CALCULATE([Revenue], SAMEPERIODLASTYEAR('sales date'[date]))
```

### 8. **Sales Quantity**  
```DAX
Sales Qty = SUM('sales transactions'[sales_qty])
```

### 9. **Profit Target**  
```DAX
Profit Target = GENERATESERIES(-0.05, 0.15, 0.01)
```

### 10. **Dynamic Target Value**  
```DAX
Profit Target Value = SELECTEDVALUE('Profit Target'[Profit Target])
```

### 11. **Target Difference**  
```DAX
Target Diff = [Profit Margin %] - 'Profit Target'[Profit Target Value]
```

---

## Power BI Dashboard Features  
- **Revenue Breakdown**: Monthly and yearly revenue trends, normalized for currency differences.  
- **Profit Analysis**: Visualization of profit margins and their contributions across markets.  
- **Market Trends**: Insights into Chennai’s market performance and other regions.  
- **Target Analysis**: Comparison of actual profit margins against defined targets.  
- **Interactive Visuals**: Filters for year, market, and product categories to dynamically analyze data.  

---

## Key Insights  
1. **Revenue Trends**: Chennai market contributed significantly to overall sales in 2020.  
2. **Profit Margins**: Profit contributions varied significantly across products and regions.  
3. **Currency Normalization**: Allowed accurate revenue analysis across INR and USD transactions.  
4. **Target Tracking**: Profit margin targets helped identify underperforming markets and products.  

---

## How to Use the Repository  
1. **SQL Queries**: Use the provided scripts to query and analyze your sales data in any SQL environment.  
2. **Power BI Dashboard**: Download the Power BI file to explore interactive visuals and insights.  
3. **Customization**: Adjust the DAX formulas and visuals based on your dataset and KPIs.  

---

## Tools Used  
- **SQL**: For querying and analyzing sales data.  
- **Power BI**: For creating interactive dashboards and calculating advanced KPIs.  
- **Excel**: Initial data cleaning and preparation.  

---

## Conclusion  
This project highlights the power of combining SQL and Power BI to derive actionable insights from sales data. From querying databases to visualizing trends and KPIs, it demonstrates the value of data analysis in driving business decisions.


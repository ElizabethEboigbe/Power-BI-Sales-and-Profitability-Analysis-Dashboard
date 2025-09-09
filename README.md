# Power BI Sales and Profitability Analysis

## Table of Contents
- [Introduction](#introduction)
- [Business Objectives](#business-objectives)
- [Problem Statement](#problem-statement)
- [Data Used](#Data-Used)
- [Dataset Description](#dataset-description)
- [Data Cleaning, Preparation & Transformation](#data-cleaning-preparation--transformation)
- [Data Modeling](#data-modeling)
- [Calculations (DAX)](#calculations-dax)
- [Key Questions Answered](#key-questions-answered)
- [Analysis & Visualizations (Answers + Visuals)](#analysis--visualizations--answers--visuals)
- [Insights & Findings](#insights--findings)
- [Conclusion & Recommendations](#conclusion--recommendations)
- [Dashboard](#Dashboard)
---

## Introduction
This project uses Power BI to analyze sales, purchases, costs, and profitability across products, countries, and time. The solution converts raw transaction and product files into a clean analytical model, applies DAX calculations, and delivers an interactive dashboard that answers business questions and supports decision-making.

---

## Business Objectives
The analysis was conducted to:

- Track sales, purchases, costs and expenses across dimensions (time, country, product, payment mode).  
- Measure company profitability and identify loss-making regions.  
- Provide concise KPIs and interactive visuals for management.  
- Enable drill-down analysis by Year, Month, Week, Day, Payment Mode, and Sales Type.  

---

## Problem Statement
The company lacked consolidated visibility into its overall sales performance, profitability, and cost structure across products, countries, and time periods. Raw data files were fragmented, making it difficult for management to identify high- and low-performing markets, understand how expenses such as transportation, rent, and tax impact total costs, or detect regions and products operating at a loss.
---

**Data Used**
- <a href="https://github.com/ElizabethEboigbe/Power-BI-Sales-and-Profitability-Analysis-Dashboard/blob/main/Power%20BI%20Sales%20and%20Profitability%20Analysis.pbix">Dataset</a>
---
## Dataset Description
 - **Transactions_data** – Transaction-level records.  
  *Key fields*: Date, product_id, Quantity, Transportation, Rent, Tax, Payment Mode, Sales Type, Country (or country_id).  

- **Product_data** – Product dimension.  
  *Key fields*: id, Selling price, Purchasing price.  

- **Country_data** – Country dimension.  


---

## Data Cleaning, Preparation & Transformation
The raw datasets (Transactions, Products, and Country) were cleaned and transformed in Power Query to make them ready for analysis. 
- uploaded the files into power query editor. 
- Standardized data types for dates, numeric values and text.  
- Derived date fields from the Date column: Day, Week of Month, Month Number, Month Name and Year.
- Formated month name to 3 character (e.g., jan, feb)  
- Merged Product_data into Transactions_data using product ID.  
- Renamed the imported country table to Country_data for clarity and consistency.  
- Handled nulls and removed duplicates where appropriate.
- Uploaded to desktop  

 
---

## Data Modeling
The model follows a star-like design with Transactions_data as the fact table and Product_data, Country_data, and a Date dimension as dimensions.

**Key relationships**

- Product_data → Transactions_data — One : Many
- Transactions_data to Product_data – many : One
- Country_data → Transactions_data — One : One  
  
 

---

## Calculations (DAX)
```
Total Sales with 5% Discount =
Transactions_data[Quantity] * Transactions_data[Product_Data.Selling price] * 0.95

Total Sales without Discount =
Transactions_Data[QUANTITY]*Transactions_Data[Product_Data.SELLING PRICE]

Total Purchase =
Transactions_Data[QUANTITY]*Transactions_Data[Product_Data.PURCHASING PRICE]

Total Expense  =
sum(Transactions_Data[Transportation]) + sum(Transactions_Data[RENT]) + sum(Transactions_Data[TAX])

Total_Cost =
sum(Transactions_Data[Product_Data.PURCHASING PRICE]) + sum(Transactions_Data[Transportation]) + sum(Transactions_Data[RENT]) + sum(Transactions_Data[TAX])

Profit =
[Total_Sales] - [Total_Cost] 

Revenue =
sum(Transactions_Data[Total Sales with 5% discounts]) + sum(Transactions_Data[Total sales without discount])
```


## Key Questions Answered
Which month recorded the highest purchase?

What is the cost and the total purchase amount?

Which month recorded the lowest purchase?

What is the cost and total purchase amount for the lowest month?

What are the company’s Total Sales (with and without the discount), Revenue, and Profit?

What share of the company’s cost structure is made up of Purchasing Cost, Transportation, Rent, and Tax?

How do Revenue, Costs, and Profit trend across months and years (2021 vs 2022)?

Which countries generate the highest revenue and which countries operate at a loss?

What is the profit margin across different countries and products?

How do Sales Type and Payment Mode impact sales performance?

Are there months or regions where expenses significantly reduce profitability?

Based on the analysis, which countries or regions should management prioritize for growth, and which require corrective action?

How effective is the 5% discount strategy in driving revenue compared to sales without discount?
 
 

## Analysis & Visualizations — Answers and Visuals
**Which month recorded the highest purchase?**

Answer: January recorded the highest total purchase: 34,290

<img width="895" height="426" alt="Screenshot 2025-09-09 013116" src="https://github.com/user-attachments/assets/050fe31a-3587-49b6-8283-b3a43e1ce468" />

**What is the cost and the total purchase amount?**

Total Cost: 5,583

Total Purchase: 34,290

**Which month recorded the lowest purchase?**

Answer: April recorded the lowest total purchase: 21,282

**What is the cost and total purchase amount for the lowest month?**

Total Purchase: 21,282

Total Cost: 3,599


**What are the company’s Total Sales (with and without the 5% discount), Revenue, and Profit?**

Answer:

Total Sales (with 5% discount): 381.34k

Total Sales (without discount): 401.41k

Revenue: 782.75k

Profit: 733.98k

<img width="676" height="90" alt="Screenshot 2025-09-09 013910" src="https://github.com/user-attachments/assets/3848da44-3677-4bed-b7f0-372f616a0611" />


**What share of the company’s cost structure is made up of Purchasing Cost, Transportation, Rent, and Tax?**

Answer: The cost structure is dominated by Purchasing Cost (≈ 84.07%), followed by Total Cost (≈ 12.33%), Transportation (≈ 1.33%), Total Expense (≈ 2%), and Tax (≈ 0.27%).

<img width="807" height="460" alt="Screenshot 2025-09-09 014912" src="https://github.com/user-attachments/assets/dd08016a-9394-44e2-b55f-6e4d23ebf886" />


**How do Revenue, Costs, and Profit trend across months and years (2021 vs 2022)?**

Answer: Revenue and profit both grew year over year, increasing from 162,822 (2021) to 189,817 (2022) (~16.6% growth). Costs rose as well but were outpaced by revenue growth, resulting in improved profitability.


**Which countries generate the highest revenue and which countries operate at a loss?**

Answer: All countries except one generated revenue, Top revenue-generating countries include Saudi Arabia, Sweden, Tuvalu, Kiribati, and Sierra Leone. Niger recorded losses in the period analyzed.

<img width="894" height="483" alt="Screenshot 2025-09-08 010901" src="https://github.com/user-attachments/assets/b83857a3-76c2-4dcc-a8ab-56349b9c7bdb" />

<img width="855" height="416" alt="Screenshot 2025-09-08 011315" src="https://github.com/user-attachments/assets/47fbe1be-27b0-48fb-8499-0ef304026ef5" />



**What is the profit margin across different countries and products?**

Answer: Profit margins vary widely — higher-volume countries (e.g., Saudi Arabia, Sweden) show healthier margins; lower-volume or high-cost markets show slim or negative margins. Top performing products deliver the best margins while underperforming products reduce overall profitability.
 
**How do Sales Type and Payment Mode impact sales performance?**

Answer: Sales Type and Payment Mode influence revenue distribution. Direct sales and certain payment modes (e.g., Cash in this dataset) show stronger contribution, while wholesale is less used. Payment mode patterns suggest opportunities to promote higher-converting channels.

 <img width="873" height="447" alt="Screenshot 2025-09-08 013755" src="https://github.com/user-attachments/assets/8e00c754-355c-44f1-a4cd-f23ea989e695" />


**Are there months or regions where expenses significantly reduce profitability?**

Answer: Yes — months with lower sales (for example April) and smaller regions with relatively high expenses show reduced profitability and, in some cases, losses.


**Based on the analysis, which countries or regions should management prioritize for growth, and which require corrective action?**

Answer: Prioritize Saudi Arabia, Sweden, Tuvalu, Kiribati, and Sierra Leone for growth. Regions such as Niger, Montenegro, South Africa, United Kingdom, El Salvador, and Uganda require corrective actions to address persistent losses.
 

**How effective is the 5% discount strategy in driving revenue compared to sales without discount?**

Answer: The 5% discount increased sales volume but reduced margins; sales without discount produce stronger profitability. The discount performs as a volume lever but should be applied selectively.


**Insights & Findings**

The analysis revealed year-over-year sales growth of 16.6% (162,822 → 189,817) with improved profitability as revenue growth outpaced costs. Performance varies by market: Saudi Arabia and Sweden are strong performers while Niger and several other smaller markets operate at a loss. Purchasing costs dominate the cost structure (>80%), with transportation, rent and tax contributing modest shares. Seasonality is apparent — January and December are peak months while April shows dips that disproportionately reduce margins. The 5% discount increases volume but erodes margins, suggesting a selective discount approach. A small set of products and markets drive the majority of revenue and profit.

**Conclusion & Recommendations**
The company shows steady growth and improved profitability, but performance is uneven across regions and products. To sustain growth, focus efforts on profitable markets and top-performing products, address loss-making regions with corrective strategies, and target purchasing cost reductions. Use discounts selectively and promote payment modes that correlate with stronger sales.

**Dashboard**

<img width="940" height="480" alt="Screenshot 2025-09-08 231809" src="https://github.com/user-attachments/assets/6d051635-6f27-410d-81d5-68b2814dd004" />

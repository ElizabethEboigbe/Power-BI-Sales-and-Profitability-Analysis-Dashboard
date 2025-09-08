# Power-BI-Sales-and-Profitability-Analysis
Power BI project analyzing sales, purchases, costs, and profitability across products and countries. Features data preparation in Power Query, data modeling, DAX measures, DAX calculated columns, KPIs, and an interactive dashboard with business insights.
Power BI Sales and Profitability Analysis
Table of contents

Introduction

Business objectives

Problem statement

Dataset description

Data cleaning, preparation & transformation

Data modeling

Calculations (DAX)

Calculated columns

Measures

Key questions answered

Analysis & visualizations (answers + visuals)

Insights & findings

Conclusion & recommendations

Repository structure & deliverables

Assumptions & notes / questions for you

Introduction

This project uses Power BI to analyze sales, purchases, costs, and profitability across products, countries, and time. The solution converts raw transaction and product files into a clean analytical model, applies DAX calculations, and delivers an interactive dashboard that answers business questions and supports decision-making.

Business objectives

The analysis was conducted to:

Track sales, purchases, costs and expenses across dimensions (time, country, product, payment mode).

Measure company profitability and identify loss-making regions.

Provide concise KPIs and interactive visuals for management.

Enable drill-down analysis by Year, Month, Week, Day, Payment Mode, and Sales Type.

Problem statement

The company lacked consolidated visibility into sales performance, profitability and cost drivers across products, countries and periods. Raw files were fragmented, making it difficult to answer questions such as which months and countries generate the highest or lowest purchases and profit, how transportation/rent/tax affect total cost, and which regions or products run at a loss.

Dataset description

Tables used

Transactions_data – transaction-level records. Key fields: Date, product_id, Quantity, Transportation, Rent, Tax, Payment Mode, Sales Type, Country (or country_id).

Product_data – product dimension. Key fields: id, Selling price, Purchasing price.

Country_data – country dimension (renamed from Table1).

Data covers multiple months across 2021 and 2022 enabling YoY comparisons.

Data cleaning, preparation & transformation

The raw datasets (Transactions, Products, and Country) were cleaned and transformed in Power Query to make them analysis-ready. Key transformation steps included:

Standardized data types for dates, numeric values and text.

Derived date fields from the Date column: Day, Week of Month, Month Number, Month Name (3-character format, e.g., Jan), and Year.

Merged Product_data into Transactions_data using product ID (bringing in Selling and Purchasing prices).

Renamed the imported country table to Country_data for clarity and consistency.

Handled nulls and removed duplicates where appropriate.

This preparation produced a clean fact table (Transactions_data) linked to product and country dimensions to support accurate DAX calculations and performant visuals.

Data modeling

The model follows a star-like design with Transactions_data as the fact table and Product_data, Country_data, and a Date dimension as dimensions.

Key relationships

Product_data[id] → Transactions_data[product_id] — One : Many

Country_data[country_id] → Transactions_data[country_id] — One : Many (or One:One if country keys are unique on both sides)

Date[Date] → Transactions_data[Date] — One : Many (use a Date table for time intelligence)

Set cross-filter direction to single by default unless bidirectional filtering is specifically required.

Calculations (DAX)
Calculated columns (row-level)

Use calculated columns when you need a row-level value visible in tables or to validate numbers. Two implementation options are shown: (A) if prices were merged into Transactions_data, or (B) if prices remain in Product_data and you use RELATED().

If prices are merged into Transactions_data:

Total Sales (5% Discount) =
Transactions_data[Quantity] * Transactions_data[Selling price] * 0.95

Total Sales (No Discount) =
Transactions_data[Quantity] * Transactions_data[Selling price]

Total Purchase =
Transactions_data[Quantity] * Transactions_data[Purchasing price]

Total Expense (Row) =
COALESCE(Transactions_data[Transportation], 0)
+ COALESCE(Transactions_data[Rent], 0)
+ COALESCE(Transactions_data[Tax], 0)


If prices are kept in Product_data:

Total Sales (5% Discount) =
Transactions_data[Quantity] * RELATED(Product_data[Selling price]) * 0.95


(Use RELATED(Product_data[Purchasing price]) for Total Purchase likewise.)

Measures (aggregations for cards and visuals)

Create measures for KPI aggregation and analysis:

Revenue =
SUM(Transactions_data[Total Sales (5% Discount)]) + SUM(Transactions_data[Total Sales (No Discount)])

Total Purchase (Measure) =
SUM(Transactions_data[Total Purchase])

Total Cost =
SUM(Transactions_data[Total Purchase])
+ SUM(Transactions_data[Transportation])
+ SUM(Transactions_data[Rent])
+ SUM(Transactions_data[Tax])

Total Expense =
SUM(Transactions_data[Transportation])
+ SUM(Transactions_data[Rent])
+ SUM(Transactions_data[Tax])

Profit =
[Revenue] - [Total Cost]

Profit Margin % =
DIVIDE([Profit], [Revenue], 0)


(If you did not materialize row-level columns in Power Query, use SUMX variants instead.)

Key questions answered

This project addresses the following business questions:

Which month recorded the highest purchase?

What is the cost and the total purchase amount?

Which month recorded the lowest purchase?

What is the cost and total purchase amount for the lowest month?

What are the company’s Total Sales (with and without the 5% discount), Revenue, and Profit?

What share of the company’s cost structure is made up of Purchasing Cost, Transportation, Rent, and Tax?

How do Revenue, Costs, and Profit trend across months and years (2021 vs 2022)?

Which countries generate the highest revenue and which countries operate at a loss?

What is the profit margin across different countries and products?

How do Sales Type and Payment Mode impact sales performance?

Are there months or regions where expenses significantly reduce profitability?

Based on the analysis, which countries or regions should management prioritize for growth, and which require corrective action?

How effective is the 5% discount strategy in driving revenue compared to sales without discount?

Analysis & visualizations — answers and visuals

For each question below: Answer (from the analysis) and recommended Visual. Include screenshots in the repo where indicated.

Which month recorded the highest purchase?

Answer: January recorded the highest total purchase: 34,290.
Visual: Stacked Column Chart — Total Purchase by Month.
Screenshot: screenshots/highest_purchase_jan.png

What is the cost and the total purchase amount?

Answer: For the highest month (January):

Total Purchase: 34,290

Total Cost: 5,583
Visual: Stacked Column Chart + Card visuals.
Screenshot: screenshots/january_purchase_costs.png

Which month recorded the lowest purchase?

Answer: April recorded the lowest total purchase: 21,282.
Visual: Stacked Column Chart — Total Purchase by Month.
Screenshot: screenshots/lowest_purchase_apr.png

What is the cost and total purchase amount for the lowest month?

Answer: For April:

Total Purchase: 21,282

Total Cost: 3,599
Visual: Card visuals + Stacked Column Chart.
Screenshot: screenshots/april_purchase_costs.png

What are the company’s Total Sales (with and without the 5% discount), Revenue, and Profit?

Answer:

Total Sales (with 5% discount): 381.34k

Total Sales (without discount): 401.41k

Revenue: 782.75k

Profit: 733.98k
Visual: KPI cards (top of dashboard).
Screenshot: screenshots/kpis_sales_revenue_profit.png

What share of the company’s cost structure is made up of Purchasing Cost, Transportation, Rent, and Tax?

Answer: The cost structure is dominated by Purchasing Cost (≈ 84.07%), followed by Total Cost (≈ 12.33%), Transportation (≈ 1.33%), Total Expense (≈ 2%) and Tax (≈ 0.27%).
Visual: Donut/Pie Chart — Cost structure.
Screenshot: screenshots/cost_structure_donut.png

How do Revenue, Costs, and Profit trend across months and years (2021 vs 2022)?

Answer: Revenue and profit both grew YoY, increasing from 162,822 (2021) to 189,817 (2022) (~16.6% growth). Costs rose as well but were outpaced by revenue growth, resulting in improved profitability.
Visual: Line chart or combo chart — Revenue, Cost, Profit by Month with Year slicer.
Screenshot: screenshots/revenue_cost_profit_trend.png

Which countries generate the highest revenue and which countries operate at a loss?

Answer: Top revenue-generating countries include Saudi Arabia, Sweden, Tuvalu, Kiribati, Sierra Leone. Niger recorded losses in the period analyzed.
Visual: Bar chart (Revenue by Country) and Table (Revenue, Profit by Country).
Screenshot: screenshots/revenue_by_country.png

What is the profit margin across different countries and products?

Answer: Profit margins vary widely — higher-volume countries (e.g., Saudi Arabia, Sweden) show healthier margins; lower-volume or high-cost markets show slim or negative margins. Top-performing products deliver the best margins while underperforming products reduce overall profitability.
Visual: Bar chart — Profit margin % by Country; Bar chart or Pareto chart — Profit margin by Product.
Screenshot: screenshots/profit_margin_by_country_product.png

How do Sales Type and Payment Mode impact sales performance?

Answer: Sales Type and Payment Mode influence revenue distribution. Direct sales and certain payment modes (e.g., Cash in this dataset) show stronger contribution, while wholesale is less used. Payment mode patterns suggest opportunities to promote higher-converting channels.
Visual: Stacked column (Sales by Sales Type and Payment Mode) and Matrix.
Screenshot: screenshots/sales_type_payment_mode.png

Are there months or regions where expenses significantly reduce profitability?

Answer: Yes — months with lower sales (for example April) and smaller regions with relatively high expenses show reduced profitability and, in some cases, losses.
Visual: Line & Column combo (Revenue vs Expense by Month); Map visual for regional profitability.
Screenshot: screenshots/expenses_impact.png

Based on the analysis, which countries or regions should management prioritize for growth, and which require corrective action?

Answer: Prioritize Saudi Arabia, Sweden, Tuvalu, Kiribati, Sierra Leone for growth. Regions such as Niger, Montenegro, South Africa, United Kingdom, El Salvador, Uganda require corrective actions to address persistent losses.
Visual: Map and ranked bar chart (Profit/Revenue by Country).
Screenshot: screenshots/prioritize_vs_corrective.png

How effective is the 5% discount strategy in driving revenue compared to sales without discount?

Answer: The 5% discount increased sales volume but reduced margins; sales without discount produce stronger profitability. The discount performs as a volume lever but should be applied selectively.
Visual: Clustered column (Sales with Discount vs Sales without Discount); KPI comparison cards.
Screenshot: screenshots/discount_effectiveness.png

Insights & findings

The analysis revealed year-over-year sales growth of 16.6% (162,822 → 189,817) with improved profitability as revenue growth outpaced costs. Performance varies by market: Saudi Arabia and Sweden are strong performers while Niger and several other smaller markets operate at a loss. Purchasing costs dominate the cost structure (>80%), with transportation, rent and tax contributing modest shares. Seasonality is apparent — January and December are peak months while April shows dips that disproportionately reduce margins. The 5% discount increases volume but erodes margins, suggesting a selective discount approach. A small set of products and markets drive the majority of revenue and profit.

Conclusion & recommendations

The company shows steady growth and improved profitability, but performance is uneven across regions and products. To sustain growth, focus efforts on profitable markets and top-performing products, address loss-making regions with corrective strategies, and target purchasing cost reductions. Use discounts selectively and promote payment modes that correlate with stronger sales.

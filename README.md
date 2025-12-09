## Summary & Objective

This project delivers a comprehensive Power BI analysis of Global Superstore's multi-year performance, focusing on two critical areas: **Customer Lifetime Value (CLV)** and **Product Portfolio Profitability**. The primary objective was to move beyond descriptive reporting to provide prescriptive, actionable insights that **reduce profit leakage** (through high churn and destructive discounting) and **optimize high-value customer retention**.
## Dataset Used 
- <a href="https://github.com/Azab-Basha/Global-Superstore-BI-Analysis/blob/main/Globa_Superstore.xlsx">Global Superstore Dataset</a>
## Key Business Questions Answered

The analysis was designed to address core business challenges across the customer journey and product lifecycle:

1.  What is the final **Customer Lifetime Value (CLV)**, and what percentage of our customer base constitutes **lifetime churn**?
2.  How does retention evolve for established **customer cohorts** over multiple years, and where is the steepest drop-off?
3.  Which products are the absolute **Best and Poorest Performers** when simultaneously weighted across **Sales, Profit**, and **Returns**?
4.  Which **Discount Bands (e.g., 40%+)** lead to the most significant **profit erosion** versus sales gain across different regions?
5.  What is the financial breakdown of sales and profit across our tiered **Customer Classes (Titanium, Platinum, Gold, and Silver)**?
6.  How do the return rates of our most profitable products compare to the average, indicating potential quality or logistics risks?
7.  Which major product categories **(Technology, Furniture, and office supplies)** offer the highest and lowest **profit contribution**, informing inventory strategy?
8.  How does the 3-Month Rolling Average of Total Profit compare to the Total Sales trend, indicating potential long-term pricing and discounting issues?
9. ​ Which customer segments **(Consumer, Corporate, Home Office)** are responsible for the highest Return Rates, informing targeted post-sale support strategies?
10. Which **geographic markets (countries)** are the highest and lowest performers in terms of absolute sales and profit margin percentage?
11. What is the relationship between **Average Shipping Days** and **Average Profit** per Order, helping to determine the optimal balance between cost and delivery speed?

## Key Findings and Insights

1. **Geographic Performance Disparity**  
   Asia Pacific recorded the highest Total Sales at **4,042,658** significantly outperforming Africa, which had the lowest at **783,773**.

2. **Recent Growth Trigger**  
   The **Rolling Avg 3M** showed a strong upward trend starting **August 19, 2015**, increasing by **38.98%** over **4.40 months**, indicating a strong but unidentified growth catalyst.

3. **Sales Volume Driver**  
   The **0–10% discount band** contributed the majority of Total Sales **7,375,423** or **(58.34%)**, far surpassing the **30–40% band**, which generated only **330,749**.

4. **Regional Profit Erosion**  
   Higher discount ranges (**30–40%** and **40%**) significantly eroded profit and sales, especially in **Australia, Turkey, and France**.

5. **Segmented Return Risk**  
   The *Consumer* segment had the highest Return Rate **(6.51%)**, followed by **Corporate (5.75%)** and *Home Office **(5.49%)**.

6. **Customer Class Value**  
   The **Titanium** class generated the highest Total Sales and Total Profit.  
   The **Silver** class generated the lowest.  
   Top individual performers:  
   - **Tom Ashbrook** – Total Sales: **30,968.13**  
   - **Raymond Buch** – Total Profit: **8,226.72**

7. **Critical Profit Leak**  
   The **(74.63%)** **Lifetime Customer Churn Rate** poses a major threat to profitability, offsetting the strong potential of the **$972.65** **Customer Lifetime Value (CLV)**.

8. **Category Sales Dominance**  
   **Technology** led with **4,744,557** in Total Sales, outperforming **Office Supplies** by **(25.27%)** (Office Supplies: **3,787,492**.

9. **Category Profit Efficiency** 
   **Technology** generated **45%** of all profit, followed by *Office Supplies **(35%)**, while **Furniture** contributed the least at **19%**.


 ## Final Recommendations

1.  *Strategic Resource Shift:** Shift marketing and sales resources away from low-performing regions like **Africa** and redirect them toward **Asia Pacific** to capitalize on proven market demand.
2.  **Growth Catalyst Replication:** Immediately investigate sales and marketing activities that occurred just prior to **August 19, 2015**, to codify and replicate this successful growth catalyst.
3.  **Discount Band Optimization:** Maintain the high-volume **0-10% discount band**. Re-evaluate the effectiveness of the low-volume **30-40% band**; consider eliminating it or shifting sales to the more profitable **10-20% range**.
4.  **Regional Discount Policy:** Establish and enforce a regional policy that **strictly curtails deep discounts (above 30%)** in markets like **Australia, Turkey, and France**, where they are shown to be unprofitable.
5.  **Segment-Specific Support:** Implement targeted post-sale support and product education specifically for the **Consumer segment** to reduce their high return rate of **(6.51%)**.
6.  **Customer Class Investment:** Prioritize marketing spend and VIP service on the top-tier **Titanium** class. Create a remediation or sunset plan for the low-performing **Silver** class to stop resource drain.
7.  **Retention Program Launch:** Launch a focused campaign within **90 days** of the first purchase to drive a second sale and actively reduce the high **74.63% lifetime churn rate**.
8.  **Technology Inventory Scale:** Allocate greater capital and warehousing space to **Technology** inventory to meet its high sales demand, ensuring supply chains can sustain its dominance.
9.  **Furniture Profit Deep-Dive:** Conduct a deep-dive analysis into the **Furniture** category to identify the source of its low **(19% )** profit contribution and develop an efficiency plan.

## Technical Toolkit & DAX Highlights

This project utilized best practices in data modeling and advanced calculations to move beyond simple descriptive reporting.

| Tool / Skill | Application |
| :--- | :--- |
| **BI Platform** | **Power BI Desktop** **(PL-300 Certified)** |
| **Data Preparation** | *Power Query (M)* for complex data cleansing and transformation. |
| **Database Querying** | **Foundational SQL** understanding for efficient data extraction and connection when connecting to live relational databases. |
| **Data Modeling** | Implemented a **Star Schema** architecture utilizing Fact and Dimension tables for optimized performance. |

---
## Advanced DAX Documentation: 1. Dynamic Pareto Analysis

This section details the custom measures and supporting tables implemented to create a fully dynamic Pareto Analysis for both Products and Customers, leveraging advanced DAX functions like `WINDOW` and disconnected tables.

### 1. X\_axis (Calculated Table)
(Description: A disconnected table used as a continuous numerical axis for plotting the Pareto curve line chart.)

```dax
X_axis = 
SELECTCOLUMNS(
    GENERATESERIES(1, COUNTROWS(Dim_Customer)),
    "X", [Value]
)
```
### 2. Metric (Calculated Table)
​(Description: A disconnected table listing the metrics the user can select for the Pareto Analysis.)
```dax
Metric = 
SELECTCOLUMNS(
    {
        "Total Sales",
        "Total Profit",
        "Total Cost",
        "Total Orders"
    },
    "Meausre", [Value]
)
```
### 3. Metric Value (Measure)
​(Description: Returns the value of the measure selected by the user from the Metric table.)
```dax
Metric Value = 
SWITCH(
    SELECTEDVALUE(Metric[Meausre]),
    "Total Sales", [Total Sales],
    "Total Profit", [Total Profit],
    "Total Cost", [Total Cost],
    "Total Orders", [Total Orders],
    ERROR("Meaure Not Selected In Metric[Measure]")
)
```
### 4. Pareto Cumulative (Measure)
​(Description: Calculates the running cumulative total of the selected metric across customers, ordered from highest value to lowest rank (using WINDOW).)
```dax
Pareto Cumulative = 
VAR X_Pos = SELECTEDVALUE(X_axis[X]) 
VAR Points =
    ADDCOLUMNS(
        VALUES(Dim_Customer[Customer ID]),
        "@Value", [Metric Value]
    )
VAR Cumulated_Points =
    WINDOW(
        1, ABS,
        X_Pos,ABS,
        Points,
        ORDERBY([@Value], DESC, Dim_Customer[Customer ID],ASC)
    )
VAR Result = 
    SUMX(
        Cumulated_Points,
        [@Value]
    )
RETURN
    Result
```
### 5. Pareto Amount (Measure)
​(Description: An alternative calculation similar to Pareto Cumulative, focusing on the specific amount at the current X_Pos.)
```dax 
Pareto Amount = 
VAR X_Pos = SELECTEDVALUE(X_axis[X])
VAR Points =
    ADDCOLUMNS(
        VALUES(Dim_Customer[Customer ID]),
        "@Value", [Metric Value]
    )
VAR Cumulated_Points =
    WINDOW(
        X_Pos, ABS,
        X_Pos,ABS,
        Points,
        ORDERBY([@Value], DESC, Dim_Customer[Customer ID],ASC)
    )
VAR Result = 
    SUMX(
        Cumulated_Points,
        [@Value]
    )
RETURN
    Result
```
### 6. Customer Name (Measure)
​(Description: Returns the name of the single customer at the specified rank (X_Pos) on the Pareto line, primarily used for tooltips or detail cards.)
```dax
Customer Name = 
VAR X_Pos = SELECTEDVALUE(X_axis[X]) 
VAR Points =
    ADDCOLUMNS(
        ALLSELECTED(Dim_Customer[Customer ID], Dim_Customer[Customer Name]),
        "@Value", [Metric Value]
    )
VAR Cumulated_Points =
    WINDOW(
        X_Pos, ABS,
        X_Pos,ABS,
        Points,
        ORDERBY([@Value], DESC, Dim_Customer[Customer ID],ASC)
    )
VAR Result = 
    SELECTCOLUMNS(
        Cumulated_Points,
        Dim_Customer[Customer Name]
    )
RETURN
    Result
```
### 7. Pareto % (Measure)
​(Description: Calculates the percentage contribution of the cumulative metric value relative to the total selected metric value for the entire filtered customer base (ALLSELECTED).)
```dax
Pareto % = 
DIVIDE(
    [Pareto Cumulative],
    CALCULATE(
        [Metric Value],
        ALLSELECTED(
            Dim_Customer)
))
```
### 8. What If Pareto % (Measure)
​(Description: Used for conditional formatting; returns the Pareto percentage only when it is exactly at the user-defined What-If percentage threshold to mark a single point on the Pareto chart.)
```dax
What If Pareto % = 
VAR Threshold = SELECTEDVALUE('What If Pareto'[What If Pareto])
VAR ParetoPercent = 
    DIVIDE(
        [Pareto Cumulative],
        CALCULATE(
            [Metric Value],
            ALL(Dim_Customer)
        )
    )
RETURN
    IF(
        ABS(ParetoPercent - Threshold) <0.01, ParetoPercent, BLANK()
    )
```

## Advanced DAX Documentation: 2.  Advanced Time Intelligence Calculations
These calculations leverage sophisticated DAX patterns to ensure accurate period-over-period comparisons and reliable rolling averages, handling partial years and incomplete sales history.

### 1. Sales\_YTD\_oPY % (Year-to-Date Change Over Previous Year)
(Description: Calculates the percentage change between Total Sales Year-to-Date (YTD) for the current year and the Total Sales YTD for the previous year. Includes a check (ShowValueForDates) to prevent displaying results for future dates.)

```dax
Sales_YTDoPY % = 
VAR Sales_PYC = 
    IF(
        [ShowValueForDates], 
        CALCULATE(
            [Total Sales],
            PARALLELPERIOD(
                Dim_Date[Date],
                -1,
                YEAR
            )
        )
    )
VAR ValueCurrentPeriod = [Sales_YTD]
VAR ValuePreviousPeriod = Sales_PYC
VAR Sales_YTDoPY =
    IF(
        NOT ISBLANK(ValueCurrentPeriod) && NOT ISBLANK(ValuePreviousPeriod),
        ValueCurrentPeriod - ValuePreviousPeriod
    )
RETURN
    DIVIDE(
        Sales_YTDoPY,
        Sales_PYC
    )
```
### 2. Rolling Avg 3M (3-Month Rolling Average)
​(Description: Calculates the 3-month rolling average of the selected metric ([Metric Value]). Includes a crucial check (FirstDayWithData <= FirstDayInPeriod) to avoid distorting the average when the 3-month period precedes the first date of transactions in the dataset.)
```dax
Rolling Avg 3M = 
VAR Period3M = 
    CALCULATETABLE(
        DATESINPERIOD(
            Dim_Date[Date],
            MAX(Dim_Date[Date]),
            -3,
            MONTH
        ),
        Dim_Date[DatesWithSales] = TRUE() 
    )

VAR FirstDayWithData = 
    CALCULATE(
        MIN(Fact_Sales[Order Date]),
        REMOVEFILTERS()
    )

VAR FirstDayInPeriod =
    MINX(
        Period3M,
        Dim_Date[Date]
    )

VAR Result =
    IF(
        FirstDayWithData <= FirstDayInPeriod, 
        AVERAGEX(
            Period3M,
            [Metric Value]
        )
    )

RETURN
    Result
```
## Advanced DAX Documentation: 3. Advanced Customer Analytics (Cohort & CLV)
These calculations form the foundation of the Customer Analysis page, allowing for dynamic cohort tracking and the calculation of potential customer value using best practices.

### 1. FirstOrderYear (Calculated Column in Fact_Sales or Dim_Customer)
(Description: Identifies the initial purchase year for every unique customer, necessary for cohort grouping.)
```dax 
FirstOrderYear = 
CALCULATE(
    MIN(Fact_Sales[Year]),
    ALLEXCEPT(Fact_Sales, Fact_Sales[Customer ID])
)
```
### 2. Retention Rate (Measure)
(Description: Calculates the percentage of customers from a specific cohort (FirstOrderYear) who made a purchase in a subsequent year, filtered to avoid showing 100% in the initial year.)
```dax
Retention Rate = 
VAR RetentionRate = 
    DIVIDE(
        [ActiveCustomers],
        CALCULATE(
            [ActiveCustomers],
            ALL(Fact_Sales),
            Fact_Sales[FirstOrderYear] = MIN(Fact_Sales[FirstOrderYear])
        )
    )
RETURN
    IF(
        RetentionRate = 1, BLANK(), RetentionRate
    )
```
### 3. Exited (Calculated Column in Dim_Customer)
(Description: A classification column used to tag customers as "Retained" (purchased in multiple years) or "Churned" (purchased only in their first year).)
```dax
Exited = 
VAR FirstPurchasedYear = 
    CALCULATE(
        MIN(Dim_Date[Year]),
        RELATEDTABLE(Fact_Sales)
    )

VAR LastPurchasedYear = 
    CALCULATE(
        MAX(Dim_Date[Year]),
        RELATEDTABLE(Fact_Sales)
    )

RETURN
    IF(
        LastPurchasedYear > FirstPurchasedYear, 
        "Retained", 
        "Churned"
    )
```
### 4. Lifetime Churn Rate (Measure)
(Description: The percentage of the total customer base that has been classified as churned, representing the overall retention problem.)
```dax
Lifetime Churn Rate = 
DIVIDE(
    [Churned Customers],
    [#Customers]
)
```
### 5. Customer Lifetime Value (CLV) (Measure)
(Description: Calculates the estimated total revenue a customer will contribute over their entire relationship using the formula: Average Customer Value (ACV) * Average Customer Lifespan (ACL).)
```dax
Customer Lifetime Value (CLV) = 
VAR APV = 
    DIVIDE(
        [Total Sales], [Total Orders]
    )

VAR PFR = 
    DIVIDE(
        [Total Orders], [#Customers]
    )

VAR ACV = APV * PFR

VAR ACL = 
    DIVIDE(1, [Lifetime Churn Rate])

VAR CLV = ACV * ACL
RETURN
    CLV
```
### 6. Avg. Profit Per Customer (Measure)
(Description: A fundamental KPI showing the total profit generated distributed across the number of unique customers.)
```dax
Avg. Profit Per Customer = 
DIVIDE(
    [Total Profit],
    [#Customers]
)
```

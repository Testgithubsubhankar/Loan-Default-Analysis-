# Loan-Default-Analysis
Power BI +SQL Server + Data Flow + Incremental Refresh


📌 **🔗 Data Integration with Dataflow (Power BI Service)**
One of the key highlights of this project was using **Power BI Dataflows** to manage and automate the data extraction and transformation layer:
✅ Connected SQL Server to Power BI Service
✅ Built **Dataflows** for reusability, schema consistency, and central data refresh
✅ Performed **cleaning and transformation** via **Power Query Online** (merge, filters, type conversion)
✅ Reduced workload on Power BI Desktop by shifting ETL to cloud-based Dataflow

---

📊 **Data Modeling in Power BI Desktop**
Once Dataflows were connected to Power BI Desktop, I focused on building a structured **star schema** using Fact and Dimension tables.

🧮 **Calculated Columns** (Created in Power BI Desktop):

```DAX
1. Age Group =
IF([Age] <= 19, "Teen",
   IF([Age] <= 39, "Adults",
      IF([Age] <= 59, "Middle Age Adults", "Senior Citizens")))

2. Credit Score Bins =
IF([CreditScore] <= 400, "Very Low",
   IF([CreditScore] <= 450, "Low",
      IF([CreditScore] <= 650, "Medium", "High")))

3. Income Bracket =
SWITCH(TRUE(),
   [Income] < 30000, "Low Income",
   [Income] >= 30000 && [Income] < 60000, "Medium Income",
   [Income] >= 60000, "High Income")
```

---

📐 **DAX Measures for Insightful KPIs**
To drive business understanding, I created a **dedicated Measures table** for reusable and organized calculations.

Here are a few examples:

```DAX
1. Average Income by Employment Type =
CALCULATE(AVERAGE([Income]), ALLEXCEPT('Loan_default', [EmploymentType]))

2. Average Loan (High Credit) =
AVERAGEX(FILTER('Loan_default', [Credit Score Bins] = "High"), [LoanAmount])

3. Default Rate by Year =
VAR Total = CALCULATE(COUNTROWS('Loan_default'), ALLEXCEPT('Loan_default', [Year]))
VAR Defaulters = CALCULATE(COUNTROWS(FILTER('Loan_default', [Default] = TRUE())), ALLEXCEPT('Loan_default', [Year]))
RETURN DIVIDE(Defaulters, Total) * 100

4. YOY Default Change % =
DIVIDE(
  CALCULATE(COUNTROWS(FILTER('Loan_default', [Default] = TRUE())), [Year] = YEAR(MAX([Loan_Date_DD_MM_YYYY])))
  - CALCULATE(COUNTROWS(FILTER('Loan_default', [Default] = TRUE())), [Year] = YEAR(MAX([Loan_Date_DD_MM_YYYY])) - 1),
  CALCULATE(COUNTROWS(FILTER('Loan_default', [Default] = TRUE())), [Year] = YEAR(MAX([Loan_Date_DD_MM_YYYY])) - 1),
  0
) * 100
```

I also used:

* `MEDIANX()` for central tendency
* `SUMX(FILTER(...))` to calculate by category
* `DATESYTD()` for YTD measures
* `ALLEXCEPT()` and `VALUES()` to control filter context precisely

---

📤 **Validation & Refresh Strategy**
✅ Cross-verified all DAX metrics against Excel calculations for validation
✅ Implemented **Incremental Refresh** in Power BI Service to optimize refresh time for large datasets

---

🔧 **Tech Stack Used**

* SQL Server (Data Lake Layer)
* Power BI Service (Dataflow, Refresh, Gateway)
* Power BI Desktop (Modeling & Visualization)
* Power Query
* DAX

---

Dashboard:

1. Loan Default Overview

![image](https://github.com/user-attachments/assets/2d7dd582-208a-4c06-9f48-5d5cc83b4335)


2. Applicant Demographics and financial profile

![image](https://github.com/user-attachments/assets/1a0a3e9a-8e90-4c30-86ef-018ad2f05f78)

3. Financial Risk Matrix

![image](https://github.com/user-attachments/assets/52147fe1-d8e1-4be1-94f4-125f7e63b9e8)



# 📱 Mobile Sales Dashboard | Power BI

An interactive **Power BI dashboard** that analyses **3,835 mobile phone sales transactions** across **5 brands, 15 models and 19 Indian cities** from **October 2021 to October 2024**. It turns raw Excel sales records into a single-page report that answers *what sells, where, when, and how customers pay*, with every visual filterable in one click.


## 📌 Table of Contents
- [Business Problem](#-business-problem)
- [Dataset](#-dataset)
- [Tools & Skills](#-tools--skills)
- [Data Preparation (Power Query)](#-data-preparation-power-query)
- [DAX Measures](#-dax-measures)
- [Dashboard Features](#-dashboard-features)
- [Key Insights](#-key-insights)
- [How to Use](#-how-to-use)
- [Repository Structure](#-repository-structure)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🎯 Business Problem


1. What are the **total sales, units sold and number of transactions**?
2. Which **brands and models** bring in the most revenue?
3. Which **cities** generate the most sales?
4. How do sales and quantity **change over months and days of the week**?
5. Which **payment methods** do customers prefer?
6. How **satisfied** are customers (ratings 1–5)?

The dashboard answers all six on one page, with slicers to drill into any brand, model, payment method, day or month.

---

## 📂 Dataset

| Property | Value |
|---|---|
| Source | Excel workbook (`Mobile Sales Data.xlsx`, sheet `Sheet1`) |
| Rows | **3,835** transactions |
| Period | **09 Oct 2021 – 08 Oct 2024** |
| Brands | Apple, Samsung, OnePlus, Vivo, Xiaomi |
| Models | 15 (iPhone SE/11/12, Galaxy S21/Note 20/A51, OnePlus 9/8T/Nord, Vivo Y51/V20/S1, Redmi 9, Redmi Note 10, Mi 11) |
| Cities | 19 (Delhi, Mumbai, Chennai, Bangalore, Hyderabad, Kolkata, …) |

**Columns**

| Column | Type | Description |
|---|---|---|
| Transaction ID | Whole number | Unique ID of the sale |
| Date | Date | Sale date (built from Day, Month and Year columns) |
| Day Name | Text | Monday … Sunday |
| Brand | Text | Phone brand |
| Mobile Model | Text | Phone model |
| Units Sold | Whole number | Quantity sold in the transaction |
| Price Per Unit | Decimal | Selling price of one unit (₹10,011 – ₹69,984) |
| Customer Name | Text | Buyer's name |
| Customer Age | Whole number | 18 – 59 years |
| City | Text | City of sale |
| Payment Method | Text | UPI, Debit Card, Credit Card, Cash |
| Customer Ratings | Whole number | Satisfaction rating 1 – 5 |

---

## 🛠 Tools & Skills

| Tool | Used for |
|---|---|
| **Microsoft Excel** | Raw data source |
| **Power Query** | Data cleaning and transformation (ETL) |
| **DAX** | KPI measures (`SUMX`, `SUM`, `COUNTROWS`, `AVERAGE`) |
| **Power BI Desktop** | Data modelling, visuals, slicers, interactivity, report design |

**Skills demonstrated:** data cleaning · date engineering · calculated measures · KPI design · map visualisation · drill-down hierarchies · cross-filtering with slicers · dashboard layout and storytelling.

---

## 🧹 Data Preparation (Power Query)

The raw sheet stored the date as three separate text columns, so I reshaped it before analysis:

1. **Imported** the Excel workbook and **promoted the first row** to headers.
2. **Set data types** for every column (whole numbers, decimals, text).
3. **Created a proper `Date` column** by combining `Day`, `Month` and `Year` (`[Day] & "-" & [Month] & "-" & [Year]`) and converting it to the *Date* type.
4. **Removed** the original `Day`, `Month`, `Year` and `Day Name` text columns.
5. **Re-created `Day Name`** from the new date with `Date.DayOfWeekName([Date])`, so it is always consistent with the date.
6. **Reordered columns** into a clean, logical layout.

Result: one clean fact table, **`Sales_Data`**, with a real date column that enables time-based analysis (month and day drill-down).

---

## 📐 DAX Measures

```DAX
Total Sales    = SUMX ( Sales_Data, Sales_Data[Units Sold] * Sales_Data[Price Per Unit] )
Total Quantity = SUM ( Sales_Data[Units Sold] )
Transactions   = COUNTROWS ( Sales_Data )
Average        = AVERAGE ( Sales_Data[Price Per Unit] )
```

| Measure | Value (all data) | Why it matters |
|---|---|---|
| **Total Sales** | **₹76.92 Cr** (₹769.2 M) | Revenue: row-by-row `Units × Price` with `SUMX` |
| **Total Quantity** | **19,150 units** | Sales volume |
| **Transactions** | **3,835** | Number of purchases |
| **Average** | **₹40,114** | Average selling price per unit |

---

## 📊 Dashboard Features

**KPI cards (top):** Total Sales · Total Quantity · Transactions · Average Price, each with a custom icon.

**Slicers (interactive filters):**
- 📅 **Month** (vertical panel on the left)
- 📱 **Mobile Model** · 🏷 **Brand** · 💳 **Payment Method** · 📆 **Day Name** (dropdowns)

**Visuals:**

| Visual | Shows | Question answered |
|---|---|---|
| 🗺 **Map** | Total Sales by City (bubble size) | Where do we sell the most? |
| 📈 **Line chart** | Total Quantity by Month → Day (drill-down) | How does demand move over time? |
| 📊 **Column chart** | Total Sales by Mobile Model | Which models earn the most? |
| 🥧 **Pie chart** | Transactions by Payment Method (% of total) | How do customers pay? |
| 🔻 **Funnel** | Customer Ratings (1–5 ★) | How satisfied are customers? |
| 🏔 **Area chart** | Total Sales by Day of Week | Which days are busiest? |
| 📋 **Table** | Brand-wise Total Sales and Quantity | How do brands compare? |

All visuals **cross-filter** each other. For example, click *Apple* in the table and the map, charts and KPIs instantly show Apple-only figures.

---

## 💡 Key Insights

- 💰 **₹76.92 Cr in total sales** from 19,150 units across 3,835 transactions, at an average of **~5 units per transaction** and an average price of **₹40,114**.
- 🏷 **Very balanced brand competition:** Apple leads with **21.0%** of sales (₹16.16 Cr), followed closely by Samsung (20.8%), OnePlus (20.0%), Vivo (19.5%) and Xiaomi (18.7%). No brand dominates.
- 📱 **Top models:** iPhone SE (₹5.96 Cr), OnePlus Nord (₹5.79 Cr) and Galaxy Note 20 (₹5.60 Cr). The lowest are Redmi 9, Vivo S1 and OnePlus 9.
- 🏙 **Delhi and Mumbai drive 43% of all sales** (₹20.39 Cr and ₹12.72 Cr). Every other city is below ₹3.2 Cr, so these two metros are the key markets.
- 💳 **UPI is the most-used payment method** (26.4% of transactions). Debit card, credit card and cash are almost equal (~24–25% each), a clear sign of India's shift to digital payments.
- ⭐ **Customers are mostly satisfied:** 60.8% of ratings are 4★ or 5★ (5★ alone is 38.8%), but 22.3% are 1–2★, which is worth investigating.
- 📆 **Saturday and Monday** are the strongest sales days. **July** sells the most units and **February** the fewest.

---

## 🚀 How to Use

1. Install **[Power BI Desktop](https://powerbi.microsoft.com/desktop/)** (free, Windows).
2. Clone or download this repository:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   ```
3. Open **`Mobile_Sales_Dashboard.pbix`**.
4. **Point it to the data file** (needed once, because the file path is saved from the author's PC):
   *Home → Transform data → Data source settings → Change Source…* → select `data/Mobile Sales Data.xlsx` from this repo → **OK** → **Close & Apply**.
5. Use the slicers and click any chart element to explore.

> You can also explore the dashboard without changing the source. Power BI keeps the last loaded data inside the `.pbix`. Step 4 is only needed to **Refresh**.

---

## 📁 Repository Structure

```
├── Mobile_Sales_Dashboard.pbix   # Power BI report
├── data/
│   └── Mobile Sales Data.xlsx    # Source dataset
├── images/
│   └── dashboard.png             # Dashboard screenshot (used above)
└── README.md
```



## 👤 Author

**Sonali Kumari**

⭐ *If you found this project useful, please give it a star!*

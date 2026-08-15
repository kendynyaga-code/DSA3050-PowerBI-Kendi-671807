# Hotel Booking Analysis Dashboard

## 1. Dataset Source

The dataset used for this project is the **Hotel Booking Demand dataset**, provided in the `hotel_bookings.csv` file from Kaggle.

The dataset contains **119,390 booking records and 32 variables** covering two hotel types: **Resort Hotel** and **City Hotel**.

---

## 2. Dataset Description

The dataset contains booking-level information about reservations, customers, arrival dates, length of stay, room types, market segments, booking channels, cancellations, and pricing.

The data can be used to investigate hotel booking patterns, cancellation behavior, customer characteristics, demand, and pricing.

---

## 3. Dataset Selection Rationale

The dataset was selected because it contains detailed booking-level data with a combination of **numerical, categorical, and date-related variables**.

This makes it suitable for demonstrating data preparation, data modelling, DAX calculations, and interactive business intelligence dashboards using Power BI.

---

## 4. Main Variables

| Variable | Description |
| --- | --- |
| `hotel` | Type of hotel |
| `is_canceled` | Indicates whether a booking was cancelled |
| `lead_time` | Number of days between booking and arrival |
| `arrival_date_week_number` | Arrival week number |
| `stays_in_weekend_nights` | Weekend nights stayed |
| `stays_in_week_nights` | Weekday nights stayed |
| `adults` | Number of adults |
| `children` | Number of children |
| `babies` | Number of babies |
| `market_segment` | Market segment of the booking |
| `distribution_channel` | Booking distribution channel |
| `customer_type` | Type of customer |
| `reserved_room_type` | Room type originally reserved |
| `assigned_room_type` | Room type assigned |
| `deposit_type` | Deposit arrangement |
| `adr` | Average Daily Rate |
| `total_of_special_requests` | Number of special requests |
| `reservation_status` | Final reservation status |
| `reservation_status_date` | Reservation status date |

---

## 5. Business / Analytical Problem

Hotels need to understand booking patterns, customer behavior, cancellations, demand, and pricing to support better operational and business decisions.

The main analytical problem investigated is:

> **How can hotel booking data be used to identify patterns in bookings, cancellations, customer behavior, demand, and pricing to support better hotel decision-making?**

---

## 6. Analytical Questions

The Power BI solution addresses the following questions:

1. How do booking volumes vary between the Resort Hotel and City Hotel over time?
2. What proportion of bookings are cancelled?
3. How does cancellation rate differ between the two hotels?
4. Which market segments and customer types generate the most bookings?
5. How does lead time relate to cancellation behavior?
6. How does ADR vary across hotels, periods, and customer segments?
7. How do length-of-stay patterns differ between the two hotels?
8. Which countries contribute the most bookings?

---

## 7. Data Preparation and Cleaning

The raw dataset was prepared and transformed using **Power Query** before being loaded into the Power BI analytical model.

The major transformations are summarized using:

### **Problem → Transformation → Reason → Result**

| # | Transformation | Problem | Transformation / Reason | Result |
| --- | --- | --- | --- | --- |
| 1 | Standardizing missing values | Missing information appeared as `N/A` and `NULL` | Replaced these representations with Power Query `null` values | Consistent missing-value representation |
| 2 | Correcting data types | Fields required appropriate numerical, text, and date types | Changed data types using Power Query | Fields became suitable for analysis |
| 3 | Creating Arrival Date | Arrival year, month, and day were stored separately | Combined the three fields into one date field | Created `Arrival Date` |
| 4 | Creating Total Stay Nights | Weekend and weekday nights were separate | Added weekend and weekday nights | Created `Total Stay Nights` |
| 5 | Creating Total Guests | Adults, children, and babies were separate | Added the three guest categories | Created `Total Guests` |
| 6 | Creating Booking Outcome | `is_canceled` used numerical codes | Created a conditional column converting `0` and `1` into `Confirmed` and `Cancelled` | Business-friendly booking status |
| 7 | Removing redundant fields | Original arrival components were redundant after creating `Arrival Date` | Removed `arrival_date_year`, `arrival_date_month`, and `arrival_date_day_of_month` | Reduced redundancy |
| 8 | Extracting date attributes | Additional time attributes were required | Extracted Year, Month, and Quarter from `Arrival Date` | Enabled time-based analysis |
| 9 | Renaming fields | Raw fields used technical naming conventions | Renamed relevant fields using descriptive names | Improved readability |

## 8. Data Modelling

A **star-schema-based analytical model** was developed in Power BI.

The model consists of one central fact table supported by three dimension tables.

### 8.1 Fact Table

**`FactHotelBookings`**

The fact table contains the individual booking records and the numerical and categorical fields required for analysis.

Important fields include:

- Booking information
- Arrival Date
- Total Stay Nights
- Total Guests
- Lead Time
- Average Daily Rate
- Market Segment
- Customer Type
- Cancellation status

### 8.2 Dimension Tables

| Dimension | Purpose |
| --- | --- |
| `DimDate` | Supports year, month, quarter, and date analysis |
| `DimHotel` | Contains unique hotel types |
| `DimCustomer` | Contains customer-type categories |

`DimCustomer` represents **customer types rather than individual customers**, because the source dataset does not contain a unique customer identifier.

### 8.3 Relationships

The model uses one-to-many relationships with **single-direction filtering** from the dimensions to the fact table.

| Dimension | Fact Table | Cardinality | Filter Direction |
| --- | --- | --- | --- |
| `DimDate` | `FactHotelBookings` | 1:* | Single |
| `DimHotel` | `FactHotelBookings` | 1:* | Single |
| `DimCustomer` | `FactHotelBookings` | 1:* | Single |

This structure provides a clear analytical model while avoiding unnecessary many-to-many and bidirectional relationships.

## 9. DAX & Business Calculations

A total of **14 meaningful DAX measures** were developed. These measures include core KPIs, calculated business measures, and advanced DAX techniques.

## 9.1 DAX Measures

| # | Measure | Purpose | Main DAX Concept |
| --- | --- | --- | --- |
| 1 | `Total Bookings` | Counts all booking records | `COUNTROWS()` |
| 2 | `Cancelled Bookings` | Counts cancelled bookings | `CALCULATE()` |
| 3 | `Confirmed Bookings` | Counts confirmed bookings | `CALCULATE()` |
| 4 | `Total Guests Measure` | Calculates total guests | `SUM()` |
| 5 | `Average ADR` | Calculates average daily rate | `AVERAGE()` |
| 6 | `Average Stay Nights` | Calculates average stay duration | `AVERAGE()` |
| 7 | `Cancellation Rate %` | Calculates cancellation percentage | `DIVIDE()` |
| 8 | `Average Guests per Booking` | Calculates average booking size | `DIVIDE()` |
| 9 | `Average Lead Time` | Calculates average lead time | `AVERAGE()` |
| 10 | `Special Request Rate %` | Calculates percentage of bookings with special requests | `CALCULATE()`, `DIVIDE()` |
| 11 | `Previous Year Bookings` | Calculates previous-year bookings | `CALCULATE()`, `SAMEPERIODLASTYEAR()` |
| 12 | `YoY Booking Growth %` | Calculates year-over-year growth | `DIVIDE()` |
| 13 | `Booking Share %` | Calculates category share | `ALLSELECTED()` |
| 14 | `Hotel Booking Rank` | Ranks hotels by booking volume | `RANKX()` |

---

### 9.2 DAX Measures

### Total Bookings

```DAX
Total Bookings =
COUNTROWS('Fact Hotel Bookings')
```

Counts all booking records and provides the primary booking-volume KPI.

### Cancelled Bookings

```DAX
Cancelled Bookings =
CALCULATE(
    [Total Bookings],
    'Fact Hotel Bookings'[Is Canceled] = 1
)
```

Counts bookings where the cancellation indicator equals `1`.

### Confirmed Bookings

```DAX
Confirmed Bookings =
CALCULATE(
    [Total Bookings],
    'Fact Hotel Bookings'[Is Canceled] = 0
)
```

Counts bookings that were not cancelled.

### Total Guests Measure

```DAX
Total Guests Measure =
SUM('Fact Hotel Bookings'[Total Guests])
```

Calculates the total number of guests within the current filter context.

### Average ADR

```DAX
Average ADR =
AVERAGE('Fact Hotel Bookings'[Average Daily Rate])
```

Calculates the average daily rate across the selected bookings.

### Average Stay Nights

```DAX
Average Stay Nights =
AVERAGE('Fact Hotel Bookings'[Total Stay Nights])
```

Calculates the average length of stay.

### Cancellation Rate %

```DAX
Cancellation Rate % =
DIVIDE(
    [Cancelled Bookings],
    [Total Bookings],
    0
)
```

Calculates the percentage of bookings that were cancelled.

### Average Guests per Booking

```DAX
Average Guests per Booking =
DIVIDE(
    [Total Guests Measure],
    [Total Bookings],
    0
)
```

Calculates the average number of guests per booking.

### Average Lead Time

```DAX
Average Lead Time =
AVERAGE('Fact Hotel Bookings'[Lead Time])
```

Calculates the average number of days between booking and arrival.

### Special Request Rate %

```DAX
Special Request Rate % =
DIVIDE(
    CALCULATE(
        [Total Bookings],
        'Fact Hotel Bookings'[Special Requests] > 0
    ),
    [Total Bookings],
    0
)
```

Calculates the percentage of bookings containing at least one special request.

### Previous Year Bookings

```DAX
Previous Year Bookings =
CALCULATE(
    [Total Bookings],
    SAMEPERIODLASTYEAR(DimDate[Date])
)
```

Calculates bookings for the equivalent period in the previous year.

### YoY Booking Growth %

```DAX
YoY Booking Growth % =
DIVIDE(
    [Total Bookings] - [Previous Year Bookings],
    [Previous Year Bookings],
    0
)
```

Measures the percentage change in booking volume compared with the previous year.

### Booking Share %

```DAX
Booking Share % =
DIVIDE(
    [Total Bookings],
    CALCULATE(
        [Total Bookings],
        ALLSELECTED('Fact Hotel Bookings')
    ),
    0
)
```

Measures a category's contribution to the selected booking volume.

### Hotel Booking Rank

```DAX
Hotel Booking Rank =
RANKX(
    ALLSELECTED(DimHotel[Hotel]),
    [Total Bookings],
    ,
    DESC,
    DENSE
)
```

Ranks hotels according to their booking volume.

---

## 10. DAX Documentation

The following six measures were selected as the most important measures for detailed documentation, as required by the assessment brief.

## 10.1 Total Bookings

**What it calculates:**  
Counts all booking records.

**Why it is useful:**  
Provides the main measure of booking volume and serves as the basis for other calculations.

**Main DAX function:**  
`COUNTROWS()`

**Filter context:**  
Responds to filters such as hotel, date, customer type, and market segment.

**Dashboard use:**  
Used as a KPI on the Executive Overview page.

---

## 10.2 Cancellation Rate %

**What it calculates:**  
Calculates the percentage of bookings that were cancelled.

**Why it is useful:**  
Cancellation rate is an important indicator of booking reliability and potential occupancy risk.

**Main DAX functions:**  
`DIVIDE()` and `CALCULATE()` through the underlying cancelled-bookings measure.

**Filter context:**  
Recalculates when users filter by hotel, time period, market segment, customer type, or other dimensions.

**Dashboard use:**  
Used as a major KPI and in cancellation analysis.

---

## 10.3 Average ADR

**What it calculates:**  
Calculates the average Average Daily Rate.

**Why it is useful:**  
Provides insight into pricing levels across hotels, periods, and customer segments.

**Main DAX function:**  
`AVERAGE()`

**Filter context:**  
Changes according to the selected hotel, period, customer type, or market segment.

**Dashboard use:**  
Used in the Executive Overview and pricing analysis.

---

## 10.4 Average Stay Nights

**What it calculates:**  
Calculates the average number of nights per booking.

**Why it is useful:**  
Helps identify differences in length-of-stay behavior between hotels and customer groups.

**Main DAX function:**  
`AVERAGE()`

**Filter context:**  
Responds dynamically to hotel, date, customer, and market filters.

**Dashboard use:**  
Used in booking and customer analysis.

---

## 10.5 Previous Year Bookings

**What it calculates:**  
Calculates bookings for the equivalent period in the previous year.

**Why it is useful:**  
Provides a historical benchmark for evaluating booking performance.

**Main DAX functions:**  
`CALCULATE()` and `SAMEPERIODLASTYEAR()`.

**Filter context:**  
Uses the current date context from `DimDate` and shifts it to the corresponding previous-year period.

**Dashboard use:**  
Used for time-series and year-over-year analysis.

---

## 10.6 YoY Booking Growth %

**What it calculates:**  
Measures the percentage change in bookings compared with the previous year.

**Why it is useful:**  
Shows whether booking demand is increasing or decreasing.

**Main DAX function:**  
`DIVIDE()` using the current and previous-year booking measures.

**Filter context:**  
Changes according to the selected time period and other report filters.

**Dashboard use:**  
Used in the Executive Overview and booking trend analysis.

---

## 11. Professional Power BI Dashboards

Three report pages were developed following the required progression:

### **Overview → Detailed Analysis → Advanced/Diagnostic Analysis**

## 11.1 Page 1 — Executive Overview

The Executive Overview provides management with a high-level view of hotel booking performance.

The page includes KPI cards, booking trends, cancellation performance, hotel comparisons, key booking indicators, and interactive slicers

---

## 11.2 Page 2 — Detailed Analysis

The Detailed Analysis page provides deeper investigation of booking characteristics and performance.

The page examines factors such as booking volume, lead time, cancellation behavior, ADR, hotel performance, and customer and booking characteristics.

Interactive visuals allow users to explore the data through filtering and cross-filtering.

---

## 11.3 Page 3 — Advanced / Diagnostic Analysis

The Advanced/Diagnostic Analysis page investigates **why** booking outcomes may differ rather than only describing what happened.

The analysis examines relationships between cancellation behavior and factors such as lead time, ADR, hotel type, and other booking characteristics.

---

## 12. Dashboard Interactivity

The report demonstrates meaningful Power BI interactivity through:

- Slicers
- Cross-filtering
- Interactive visual selections
- Time-based filtering
- Hotel-level filtering
- Customer and booking-category filtering

These features allow users to move from overall performance to more detailed analysis.

---

## 13. Analytical Story

The three report pages follow the required analytical progression:

**What happened? → Where did it happen? → Why might it be happening? → What requires attention?**

### What happened?

The Executive Overview communicates overall booking volume, cancellation rate, ADR, and other major KPIs.

### Where did it happen?

The Detailed Analysis page compares booking behavior across hotels, customer groups, time periods, and booking characteristics.

### Why might it be happening?

The Advanced/Diagnostic Analysis page investigates relationships between cancellation behavior and factors such as lead time and ADR.

### What requires attention?

The analysis highlights differences in cancellation behavior, booking patterns, and pricing that may require further investigation by hotel management.

---

## 14. Conclusion

This project demonstrates the development of a complete **Power BI business intelligence solution** using the Hotel Booking Demand dataset.

The project covered:

- Power Query data preparation
- Data cleaning and transformation
- Star-schema data modelling
- 14 meaningful DAX measures
- Advanced DAX techniques
- Interactive dashboard development
- Executive, detailed, and diagnostic analysis

The resulting dashboard provides an interactive view of hotel booking performance and allows users to investigate booking volume, cancellations, customer behavior, length of stay, lead time, and pricing patterns.

The project demonstrates how raw booking data can be transformed into a structured analytical model and business-oriented insights for hotel decision-making.

---

# Hotel Booking Analysis Dashboard

## 1. Dataset Source

The dataset used for this project is the **Hotel Booking Demand dataset**, provided in the `hotel_bookings.csv` file from Kaggle

The dataset contains hotel booking records for two types of hotels: a **Resort Hotel** and a **City Hotel**. The dataset contains **119,390 records and 32 variables**, providing detailed information about individual hotel bookings.

---

## 2. Dataset Description

The dataset represents individual hotel booking records and contains information about reservations, customers, arrival dates, length of stay, room types, market segments, booking channels, cancellations, and other booking characteristics.

The dataset includes bookings from both a **Resort Hotel** and a **City Hotel** across multiple years.

The information can be used to investigate hotel booking patterns, cancellation behavior, customer characteristics, demand, and pricing-related patterns.

---

## 3. Dataset Selection Rationale

This dataset was selected because it contains detailed, booking-level information rather than already summarized results. This provides an opportunity to perform data preparation, transformation, analysis, and visualization using Power BI.

The dataset also contains a combination of **numerical, categorical, and date-related variables**, making it suitable for developing an interactive business intelligence dashboard.

The dataset provides sufficient information to investigate different aspects of hotel performance, including bookings, cancellations, customer behavior, length of stay, market segments, and pricing.

---

## 4. Main Variables

The dataset contains 32 variables. Some of the main variables used in the analysis include:

| Variable | Description |
| --- | --- |
| `hotel` | Type of hotel |
| `is_canceled` | Indicates whether the booking was canceled |
| `lead_time` | Number of days between booking and arrival |
| `arrival_date_year` | Year of arrival |
| `arrival_date_month` | Month of arrival |
| `arrival_date_week_number` | Week number of arrival |
| `arrival_date_day_of_month` | Day of arrival |
| `stays_in_weekend_nights` | Number of weekend nights stayed |
| `stays_in_week_nights` | Number of weekday nights stayed |
| `adults` | Number of adults |
| `children` | Number of children |
| `babies` | Number of babies |
| `meal` | Meal plan selected |
| `country` | Country of the guest |
| `market_segment` | Market segment of the booking |
| `distribution_channel` | Distribution channel used |
| `is_repeated_guest` | Indicates whether the guest is a repeated guest |
| `reserved_room_type` | Room type originally reserved |
| `assigned_room_type` | Room type assigned to the guest |
| `booking_changes` | Number of changes made to the booking |
| `deposit_type` | Type of deposit arrangement |
| `days_in_waiting_list` | Number of days the booking was on the waiting list |
| `customer_type` | Type of customer |
| `adr` | Average Daily Rate |
| `total_of_special_requests` | Number of special requests |
| `reservation_status` | Final status of the reservation |
| `reservation_status_date` | Date associated with the reservation status |

---

## 5. Business / Analytical Problem

Hotels need to understand booking patterns, customer behavior, cancellations, and pricing in order to improve their operations and support better business decisions.

The main analytical problem investigated in this project is:

> **How can hotel booking data be used to identify patterns in bookings, cancellations, customer behavior, demand, and pricing to support better hotel decision-making?**

The Power BI solution will analyze booking-level data to identify important patterns and relationships that can help understand hotel performance and customer behavior.

---

## 6. Analytical Questions

The Power BI dashboard will aim to answer the following questions:

1. **How do booking volumes vary between the Resort Hotel and City Hotel over time?**

2. **What proportion of bookings are canceled, and how does the cancellation rate differ between the two hotels?**

3. **Which market segments and customer types generate the most bookings?**

4. **How does booking lead time relate to cancellation behavior?**

5. **How does the Average Daily Rate (ADR) vary across hotels, seasons, customer types, and market segments?**

6. **How do length-of-stay patterns differ between the Resort Hotel and City Hotel?**

7. **Which countries contribute the most hotel bookings?**

8. **How do room types, booking channels, and special requests vary across hotel bookings?**

---

## 7. Data Preparation and Cleaning

The raw dataset was prepared and transformed using **Power Query** before being loaded into the analytical model. The transformations were selected to address data-quality issues and to create business-ready variables for analysis.

The major Power Query transformations are documented below using the following structure:

Problem → Transformation → Reason → Result

---

### 7.1 Standardizing Missing-Value Representations

**Problem:**
The raw dataset contained different textual representations of missing information, including values such as `N/A` and `NULL`.

**Transformation:**
Power Query's **Replace Values** functionality was used to replace the `N/A` and `NULL` representations with Power Query `null` values.

**Reason:**
Different representations of missing information could otherwise be interpreted as separate categorical values. Converting them to a consistent `null` representation allows Power Query to correctly recognize them as missing data.

**Result:**
Missing-value representations were standardized into Power Query `null` values, providing a consistent basis for further data preparation.

---

### 7.2 Correcting Data Types

**Problem:**
The raw dataset contained numerical, categorical, and date-related variables that needed to be interpreted using appropriate data types.

**Transformation:**
Power Query's **Changed Type** functionality was used to assign appropriate data types to the relevant fields.

**Reason:**
Correct data types are necessary for accurate calculations, sorting, filtering, relationships, and DAX calculations.

**Result:**
The dataset contains appropriately typed numerical, text, and date fields, providing a reliable foundation for subsequent analysis.

---

### 7.3 Creating Arrival Date

**Problem:**
The original arrival date was stored across three separate fields: `arrival_date_year`, `arrival_date_month`, and `arrival_date_day_of_month`.

**Transformation:**
A custom column named `Arrival Date` was created by combining the year, month, and day information into a single date field.

**Reason:**
A consolidated date field is more suitable for chronological analysis and allows the booking table to be connected to a dedicated Date dimension during data modelling.

**Result:**
Each booking now contains a single `Arrival Date` field representing the guest's arrival date.

---

### 7.4 Creating Total Stay Nights

**Problem:**
The duration of each booking was divided between `stays_in_weekend_nights` and `stays_in_week_nights`.

**Transformation:**
A custom column named `Total Stay Nights` was created by adding the weekend and weekday night values.

**Calculation:**

```text
Total Stay Nights = Weekend Nights + Weekday Nights
```

**Reason:**
Creating a single measure of stay duration makes it easier to analyze booking length and compare the average duration of stays across hotels, customer types, and other booking characteristics.

**Result:**
The dataset now contains a `Total Stay Nights` field representing the total number of nights associated with each booking.

---

### 7.5 Creating Total Guests

**Problem:**  
The number of guests in each booking was represented across three separate fields: `adults`, `children`, and `babies`. This made it difficult to analyze the total number of guests associated with each booking using a single field.

**Transformation:**  
A custom column named `Total Guests` was created by adding the values in the `adults`, `children`, and `babies` fields.

**Calculation:**

```text
Total Guests = Adults + Children + Babies
```

**Reason:**  
Creating a single total guest field makes it easier to analyze booking size and compare the number of guests across hotels, customer types, and market segments.

**Result:**  
The dataset now contains a `Total Guests` field representing the total number of adults, children, and babies associated with each booking.

---

### 7.6 Creating Booking Outcome

**Problem:**  
The original `is_canceled` field used numerical codes to represent booking status. A value of `0` indicated that a booking was not canceled, while `1` indicated that the booking was canceled. These numerical values were less intuitive for business users.

**Transformation:**  
A custom column named `Booking Outcome` was created using the `is_canceled` field. Since the field was stored as text, the following conditional logic was applied:

```text
if [is_canceled] = "1" then "Cancelled" else "Confirmed"
```

The resulting values were:

| `is_canceled` | `Booking Outcome` |
| --- | --- |
| `0` | Confirmed |
| `1` | Cancelled |

**Reason:**  
Converting numerical status codes into descriptive categories makes the field easier to understand and use in dashboard visuals, filters, and business analysis.

**Result:**  
The dataset now contains a business-friendly `Booking Outcome` field with two categories: `Confirmed` and `Cancelled`.

---

### 7.7 Removing Redundant Arrival Date Fields

**Problem:**  
After creating the consolidated `Arrival Date` field, the original arrival date components were no longer necessary in the main booking table.

**Transformation:**  
The following columns were removed:

- `arrival_date_year`
- `arrival_date_month`
- `arrival_date_day_of_month`

**Reason:**  
These fields were already represented by the newly created `Arrival Date` field. Keeping both representations would create unnecessary redundancy in the dataset.

**Result:**  
The main booking table now uses the consolidated `Arrival Date` field while the redundant year, month, and day columns have been removed.

---

### 7.8 Extracting Year, Month and Quarter from Arrival Date

**Problem:**  
Although a consolidated `Arrival Date` field had been created, additional date attributes were required for time-based analysis.

**Transformation:**  
Year, Month, and Quarter were extracted from the `Arrival Date` field using Power Query's date transformation functionality.

The following fields were created:

- `Arrival Year`
- `Arrival Month`
- `Arrival Quarter`

**Reason:**  
These derived fields allow booking patterns to be analyzed across different years, months, and quarters. They also provide useful fields for dashboard filtering and visualization.

**Result:**  
The dataset now contains additional time attributes that can be used to analyze booking trends and seasonality.

---

### 7.9 Renaming Fields Appropriately

**Problem:**  
Several fields in the raw dataset used technical naming conventions, including underscores and abbreviated names. These names were less readable and less suitable for presentation to business users.

**Transformation:**  
Relevant fields were renamed using clearer and more descriptive names.

Examples include:

| Original Field | New Field |
| --- | --- |
| `lead_time` | `Lead Time` |
| `adr` | `Average Daily Rate` |
| `market_segment` | `Market Segment` |
| `customer_type` | `Customer Type` |
| `stays_in_weekend_nights` | `Weekend Nights` |
| `stays_in_week_nights` | `Weekday Nights` |
| `total_of_special_requests` | `Special Requests` |

**Reason:**  
Clear and descriptive field names improve readability and make the Power BI data model, DAX calculations, and dashboard visuals easier for business users to interpret.

**Result:**  
The transformed dataset contains more descriptive and business-friendly field names.

---

### 7.10 Power Query Transformation Summary

| # | Transformation | Purpose |
| --- | --- | --- |
| 1 | Standardizing missing-value representations | Converted `N/A` and `NULL` representations to Power Query `null` values |
| 2 | Correcting data types | Ensured fields use appropriate data types |
| 3 | Creating Arrival Date | Combined separate arrival date components into one date field |
| 4 | Creating Total Stay Nights | Calculated the total length of each stay |
| 5 | Creating Total Guests | Calculated the total number of guests per booking |
| 6 | Creating Booking Outcome | Converted cancellation codes into descriptive booking statuses |
| 7 | Removing redundant arrival fields | Removed the original year, month, and day fields after creating `Arrival Date` |
| 8 | Extracting Year, Month and Quarter | Created additional time attributes for analysis |
| 9 | Renaming fields appropriately | Improved field readability and business interpretation |

The completed Power Query transformations prepared the raw hotel booking

---

## 8. Data Modelling

The cleaned hotel booking dataset was transformed into a **star-schema-based analytical model** in Power BI. The model consists of a central fact table containing booking-level records and supporting dimension tables containing descriptive attributes used to filter and analyze the bookings.

The model was designed to provide a clear separation between booking information and descriptive attributes while avoiding unnecessary many-to-many relationships, ambiguous filter paths, and inappropriate bidirectional relationships.

---

### 8.1 Fact Table

The main fact table is **`FactHotelBookings`**.

This table contains the individual hotel booking records and forms the centre of the analytical model. Each row represents a booking record and contains the numerical and categorical information required for analysis.

Important fields in the fact table include:

- `hotel`
- `is_canceled`
- `lead_time`
- `Arrival Date`
- `Total Stay Nights`
- `Total Guests`
- `market_segment`
- `customer_type`
- `adr`
- `reservation_status`
- `total_of_special_requests`

#### Why the Fact Table Was Selected

`FactHotelBookings` was selected as the fact table because the dataset is structured at the **individual booking level**. It contains the numerical and categorical information required to calculate business KPIs and analyze booking behavior.

The table therefore acts as the central source of booking-level information, while the dimension tables provide descriptive attributes for filtering and grouping the bookings.

---

### 8.2 DimHotel

The **`DimHotel`** table was created to contain the unique hotel categories in the dataset.

The dimension contains the following hotel types:

- City Hotel
- Resort Hotel

#### Why DimHotel Was Created

Hotel type is an important analytical dimension because many of the business questions involve comparing the performance and booking behavior of the City Hotel and Resort Hotel.

Separating hotel information into a dimension allows hotel-level filtering to be handled consistently and avoids unnecessary repetition of descriptive hotel categories in the analytical model.

---

### 8.3 DimCustomer

The **`DimCustomer`** table was created using the unique `Customer Type` categories available in the dataset.

The dimension allows bookings to be analyzed according to customer type, including categories such as:

- Contract
- Group
- Transient
- Transient-Party

#### Why DimCustomer Was Created

Customer type is an important descriptive attribute for understanding booking behavior and cancellation patterns.

The dimension allows users to filter and compare booking metrics across different customer types without duplicating the same customer-type categories throughout the model.

The source dataset does not contain a unique customer identifier. Therefore, `DimCustomer` represents **customer types rather than individual customers**.

---

### 8.4 DimDate

A dedicated **`DimDate`** table was created to support time-based analysis.

The Date dimension contains:

- `Date`
- `Year`
- `Month Number`
- `Month`
- `Quarter`

The date range was generated from the minimum and maximum `Arrival Date` values in the booking data.

#### Why DimDate Was Created

The hotel booking dataset contains arrival dates and supports analysis of booking patterns over time.

A dedicated Date dimension allows the report to consistently analyze bookings by:

- Year
- Month
- Quarter
- Date

It also provides a reliable foundation for time-based DAX calculations.

The `DimDate` table was marked as the dedicated Date Table in Power BI.

---

### 8.5 Relationships

One-to-many relationships were established between each dimension and the central fact table.

| Dimension Table | Fact Table | Relationship | Cardinality | Filter Direction |
| --- | --- | --- | --- | --- |
| `DimDate` | `FactHotelBookings` | `Date` → `Arrival Date` | 1:* | Single |
| `DimHotel` | `FactHotelBookings` | `Hotel` → `hotel` | 1:* | Single |
| `DimCustomer` | `FactHotelBookings` | `Customer Type` → `customer_type` | 1:* | Single |

The dimension tables are on the **one side** of each relationship because each dimension contains unique category or date values. The fact table is on the **many side** because the same hotel, customer type, or date can occur across many booking records.

---

### 8.6 Cardinality Decisions

A **one-to-many (1:*)** relationship was selected for all dimension-to-fact relationships.

This structure reflects the nature of the data:

- One hotel can have many bookings.
- One customer type can be associated with many bookings.
- One date can have many bookings.

Using one-to-many relationships avoids unnecessary many-to-many relationships and provides a clear flow of information from the dimensions to the booking fact table.

---

### 8.7 Filter Direction Decisions

All dimension-to-fact relationships use **single-direction filtering**.

The filter flows from the dimension table to the `FactHotelBookings` table.

For example, selecting **City Hotel** in a report filters the relevant booking records in the fact table.

Single-direction filtering was selected because it is sufficient for the analytical requirements of the model and reduces the risk of ambiguous filter paths.

Bidirectional filtering was avoided because it is unnecessary for this model and could introduce unwanted interactions between tables.

---

### 8.8 Data Types and Naming

Appropriate data types were established during the Power Query data preparation stage.

The model uses:

- Date data types for date fields.
- Numeric data types for numerical variables and measures.
- Text data types for categorical attributes.

Clear naming conventions were also used to distinguish between fact and dimension tables:

- `FactHotelBookings`
- `DimHotel`
- `DimCustomer`
- `DimDate`

Descriptive field names were used where appropriate to improve readability and make the model easier for business users to understand.

---

### 8.9 Modelling Challenges

One modelling challenge was that the original dataset does not contain a unique booking identifier or individual customer identifier.

Because there is no unique customer ID, an individual customer dimension could not be reliably created. Instead, `DimCustomer` was based on the available `Customer Type` attribute.

Another consideration was the presence of separate arrival year, month, and day fields in the raw dataset. These were first combined into the `Arrival Date` field during Power Query transformation. The redundant raw date components were then removed from the main fact table, and a dedicated `DimDate` table was created for time-based analysis.

These decisions were made to maintain a clear and technically defensible analytical model while avoiding unnecessary dimensions and relationships.

---

### 8.10 Final Model Structure

The final model follows a star-schema structure:

#### DimDate → FactHotelBookings ← DimHotel

#### DimCustomer → FactHotelBookings

`FactHotelBookings` forms the centre of the model, while `DimDate`, `DimHotel`, and `DimCustomer` provide descriptive attributes used to filter and analyze booking-level information.

The model uses one-to-many relationships with single-direction filtering and avoids unnecessary many-to-many or bidirectional relationships.

A screenshot of the completed **Power BI Model View** is included in the project documentation.

---

## 9. DAX & Business Calculations

After completing the Power Query data preparation and developing the analytical data model, DAX measures were created to convert the transformed hotel booking data into meaningful business indicators.

A total of **14 DAX measures** were developed. The measures include core booking KPIs, calculated business measures, and advanced calculations involving filter context, time intelligence, `ALLSELECTED()`, and `RANKX()`.

The measures were designed to answer the project's main analytical questions, particularly those related to booking volume, cancellations, customer behavior, pricing, length of stay, and changes in booking performance over time.

---

### 9.1 DAX Measures Created

| # | Measure | Purpose | Main DAX Concept |
| --- | --- | --- | --- |
| 1 | `Total Bookings` | Counts all booking records | `COUNTROWS()` |
| 2 | `Cancelled Bookings` | Counts cancelled bookings | `CALCULATE()` |
| 3 | `Confirmed Bookings` | Counts bookings that were not cancelled | `CALCULATE()` |
| 4 | `Total Guests Measure` | Calculates the total number of guests | `SUM()` |
| 5 | `Average ADR` | Calculates the average daily rate | `AVERAGE()` |
| 6 | `Average Stay Nights` | Calculates the average length of stay | `AVERAGE()` |
| 7 | `Cancellation Rate %` | Calculates the proportion of bookings that were cancelled | `DIVIDE()` |
| 8 | `Average Guests per Booking` | Calculates the average number of guests per booking | `DIVIDE()` |
| 9 | `Average Lead Time` | Calculates the average number of days between booking and arrival | `AVERAGE()` |
| 10 | `Special Request Rate %` | Calculates the percentage of bookings with at least one special request | `CALCULATE()`, `DIVIDE()` |
| 11 | `Previous Year Bookings` | Calculates bookings for the previous year | `CALCULATE()`, `SAMEPERIODLASTYEAR()` |
| 12 | `YoY Booking Growth %` | Measures year-over-year booking growth | `DIVIDE()`, Time Intelligence |
| 13 | `Booking Share %` | Calculates a category's share of selected bookings | `ALLSELECTED()`, `DIVIDE()` |
| 14 | `Hotel Booking Rank` | Ranks hotels according to booking volume | `RANKX()` |

---

##$ 9.2 Core DAX Measures

### 9.2.1 Total Bookings

**DAX:**
`Total Bookings = COUNTROWS('Fact Hotel Bookings')`

This measure counts the total number of booking records in the fact table. It provides the primary booking-volume KPI and forms the basis of several other business measures.

### 9.2.2 Cancelled Bookings

**DAX:**
`Cancelled Bookings = CALCULATE([Total Bookings], 'Fact Hotel Bookings'[Is Canceled] = 1)`

This measure counts bookings where `Is Canceled` is equal to 1. The measure uses `CALCULATE()` to modify the filter context so that only cancelled bookings are included.

### 9.2.3 Confirmed Bookings

**DAX:**
`Confirmed Bookings = CALCULATE([Total Bookings], 'Fact Hotel Bookings'[Is Canceled] = 0)`

This measure counts bookings that were not cancelled. It uses `CALCULATE()` to apply a filter where `Is Canceled` is equal to 0.

### 9.2.4 Total Guests Measure

**DAX:**
`Total Guests Measure = SUM('Fact Hotel Bookings'[Total Guests])`

This measure calculates the total number of guests across all bookings within the current filter context. The underlying `Total Guests` column was created during Power Query by combining the number of adults, children, and babies associated with each booking.

### 9.2.5 Average ADR

**DAX:**
`Average ADR = AVERAGE('Fact Hotel Bookings'[Average Daily Rate])`

This measure calculates the average Average Daily Rate across bookings. ADR provides an indication of the daily price associated with hotel bookings and can be compared across hotels, time periods, customer types, and market segments.

### 9.2.6 Average Stay Nights

**DAX:**
`Average Stay Nights = AVERAGE('Fact Hotel Bookings'[Total Stay Nights])`

This measure calculates the average number of nights per booking. The underlying `Total Stay Nights` field was created during Power Query by combining weekend and weekday nights.

---

### 9.3 Calculated Business Measures

### 9.3.1 Cancellation Rate %

**DAX:**
`Cancellation Rate % = DIVIDE([Cancelled Bookings], [Total Bookings], 0)`

**What it calculates:** The percentage of hotel bookings that were cancelled.

**Why it is useful:** Cancellation rate is an important business KPI because cancellations can affect occupancy planning, expected demand, and revenue. Comparing cancellation rates across hotels, customer types, market segments, and time periods can help identify areas requiring attention.

**Main DAX functions used:** `DIVIDE()` and the `[Cancelled Bookings]` and `[Total Bookings]` measures.

**Filter context:** The measure dynamically responds to the current filter context. For example, when a user selects a particular hotel, year, market segment, or customer type, both the numerator and denominator are recalculated for that selection.

**Dashboard use:** This measure will be used as a major KPI on the Executive Overview page and as an analytical metric on the Cancellation and Diagnostic Analysis page.

### 9.3.2 Average Guests per Booking

**DAX:**
`Average Guests per Booking = DIVIDE([Total Guests Measure], [Total Bookings], 0)`

This measure calculates the average number of guests associated with each booking. It is useful for understanding typical booking party size and can be compared across hotels, customer types, and market segments.

The measure uses `DIVIDE()` to safely calculate the ratio while avoiding division-by-zero errors.

### 9.3.3 Average Lead Time

**DAX:**
`Average Lead Time = AVERAGE('Fact Hotel Bookings'[Lead Time])`

This measure calculates the average number of days between the booking and the guest's arrival. Lead time is particularly useful when investigating cancellation behavior because bookings made far in advance may have different cancellation patterns from last-minute bookings.

### 9.3.4 Special Request Rate %

**DAX:**
`Special Request Rate % = DIVIDE(CALCULATE([Total Bookings], 'Fact Hotel Bookings'[Special Requests] > 0), [Total Bookings], 0)`

This measure calculates the percentage of bookings that contain at least one special request.

The `CALCULATE()` function modifies the filter context so that only bookings with more than zero special requests are counted.

This measure can be used to investigate customer behavior and service requirements across hotels and customer segments.

---

##$ 9.4 Advanced DAX Measures

### 9.4.1 Previous Year Bookings

**DAX:**
`Previous Year Bookings = CALCULATE([Total Bookings], SAMEPERIODLASTYEAR(DimDate[Date]))`

**What it calculates:** The number of bookings recorded during the equivalent period in the previous year.

**Why it is useful:** This measure provides a historical comparison that allows hotel booking performance to be evaluated against the previous year.

**Main DAX functions used:** `CALCULATE()` and `SAMEPERIODLASTYEAR()`.

**Filter context:** The calculation uses the current date context and shifts it to the equivalent period in the previous year using the dedicated `DimDate` table.

**Dashboard use:** The measure will be used in time-based visuals on the Executive Overview and Booking Analysis pages.

### 9.4.2 YoY Booking Growth %

**DAX:**
`YoY Booking Growth % = DIVIDE([Total Bookings] - [Previous Year Bookings], [Previous Year Bookings], 0)`

**What it calculates:** The percentage change in booking volume compared with the previous year.

**Why it is useful:** Year-over-year growth provides a clear measure of whether booking demand is increasing or decreasing over time.

**Main DAX functions used:** `DIVIDE()` and the `[Previous Year Bookings]` measure.

**Filter context:** The result changes according to the selected time period and other report filters. The measure compares the current booking count with the equivalent previous-year period.

**Dashboard use:** This measure will be used in the Executive Overview and time-trend analysis to communicate changes in booking performance.

### 8.4.3 Booking Share %

**DAX:**
`Booking Share % = DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALLSELECTED('Fact Hotel Bookings')), 0)`

**What it calculates:** The proportion of selected bookings represented by the current category.

**Why it is useful:** Booking share allows categories such as hotels, market segments, or customer groups to be compared based on their contribution to the selected booking volume.

**Main DAX functions used:** `DIVIDE()`, `CALCULATE()`, and `ALLSELECTED()`.

**Filter context:** `ALLSELECTED()` allows the measure to respect the user's broader report selections while removing the specific category context being evaluated. This allows each category's contribution to be compared against the selected total.

**Dashboard use:** The measure can be used in category-level visuals, such as comparing booking contribution between the Resort Hotel and City Hotel.

### 9.4.4 Hotel Booking Rank

**DAX:**
`Hotel Booking Rank = RANKX(ALLSELECTED(DimHotel[Hotel]), [Total Bookings], , DESC, DENSE)`

**What it calculates:** Ranks hotels according to their booking volume.

**Why it is useful:** Ranking allows hotel performance to be compared directly and identifies which hotel contributes the greatest number of bookings.

**Main DAX functions used:** `RANKX()` and `ALLSELECTED()`.

**Filter context:** The ranking respects broader report selections while comparing the hotels within the selected context.

**Dashboard use:** The measure can be used in hotel comparison tables and analytical visuals to identify the highest-volume hotel.

---

##$ 9.5 Six Most Important DAX Measures

The following six measures were selected for detailed documentation because they are the most directly connected to the project's business questions and dashboard analysis.

### 1. Total Bookings

**What it calculates:** Counts all booking records in the fact table.

**Why it is useful:** Total bookings provide the primary measure of booking volume and serve as a baseline for other calculations such as cancellation rate and booking growth.

**Main DAX functions:** `COUNTROWS()`.

**Filter context:** The measure automatically responds to filters such as hotel, year, month, market segment, and customer type.

**Dashboard use:** Used as a primary KPI on the Executive Overview page.

### 2. Cancellation Rate %

**What it calculates:** Calculates the percentage of bookings that were cancelled.

**Why it is useful:** Cancellation rate is a major indicator of booking reliability and potential occupancy and revenue risk.

**Main DAX functions:** `DIVIDE()` with `[Cancelled Bookings]` and `[Total Bookings]`.

**Filter context:** The measure dynamically recalculates when users filter the report by hotel, year, market segment, customer type, or other dimensions.

**Dashboard use:** Used as a KPI on the Executive Overview and for cancellation analysis.

### 3. Average ADR

**What it calculates:** Calculates the average Average Daily Rate across bookings.

**Why it is useful:** ADR provides an indication of pricing levels and allows comparison of pricing patterns across hotels, periods, and customer segments.

**Main DAX functions:** `AVERAGE()`.

**Filter context:** The result changes according to the selected hotel, time period, customer type, market segment, or other report filters.

**Dashboard use:** Used as a KPI and in pricing analysis visuals.

### 4. Average Stay Nights

**What it calculates:** Calculates the average number of nights associated with a booking.

**Why it is useful:** Length of stay provides insight into customer behavior and allows comparison between the Resort Hotel and City Hotel.

**Main DAX functions:** `AVERAGE()`.

**Filter context:** The measure responds dynamically to hotel, time, customer, and market segment filters.

**Dashboard use:** Used as a KPI and in hotel and customer analysis.

### 5. Previous Year Bookings

**What it calculates:** Calculates the booking volume for the equivalent period in the previous year.

**Why it is useful:** It provides a historical benchmark for evaluating changes in booking demand.

**Main DAX functions:** `CALCULATE()` and `SAMEPERIODLASTYEAR()`.

**Filter context:** The measure uses the current date context from `DimDate` and shifts it to the corresponding previous-year period.

**Dashboard use:** Used in time-series analysis and year-over-year comparisons.

### 6. YoY Booking Growth %

**What it calculates:** Calculates the percentage change in bookings compared with the previous year.

**Why it is useful:** It provides a direct measure of whether booking demand is growing or declining over time.

**Main DAX functions:** `DIVIDE()` and `[Previous Year Bookings]`.

**Filter context:** The measure changes according to the selected time period and other report filters.

**Dashboard use:** Used on the Executive Overview and booking trend analysis to communicate changes in performance.

---

### 9.6 Summary of Advanced DAX Techniques

The DAX solution demonstrates several advanced concepts required by the examination brief.

| Technique | Example Measure | Purpose |
| --- | --- | --- |
| `CALCULATE()` | Cancelled Bookings | Modifies filter context |
| `DIVIDE()` | Cancellation Rate % | Performs safe ratio calculations |
| `SAMEPERIODLASTYEAR()` | Previous Year Bookings | Performs time-intelligence analysis |
| `ALLSELECTED()` | Booking Share % | Controls filter context while preserving user selections |
| `RANKX()` | Hotel Booking Rank | Ranks categories according to a measure |
| Measure branching | Cancellation Rate % | Builds business measures from existing measures |
| Filter context | Multiple measures | Allows results to dynamically respond to slicers and report selections |

The DAX calculations therefore move beyond simple aggregation and provide business-oriented metrics that respond dynamically to the analytical context of the Power BI report. The measures will form the quantitative foundation of the interactive dashboards developed in the next stage of the project.

---

---

## 10. Professional Power BI Dashboards

The final Power BI report was developed as a three-page interactive dashboard following the progression:

### Executive Overview → Detailed Analysis → Advanced/Diagnostic Analysis

The dashboard was designed to move from understanding overall hotel booking performance to investigating specific booking patterns and finally identifying factors associated with cancellation behavior and other important business outcomes.

The report uses a consistent visual theme, KPI cards, charts, slicers, and interactive filtering to communicate the main findings from the hotel booking dataset.

---

### 10.1 Dashboard Design Approach

The dashboard was designed according to the following principles:

- Clear visual hierarchy
- Consistent formatting and spacing
- Consistent color palette
- Appropriate visual selection
- KPI-driven presentation
- Interactive filtering
- Business-oriented storytelling
- Minimal visual clutter
- Clear progression from overview to detailed analysis

The report follows the analytical storytelling approach:

> **What happened? → Where did it happen? → Why is it happening? → What requires attention?**

Users can interact with the report using slicers and cross-filtering to investigate the results across different hotels, time periods, customer types, and other booking characteristics.

---

### 10.2 Page 1 – Executive Overview

The **Executive Overview** page provides a high-level summary of hotel booking performance.

The purpose of this page is to allow a manager or decision-maker to understand the major characteristics of the dataset and the overall booking situation quickly without having to examine individual records.

### Key information presented

The page includes KPI cards and visualizations covering:

- Total Bookings
- Cancelled Bookings
- Cancellation Rate
- Average Daily Rate (ADR)
- Average Stay Nights
- Total Guests
- Booking trends over time
- Hotel-level comparisons
- Booking distribution across important categories

Interactive slicers allow users to investigate the KPIs and visualizations according to relevant dimensions such as hotel and time period.

### Business purpose – Executive Overview

The Executive Overview answers questions such as:

- How many bookings were recorded?
- How many bookings were cancelled?
- What is the overall cancellation rate?
- What is the average booking price?
- How long do guests typically stay?
- How does booking activity change over time?
- How do the City Hotel and Resort Hotel compare?

This page provides management with an immediate understanding of overall hotel booking performance.

### Screenshot: Executive Overview Dashboard

The completed Executive Overview dashboard is documented in the project screenshots as:

`screenshots/10_dashboard_overview.png`

---

### 10.3 Page 2 – Detailed Booking Analysis

The **Detailed Booking Analysis** page provides a deeper examination of booking behavior and the characteristics associated with hotel reservations.

While the Executive Overview focuses on the overall picture, this page allows users to investigate specific patterns across booking characteristics and customer-related dimensions.

### Key areas of analysis – Detailed Booking

The page includes visualizations examining:

- Booking volume by hotel
- Booking patterns over time
- Booking lead time
- Cancellation behavior
- Average Daily Rate (ADR)
- Customer and market segment patterns
- Length of stay
- Other relevant booking characteristics

Interactive filters allow the user to investigate these patterns for different hotels and time periods.

### Business purpose – Detailed Booking Analysis

This page helps answer questions such as:

- Which hotel receives more bookings?
- How does booking lead time vary across booking groups?
- How does ADR differ between booking categories?
- How do customer and market segments contribute to booking volume?
- How do length-of-stay patterns differ between hotels?
- What booking characteristics are associated with higher cancellation levels?

The page therefore moves from the high-level performance shown on the Executive Overview to more detailed analysis of booking behavior.

### Screenshot: Detailed Booking Analysis Dashboard

The completed Detailed Booking Analysis dashboard is documented in the project screenshots as:

`screenshots/11_dashboard_analysis.png`

---

### 10.4 Page 3 – Advanced / Diagnostic Analysis

The **Advanced/Diagnostic Analysis** page investigates relationships and patterns that may help explain hotel booking outcomes.

Rather than only showing what happened, this page focuses on identifying potential factors associated with important outcomes, particularly cancellation behavior.

One of the key analytical relationships investigated is the relationship between **lead time, ADR, and cancellation rate**.

### Key areas of analysis – Advanced / Diagnostic

The page includes analysis of:

- Cancellation rate by lead time
- Cancellation rate across booking characteristics
- ADR and cancellation behavior
- Hotel-level cancellation differences
- Customer and market segment cancellation patterns
- Other diagnostic relationships relevant to booking outcomes

### Business purpose – Advanced / Diagnostic Analysis

This page helps answer questions such as:

- Does cancellation behavior change as booking lead time increases?
- Are bookings with longer lead times associated with higher cancellation rates?
- How does ADR relate to cancellation behavior?
- Which hotel or customer segments experience higher cancellation rates?
- Which booking characteristics may require further management attention?

The purpose is not to claim that these relationships prove causation, but rather to identify **patterns and areas that may warrant further investigation**.

### Screenshot: Advanced / Diagnostic Analysis Dashboard

The completed Advanced/Diagnostic Analysis dashboard is documented in the project screenshots as:

`screenshots/12_cancellation_and_diagnostic_analysis.png`

---

### 10.5 Dashboard Interactivity

The Power BI report incorporates interactive features to allow users to explore the analysis rather than viewing only static results.

### Slicers

Slicers allow users to filter the report according to relevant dimensions such as:

- Hotel
- Year
- Month
- Customer Type
- Other available booking characteristics

### Cross-filtering

Power BI's cross-filtering functionality allows selections made in one visual to affect related visuals on the same report page.

For example, selecting a particular hotel allows users to investigate how its booking volume, cancellation rate, ADR, and other metrics compare with the overall results.

### Dynamic analytical context

The DAX measures were designed to respond to the current filter context. Therefore, KPI cards and analytical measures can change dynamically when users interact with slicers and visuals.

This allows users to move from overall performance to specific subsets of the hotel booking data.

---

### 10.6 Dashboard Storytelling

The three report pages were designed to provide a logical analytical flow:

| Page | Focus | Main Question |
| --- | --- | --- |
| Page 1 – Executive Overview | Overall performance | **What happened?** |
| Page 2 – Detailed Analysis | Booking and customer patterns | **Where and how did it happen?** |
| Page 3 – Advanced/Diagnostic Analysis | Cancellation and behavioral relationships | **Why might it be happening?** |

This structure allows the user to begin with the overall performance of the hotels, investigate important booking patterns, and then move toward deeper diagnostic analysis.

The dashboard therefore follows the intended progression of:

> **Overview → Detailed Analysis → Deeper Insights**

---

### 10.7 Key Dashboard Insights

The dashboards provide several important observations from the hotel booking data.

### Booking Volume

The dataset contains **119,390 booking records**, providing a substantial basis for comparing booking behavior across the City Hotel and Resort Hotel.

### Cancellation Behavior

Cancellation rate is one of the primary KPIs in the report. The dashboard allows cancellation behavior to be examined across hotels, time periods, customer types, market segments, and lead-time categories.

### Pricing

Average Daily Rate (ADR) is used to investigate differences in booking prices across hotels and other booking characteristics.

### Length of Stay

Total Stay Nights and Average Stay Nights provide insight into the duration of hotel visits and allow comparisons between different booking groups.

### Booking Lead Time

Lead time is examined as an important booking characteristic and is used in the diagnostic analysis to investigate its relationship with cancellation behavior.

These insights are presented interactively in the Power BI report rather than being limited to a single static summary.

---

### 10.8 Dashboard Screenshots

The following screenshots provide evidence of the completed Power BI report:

| Screenshot | Description |
| --- | --- |
| `screenshots/10_dashboard_overview.png` | Executive Overview |
| `screenshots/11_dashboard_analysis.png` | Detailed Booking Analysis |
| `screenshots/12_cancellation_and_diagnostic_analysis.png` | Advanced / Diagnostic Analysis |

The screenshots demonstrate the final dashboard design, visualizations, KPIs, and analytical outputs produced using Power BI.

---

### 10.9 Final Dashboard Outcome

The completed Power BI solution combines:

- Power Query data preparation
- Star-schema data modelling
- DAX business calculations
- Interactive visualizations
- KPI reporting
- Detailed booking analysis
- Diagnostic analysis

The final report transforms the raw hotel booking dataset into an interactive business intelligence solution that can be used to investigate booking performance, cancellation behavior, customer patterns, pricing, and length-of-stay characteristics.

---

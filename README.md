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

## 7.10 Power Query Transformation Summary

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

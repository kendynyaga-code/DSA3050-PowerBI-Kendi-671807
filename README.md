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
|---|---|
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
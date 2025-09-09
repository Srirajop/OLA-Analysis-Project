# 🚖 OLA Analysis Project

This project uses SQL and Power BI to tackle the business challenge of optimizing a ride-booking service. By analyzing booking statuses, customer behavior, and driver performance, I identified key trends to inform strategic decisions. The final Power BI dashboard provides actionable insights on revenue, sales, and vehicle performance, directly addressing the need for data-driven service improvement.

## 🎥 Demo

<img src="https://github.com/user-attachments/assets/ec96075c-619a-488a-8219-be75ded65077" alt="GIF of the Power BI dashboard" width="800">

> [!NOTE]
> Click the dropdown lists below for more information on SQL or Power BI.

---

<details>
<summary>SQL 📊</summary>

# 🚖 OLA Data Analyst Project SQL

## 📂 Introduction to the Database

This project involves analyzing ride bookings data for a ride-hailing service, "OLA." The database contains various tables (e.g., `bookings`, `customers`, and `drivers`) that store information about ride bookings, customer ratings, driver ratings, vehicle types, payment methods, and more.

The main objective of this project is to extract meaningful insights and statistics using SQL queries. This document showcases a set of queries to answer specific analytical questions about the business performance and customer behavior.

### 🛠️ How the Database Works

- **📊 Tables**: The database is primarily focused on the `bookings` table, which contains the following key columns:

  - `Booking_ID`: Unique identifier for each ride.
  - `Customer_ID`: ID of the customer who booked the ride.
  - `Vehicle_Type`: Type of vehicle used (e.g., Prime Sedan, Auto, etc.).
  - `Booking_Status`: Status of the ride (e.g., `Success`, `Cancelled by Customer`, etc.).
  - `Ride_Distance`: Distance covered in the ride (in kilometers).
  - `Payment_Method`: Mode of payment used for the ride (e.g., UPI, Card, Cash).
  - `Driver_Ratings`: Ratings provided by customers to the drivers (out of 5).
  - `Customer_Rating`: Ratings provided by drivers to the customers (out of 5).
  - `Booking_Value`: Monetary value of the completed ride.
  - `Incomplete_Rides`: A flag to indicate whether the ride was completed or not.
  - `Incomplete_Rides_Reason`: If the ride was incomplete, this column stores the reason.

- **🔍 Views**: This document includes SQL `CREATE VIEW` statements to predefine specific datasets and make querying simpler for repetitive tasks.

- **📈 Key Insights**: Using SQL, we retrieve data that helps us answer questions such as:
  - The top-performing customers.
  - Average ratings and distances.
  - Trends in cancellations by drivers and customers.
  - The total revenue from successful rides.

---

## 🏗️ Database Setup

```sql
CREATE DATABASE Ola;
USE Ola;
## 📂 Importing Data into MySQL Workbench

To work with the database, we first need to import the data from the `bookings.csv` file into MySQL Workbench. Follow these steps:

1. **Open MySQL Workbench**:

   - Launch MySQL Workbench and connect to your database server.

2. **Select the Database**:

   - Use the `Ola` database by running:
     ```sql
     USE Ola;
     ```

3. **Go to the Import Section**:

   - Click on the "Server" menu and select "Data Import."

4. **Choose the CSV File**:

   - In the "Import" tab, choose the `bookings.csv` file as the source.
   - Ensure the "Import Data from File" option is selected.

5. **Map the Table**:

   - Select the destination table (`bookings`).
   - Map the CSV columns to the corresponding table columns.

6. **Run the Import**:

   - Click on "Start Import."

7. **Verify the Data**:
   - After importing, verify the data using:
     ```sql
     SELECT * FROM bookings LIMIT 10;
     ```

---

## 📜 SQL Queries & Answers

### 1️⃣ Retrieve all successful bookings:

**📝 Query:**

```sql
CREATE VIEW Successful_Bookings AS
SELECT *
FROM bookings
WHERE Booking_Status = 'Success';
```

**📊 Answer:**

```sql
SELECT * FROM Successful_Bookings;
```

<img width="1235" height="256" alt="image" src="https://github.com/user-attachments/assets/6eeb3e0d-76a5-463a-bc70-9fb4ee72ee71" />


---

### 2️⃣ Find the average ride distance for each vehicle type:

**📝 Query:**

```sql
CREATE VIEW ride_distance_for_each_vehicle AS
SELECT Vehicle_Type, AVG(Ride_Distance) AS avg_distance
FROM bookings
GROUP BY Vehicle_Type;
```

**📊 Answer:**

```sql
SELECT * FROM ride_distance_for_each_vehicle;
```

<img width="201" height="176" alt="image" src="https://github.com/user-attachments/assets/c568a348-67c0-4777-863d-a56003aab5af" />


---

### 3️⃣ Get the total number of cancelled rides by customers:

**📝 Query:**

```sql
CREATE VIEW cancelled_rides_by_customers AS
SELECT COUNT(*) AS total_cancelled_rides
FROM bookings
WHERE Booking_Status = 'cancelled by Customer';
```

**📊 Answer:**

```sql
SELECT * FROM cancelled_rides_by_customers;
```

<img width="114" height="53" alt="image" src="https://github.com/user-attachments/assets/f76e28b3-d47a-4c7f-814f-48b258fe8c84" />


---

### 4️⃣ List the top 5 customers who booked the highest number of rides:

**📝 Query:**

```sql
CREATE VIEW Top_5_Customers AS
SELECT Customer_ID, COUNT(Booking_ID) AS total_rides
FROM bookings
GROUP BY Customer_ID
ORDER BY total_rides DESC
LIMIT 5;
```

**📊 Answer:**

```sql
SELECT * FROM Top_5_Customers;
```

<img width="214" height="127" alt="image" src="https://github.com/user-attachments/assets/9032a394-730a-4a7e-bd64-1e08d8892ea1" />


---

### 5️⃣ Get the number of rides cancelled by drivers due to personal and car-related issues:

**📝 Query:**

```sql
CREATE VIEW Rides_cancelled_by_Drivers_P_C_Issues AS
SELECT COUNT(*) AS cancelled_by_drivers
FROM bookings
WHERE cancelled_Rides_by_Driver = 'Personal & Car related issue';
```

**📊 Answer:**

```sql
SELECT * FROM Rides_cancelled_by_Drivers_P_C_Issues;
```

<img width="107" height="58" alt="image" src="https://github.com/user-attachments/assets/c3805c55-6564-4e42-9277-be6db576d4a0" />


---

### 6️⃣ Find the maximum and minimum driver ratings for Prime Sedan bookings:

**📝 Query:**

```sql
CREATE VIEW Max_Min_Driver_Rating AS
SELECT MAX(Driver_Ratings) AS max_rating,
       MIN(Driver_Ratings) AS min_rating
FROM bookings
WHERE Vehicle_Type = 'Prime Sedan';
```

**📊 Answer:**

```sql
SELECT * FROM Max_Min_Driver_Rating;
```
<img width="209" height="52" alt="image" src="https://github.com/user-attachments/assets/06c3003d-0296-4524-bea7-84556f2a36dd" />


---

### 7️⃣ Retrieve all rides where payment was made using UPI:

**📝 Query:**

```sql
CREATE VIEW UPI_Payment AS
SELECT *
FROM bookings
WHERE Payment_Method = 'UPI';
```

**📊 Answer:**

```sql
SELECT * FROM UPI_Payment;
```
<img width="209" height="52" alt="image" src="https://github.com/user-attachments/assets/d69da267-90bc-41ec-a525-2f9dcb2f1d6a" />


---

### 8️⃣ Find the average customer rating per vehicle type:

**📝 Query:**

```sql
CREATE VIEW AVG_Cust_Rating AS
SELECT Vehicle_Type, AVG(Customer_Rating) AS avg_customer_rating
FROM bookings
GROUP BY Vehicle_Type;
```

**📊 Answer:**

```sql
SELECT * FROM AVG_Cust_Rating;
```

<img width="296" height="174" alt="image" src="https://github.com/user-attachments/assets/297284cc-3448-458a-b316-5a3e62a804ac" />


---

### 9️⃣ Calculate the total booking value of rides completed successfully:

**📝 Query:**

```sql
CREATE VIEW total_successful_ride_value AS
SELECT SUM(Booking_Value) AS total_successful_ride_value
FROM bookings
WHERE Booking_Status = 'Success';
```

**📊 Answer:**

```sql
SELECT * FROM total_successful_ride_value;
```

<img width="187" height="45" alt="image" src="https://github.com/user-attachments/assets/71e13bf4-081c-4c5d-ab2f-d64580d757c2" />

---

### 🔟 List all incomplete rides along with the reason:

**📝 Query:**

```sql
CREATE VIEW Incomplete_Rides_Reason AS
SELECT Booking_ID, Incomplete_Rides_Reason
FROM bookings
WHERE Incomplete_Rides = 'Yes';
```

**📊 Answer:**

```sql
SELECT * FROM Incomplete_Rides_Reason;
```

<img width="333" height="272" alt="image" src="https://github.com/user-attachments/assets/3b03de85-baba-4ba6-ac98-d99488af5450" />





</details>

<details>
    <summary>Power BI 📈</summary>

# OLA Data Analysis in Power BI 📊

This Power BI project provides a comprehensive analysis of OLA's operational and customer data, focusing on ride volume, customer ratings, revenue, and performance metrics. The analysis leverages dynamic dashboards, interactive charts, and key performance indicators (KPIs) to identify trends and insights.

## ✨ Key Features

📌 **Ride Volume Analysis**: Tracks ride volume over time, helping to identify peak demand periods.

📌 **Booking Status Breakdown**: Visualizes the proportion of completed, canceled, and pending rides.

📌 **Top 5 Vehicle Types**: Highlights the most popular vehicle types based on ride distance.

📌 **Cancellation Insights**: Analyzes reasons for ride cancellations to improve customer experience.

📌 **Revenue Insights**: Breaks down revenue by payment methods to understand customer preferences.

📌 **Top Customers**: Identifies the top 5 customers based on their total booking value.

📌 **Driver Ratings**: Analyzes driver rating distribution to ensure service quality.

📌 **Customer vs. Driver Ratings**: Compares customer and driver ratings to identify gaps in satisfaction.

---
---

## 🛠️ Tools Used:

**Power BI**: For creating dashboards, visualizations, and interactive reports.

**SQL**: For querying, aggregating, and preparing data for analysis.

**Excel/CSV**: For preprocessing and cleaning raw data.

## 🚀 Steps in Project

✔️ Requirement Gathering / Business Requirements

✔️ Data Extraction

✔️ Data Walkthrough

✔️ Data Cleaning

✔️ Data Modeling

✔️ DAX Calculations

✔️ Dashboard Layout Design

✔️ Chart Development and Formatting

✔️ Dashboard / Report Development

✔️ Insights Generation

✔️ Report Presentation

## 🧑‍💼 Business Requirement

To conduct a comprehensive analysis of OLA's ride data, focusing on key aspects such as ride volume, booking status, and vehicle types. The analysis will also include customer and driver ratings, reasons for canceled rides, and revenue by payment method. Additionally, it will identify the top customers by total booking value and examine the distribution of ride distances per day. The goal is to provide actionable insights that can help optimize OLA's services and improve overall performance.

## 📈 KPI’s Requirements

**1. Total Sales:** The overall revenue generated from all items sold.

**2. Average Sales:** The average revenue per sale.

**3. Number of Items:** The total count of different items sold.

**4. Average Rating:** The average customer rating from items sold.

## 📊 Chart’s Requirements

<ol>  
<h3><li> Overall 📅</li></h3>  
<ul>  
  <li>Ride Volume Over Time: Visualize ride volume trends over time.</li>  
  <li>Booking Status Breakdown: Display the distribution of completed, canceled, and pending bookings.</li>  
  <br>
<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
    <img width="1425" height="809" alt="image" src="https://github.com/user-attachments/assets/08bf7ad2-3cd3-42d8-be59-a07bcdb340d4" />


</div>
</ul>

<h3><li> Vehicle Type 🚗:</li></h3>  
<ul>  
  <li>Top 5 Vehicle Types by Ride Distance: Show the top 5 vehicle types based on ride distance.</li>  
  <br>
<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
    <img width="1427" height="799" alt="image" src="https://github.com/user-attachments/assets/446faded-60fe-44fc-aa34-f55a2ac694e3" />


</div>
</ul>

<h3><li> Revenue 💰:</li></h3>  
<ul>  
  <li>Revenue by Payment Method: Visualize total revenue generated by different payment methods.</li>  
  <li>Top 5 Customers by Total Booking Value: Identify the top 5 customers with the highest total booking value.</li>  
  <li>Ride Distance Distribution Per Day: Display the distribution of ride distances on a daily basis.</li> 
  <br>
<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
    <img width="1424" height="801" alt="image" src="https://github.com/user-attachments/assets/f7c9e861-0bd2-4094-aca2-e05b4de0a4ec" />


</div> 
</ul>

<h3><li> Cancellation 🚫:</li></h3>  
<ul>  
  <li>Cancelled Rides Reasons (Customer): Show the reasons behind canceled rides by customers.</li>  
  <li>Cancelled Rides Reasons (Driver): Show the reasons behind canceled rides by drivers.</li>  
  <br>
<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
    <img width="1419" height="801" alt="image" src="https://github.com/user-attachments/assets/6fb4e676-52a1-473b-8784-c68e3a0b209a" />


</div>
</ul>

<h3><li> Ratings 🌟:</li></h3>  
<ul>  
  <li>Driver Ratings: Display the distribution of ratings given to drivers.</li>  
  <li>Customer Ratings: Display the distribution of ratings given by customers.</li>  
  <br>
<div style="display: flex; justify-content: center; align-items: center; gap: 20px;">
    <img width="1427" height="798" alt="image" src="https://github.com/user-attachments/assets/fd02cf83-79f2-4eec-a6a9-954fd2fa574f" />

</div>
</ul>
</ol>

## Dashboard Insights

### Key Insights 🔑:

1. **Ride Volume Trends 📉**: Identify peak times and demand fluctuations.
2. **Booking Status Insights 📋**: Understand the distribution of booking statuses.
3. **Vehicle Type Performance 🚗**: Discover the most effective vehicle types.
4. **Revenue Patterns 💸**: Track payment method usage and high-value customers.
5. **Cancellation Analysis 🔄**: Pinpoint reasons for ride cancellations.

### 🎛 Interactive Features:

- Drill-through options to explore details at multiple levels.
- Custom slicers for dynamic filtering.
- KPIs displayed in real-time visuals.

## How to Use 📋

1. Download the Power BI file: `Ola DA Project.pbix`
2. Open the file in **Power BI Desktop**.
3. Explore the dashboards and insights interactively.


## Contact 📧

For any queries or feedback, feel free to reach out:

- **Name**: Sriraj Pillai
- **Email**: sriraj2104@gmail.com


## 🙌 Acknowledgments

A big shoutout to [Top VarSity](https://www.youtube.com/@TopVarSity) for their helpful tutorial that guided this project. A heartfelt thanks to Top VarSity for sharing valuable insights in their YouTube video tutorial, which can be found [here](https://www.youtube.com/watch?si=29Ikp70AdbmvziIh&v=1uPUyT9LoHQ&feature=youtu.be). Your content played a significant role in shaping the success of this project!















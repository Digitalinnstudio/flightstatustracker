# Flight Status & Performance Dashboard (FAA Case Study)
https://app.powerbi.com/reportEmbed?reportId=1cdf96f6-e2f3-4bac-aa07-48a05386800c&autoAuth=true&ctid=edd67062-29c1-4a50-8fac-9813dfae8e4b

## Project Overview

This project is a complete end-to-end Power BI solution that transforms a massive, raw dataset of 2 million commercial flight records into an interactive dashboard for the Federal Aviation Administration (FAA) case study.

The primary goal was to analyse and identify patterns related to flight delays, cancellations, and on-time performance across major US airports in 2015.

This dashboard was built following best practices for data modeling star schema and visual design, culminating in a clear, actionable report for operational decision makers.

---

## Key Business Questions Answered

The final dashboard provides clear answers to critical operational questions:

* **Overall Performance:** What is the total volume of flights, and what are the overall On-Time, Delayed, and Cancelled rates?
* **Airport Traffic:** Which airport cities generate the highest volume of air traffic? 
* **Airline Reliability:** Which airlines are the most and least reliable in terms of percent delayed flights? 
* **Cancellation Reasons:** What is the composition of flight cancellations by type (e.g., Weather, Carrier, Security)? 
* **Cancellation Trends:** Are cancellations more common on specific days of the week? 

---

## Dashboard Highlights & Visuals

The report organizes data into three distinct sections (Total Flights, Delayed Flights, and Canceled Flights) and utilises several custom DAX measures to create the following key visuals:

### 1. KPIs
A set of three Card visuals displaying major metrics at the top of the report:
* **Total Flights:** Volume of all records 
* **Delayed Flights:** Volume of delayed flights.
* **Cancelled Flights:** Volume of cancelled flights.

### 2. Time-Series Trends
Line charts showing the monthly trend for Total, Delayed, and Cancelled Flights, providing essential time-based context.

### 3. Core Analytical Visuals
| Visual | Purpose & Data Fields | Key Insights Provided |
| :--- | :--- | :--- |
| **Bar Chart: Total Flights by City** | Breaks down flight volume using the `City` field from the Airport dimension table. | Identifies top-volume airports like Atlanta and Chicago. |
| **Bar Chart: % Delayed by Airline** | Measures the **`% Delayed`** custom DAX measure against the `Airline Name`. | Uncovers the least and most reliable carriers (e.g., JetBlue was least reliable in the sample). |
| **Donut Chart: % Cancellations by Type** | Uses the `Cancellation Description` to show composition of the **`Cancelled Flights`** measure. | Highlights the dominance of 'Weather' as a cancellation cause (57.3% in the sample). |
| **Column Chart: % Cancellations by Day of Week** | Uses the `Day of Week` column against the **`% Cancelled`** measure. | Reveals peak cancellation days (e.g., Mondays and Sundays). |
| **100% Stacked Bar Chart** | Shows the composition of **`Status`** (Cancelled, Delayed, On Time) across the total flight count). | Provides a high-level view of overall flight success/failure rate. |

---

## Technical Implementation

### 1. Data Sources & Preparation 

* **Data Sources:** Four local CSV files were loaded: `Flights` (Fact Table), `Airlines`, `Airports`, and `Cancellation Codes` (Dimension Tables)
* **Data Transformation:** Focused on isolating key columns (`Year`, `Month`, `Day of Week`, `Airline`, `Origin Airport`, `Departure Delay`, `Cancelled`, `Cancellation Reason`)
* **Custom Column:** Created a calculated column named **`Status`** (using a conditional column) to categorise each flight as `Cancelled`, `Delayed` (delay > 0), or `On Time`) 

### 2. Data Modeling (Star Schema)

* **Model Structure:** A classic star schema was implemented, connecting all three dimension tables

### 3. DAX Measures & Logic

A total of seven custom measures were created in the `Flights` table to support the analytical needs:

| Measure | Formula Description | Type |
| :--- | :--- | :--- |
| **`Total Flights`** | `COUNTROWS('Flights')` | Volume |
| **`Cancelled Flights`** | `CALCULATE([Total Flights], 'Flights'[Status] = "Cancelled")` | Volume |
| **`Delayed Flights`** | `CALCULATE([Total Flights], 'Flights'[Status] = "Delayed")` | Volume |
| **`On Time Flights`** | `CALCULATE([Total Flights], 'Flights'[Status] = "On Time")` | Volume |
| **`% Cancelled`** | `DIVIDE([Cancelled Flights], [Total Flights], 0)` | Rate |
| **`% Delayed`** | `DIVIDE([Delayed Flights], [Total Flights], 0)` | Rate |
| **`% On Time`** | `DIVIDE([On Time Flights], [Total Flights], 0)` | Rate |

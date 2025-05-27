# Bus Transit On-Time Performance (OTP) Power BI Dashboard

Public transit reliability is important and directly impacts rider trust in the system. Transit agencies hstruggle to adhere to schedules due to traffic, unpredictable demand patterns and operational inefficiencies. To address these challenges and strengthen transparency, this On-Time Performance (OTP) Dashboard is developed. This tool analyzes schedule adherence, delays by route/time, and historical trends using GPS trackers, passenger counters, and schedule data.

---

## Key Features

- **Three Interactive Pages:**
  1. **Overview & Monthly Analysis:** Global OTP trends by day of service, coverage type, and timeframes.
  2. **Route-Level Analysis:** Compare on-time performance across individual routes and identify top-performing and underperforming lines.
  3. **Geospatial Analysis:** Explore stop-level reliability using an interactive bubble map with filters for day, route, time of day, direction, and coverage type.

- **Business Questions Answered:**
  - What is the overall monthly on-time performance by day of service?
  - How does OTP vary across different route categories (e.g., Frequent, Coverage, Rapid)?
  - What are the trends for specific routes over the past 12 months?
  - How does OTP differ by month, year, and week?
  - What is the difference in OTP during peak vs. non-peak hours?

- **Dataset:**
The dataset comprises of GPS tracker data (vehicle locations and adherence metrics), APC records (boarding/alighting counts per stop), and schedule adherence logs (planned vs. actual departure/arrival times). This dataset was structured into two types of relational tables:<br>
(i) Fact tables that capture quantitative metrics like delay minutes and passenger volumes.<br>
(ii) Dimension tables that capture attributes like route types and stop location.

Initial data preprocessing was done including exploratory data analysis (EDA) to summarize record counts, identify null values, compute averages and assess data completeness. All data was uploaded into PostgreSQL and custom SQL queries were executed using common table expressions and relational joins across fact and dimension tables. 

---

## Dashboard

### Page 1 - Overview & Monthly Analysis  
<img src="/images/page_1.png"><br>

The first page of the dashboard provides an overview and monthly analysis of the rides. It includes two cards: one showing the overall average on-time performance and the other displaying the average departure delay time in minutes. We have added two slicers to allow filtering of the data - one by day type (Saturday, Sunday, or weekday) and the other by date range. The page also features three interactive visualizations:<br>

(i) a column chart displaying the on-time performance percentage by day type,<br>
(ii) a line chart illustrating the month-to-month on-time performance trend, and<br>
(iii) a stacked bar chart showing the monthly distribution of trips categorized as on time, late, or early.<br>

Selecting a specific month updates all cards and visualizations dynamically. The on-time performance has been calculated based on the criteria you provided. Trips are considered On Time if the DepartTimeVarianceSecs in FactSegmentAdherence csv is between -60 and 300, Late if the departure delay exceeds 300, and Early if the departure delay is less than -60. If DepartTimeVarianceSecs was NULL, that row was excluded from OTP calculations.

### Page 2 - Route-Level Analysis  
<img src="/images/page_2.png"><br>

The second page focuses on route level performance. Similar to the first page, here also there are two cards showing the overall average on-time performance and displaying the average departure delay time in minutes. Filtering on this page is done by selecting the coverage type. There are 4 visualizations on this page:

(i) There is a bar chart to show on-time performance by coverage type. <br>
(ii) There is another bar chart to show on-time performance by route, with routes sorted from highest to lowest on-time performance to easily identify the best and worst performing routes.<br>
(iii) There is also a line chart that shows the average departure delay time by route, sorted in descending order. The average delay is calculated by taking the mean of the DepartTimeVarianceSecs for each route. <br>
(iv) To provide deeper insights into route level delays,there is column chart added that highlights the top five stops with the highest departure delays. 

As with the previous page, all visualizations in this page are interactive, and selecting a particular route in any of the visualizations, will update the top 5 worst performing stops visualization to show the stops with the most departure delay time for that selected route. 

### Page 3 - Geospatial Analysis at Stop Level  
<img src="/images/page_3.png"><br>

The third and final page is the most important and shows the geospatial analysis. A bubble map is created using ArcGIS to display OTP across the Indianapolis area. The map shows OTP at the stop level based on the average value of DepartTimeVarianceSecs. 5 filters are provided for this and for all the stops that meet the selected criteria, their mean DepartTimeVarianceSecs value is calcualted and then the OTP status is categorized into early, on-time, late based on whether it is less than -60, between -60 to 300, or more than 300, respectively. On the map, stops classified as On-Time are marked in blue, Late in red, and Early in green.
The five filters available for refining the view are

(i) Day of service - Saturday, Sunday, Weekday<br>
(ii) Time of day - Morning (5am to 10am), Afternoon (10am to 4pm), Evening (4pm to 9pm), Night (9pm to 5am)<br>
(iii) Coverage type<br>
(iv) Direction<br>
(v) Route Name

---


### Filtering Examples

#### OTP for East Direction Routes on Weekday Nights  
<img src="/images/page_3_filter_1.png"><br>
Most trips arrive on time, with very few early arrivals and almost no late arrivals.

#### OTP for Red Line Coverage Type on Saturday Afternoons  
<img src="/images/page_3_filter_2.png"><br>
A significant number of trips arrive late during this time frame.

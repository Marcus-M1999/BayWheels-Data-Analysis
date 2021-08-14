# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so.

- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include:
  * Single Ride
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions:

  * What are the 5 most popular trips that you would call "commuter trips"?

  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data.

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to:
  * market offers differently to generate more revenue
  * remove offers that are not working
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc.

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced.

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch.

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties.

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback.
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown.

- What's the size of this dataset? (i.e., how many trips)
* Answer:There are 1947419 trips in this dataset. My Query can be found below:
  * SQL query:
  ```
  SELECT count(trip_id) 
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` 
  ```

- What is the earliest start date and time and latest end date and time for a trip?
* Answer:The earliest date is: 2013-08-29 09:08:00 UTC, and the latest date is: 2018-04-30 23:58:45 UTC.
  * SQL query:
  ```
  SELECT min(start_date), max(start_date) 
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` 
  ```

- How many bikes are there?
* Answer:There are 10445 bikes.
  * SQL query:
 ```
 WITH cte AS
(
SELECT DISTINCT(station_id), capacity
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
)
SELECT SUM(capacity) FROM cte
```





### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: What are the top 5 most popular start stations?
  * Answer: The top 5 most popular start stations are (in order):
  Central Ave at Fell St.
2nd St. at Townsend St
8th St. at Ringold St.
Howard St. at 8th St.
Laguna St at Hayes St

  * SQL query:
  ``` 
  SELECT i.name, COUNT(trip_id) AS trips
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` t
  JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` i
    ON t.start_station_id = i.station_id
  GROUP BY i.name
  ORDER BY trips DESC
  ```

- Question 2: Which hour is the most popular to travel in?
  * Answer: 8am is the most popular hour to travel in, with 240965 trips. The top 5 popular hours fall between 8-9am or 4-6 pm. 
  * SQL query: 
    ```
    SELECT EXTRACT(HOUR FROM start_date) AS Hour, COUNT(trip_id) AS trips
    FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` t
    GROUP BY Hour
    ORDER BY trips DESC
  ```

- Question 3: What is the most popular trip starting during the morning commute hours? (7am - 9 am)
  * Answer: The most popular morning trip is from 8:50am to 8:56am.
  * SQL query:
   ```
  SELECT EXTRACT(HOUR FROM start_date) AS Start_Hour, 
    EXTRACT(MINUTE FROM start_date) AS Start_Minute,
    EXTRACT(HOUR FROM end_date) AS End_Hour, 
    EXTRACT(MINUTE FROM end_date) AS End_Minute,
    COUNT(trip_id) AS trips
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` t
  WHERE EXTRACT(HOUR FROM start_date) > 7 AND EXTRACT(HOUR FROM start_date) < 9
  GROUP BY Start_Hour, Start_Minute, End_Hour, End_Minute
  ORDER BY trips DESC
```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

* What's the size of this dataset? (i.e., how many trips)
   ```
   bq query --use_legacy_sql=false 'SELECT count(trip_id) FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` '
  ---------+
  |   f0_   |
  +---------+
  | 1947419 |
  +---------+

   ```

* What is the earliest start time and latest end time for a trip?

```
bq query --use_legacy_sql=false 'SELECT min(start_date), max(start_date) FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` '

+---------------------+---------------------+
|         f0_         |         f1_         |
+---------------------+---------------------+
| 2013-08-29 09:08:00 | 2018-04-30 23:58:45 |
+---------------------+---------------------+

   ```

* How many bikes are there?
```
bq query --use_legacy_sql=false 'WITH cte AS 
(
SELECT DISTINCT(station_id), capacity
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
)
SELECT SUM(capacity) FROM cte 
 '
 '
+-------+
|  f0_  |
+-------+
| 10453 |
+-------+

   ```


2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

* How many trips are in the morning vs in the afternoon?

There are 341,551 trips in the afternoon commute hours (from 4-7pm) and 359,138 trips in the morning commute hours (from 6-9am).
```
SELECT  COUNT(trip_id) AS trips,
CASE
WHEN EXTRACT(HOUR FROM start_date) < 10 THEN 'Morning Commute'
WHEN EXTRACT(HOUR FROM start_date) > 10 THEN 'Afternoon Commute'
END AS Commute
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
WHERE (TIME(start_date) > TIME(7, 0, 0) 
    AND TIME(start_date) < TIME(9, 0, 0))
    OR (TIME(start_date) > TIME(16, 0, 0) AND 
    TIME(start_date) < TIME(19, 0, 0)) AND
    duration_sec < 900 AND 
    duration_sec > 300 AND
    (EXTRACT(DAYOFWEEK FROM start_date) NOT IN (1, 7)) AND
    start_station_id != end_station_id
GROUP BY Commute
  ```

### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: What defines a commuter trip?

- Question 2: Is it more beneficial to offer discounts during commute hours, or non commute hours? (find number of bikes available at each time)

- Question 3: What factors will influence a consumer's decision to rent a bike?

- Question 4: What impact do subscriptions have on commuter trips?

- Question 5: Is there a difference between the number of commuters in the morning and afternoon?

- Question 6: Which months are the least popular for bike sharing?
  
- Question 7: Which stations are least popular, where are they located?

- Question 8: Question 8: Who are getting bikes from the least rented stations?

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: What defines a commuter trip?

  * Answer: A commuter trip is one that takes place either during the morning commute hours (7am to 9am) or the afternoon commute hours (4pm to 7pm), as defined by the carpool lane on California highways. These trips will take place during the weekdays, when the majority of people work. Additionally, the trips will have to be longer than 5 minutes, and shorter than 19 minutes based on this [study](https://bikeleague.org/content/new-census-data-bike-commuting#:~:text=Commute%20time%3A%20The%20average%20bicycle,10%20and%2014%20minutes%20long.). Trips that start and end at the same station will be excluded from the table
  
  * SQL query: 
```
SELECT *
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
WHERE (TIME(start_date) > TIME(7, 0, 0)
AND TIME(start_date) < TIME(9, 0, 0))
OR (TIME(start_date) > TIME(16, 0, 0) AND
TIME(start_date) < TIME(19, 0, 0)) AND
duration_sec < 900 AND
duration_sec > 300 AND
(EXTRACT(DAYOFWEEK FROM start_date) NOT IN (1, 7)) AND
start_station_id != end_station_id
  ```

- Question 2: What factors will influence a consumer's decision to rent a bike?
  * Answer: The main factor seems to be geographical location. Since we don’t have price data, the desires of the customer is hard to pinpoint.

  * SQL query:
```
SELECT c. start_station_name, AVG(s.num_bikes_available) AS avg_bikes_available, COUNT(trip_id) AS trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status` s
LEFT JOIN `strange-radius-315021.section_99.Commuter_Table` c
ON c.start_station_id = s.station_id
GROUP BY c.start_station_name
ORDER BY trips DESC
  ```

- Question 3: What impact do subscriptions have on commuter trips?
  * Answer: Most commuters seem to be subscribers as proven by the table, with over 618,000 commuters being subscribers and 38,000 not being subscribed to the service over the entire 4 year period.
  * SQL query:
```
SELECT c.subscriber_type, COUNT(trip_id) AS trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status` s
JOIN `strange-radius-315021.section_99.Commuter_Table` c
ON c.start_station_id = s.station_id
GROUP BY c.subscriber_type
ORDER BY trips DESC
  ```

- Question 4:  Is there a difference between the number of commuters in the morning and afternoon?
  * Answer: The most popular morning and afternoon trips seem to mirror each other, however it appears that there are more people traveling in the afternoon commute hours compared to the morning ones.
  * SQL query(s): 
```    
SELECT m.start_station_name AS start_station,
    m.end_station_name AS end_station,
    COUNT(m.trip_id) AS trips
    FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status` s
    JOIN `strange-radius-315021.section_99.afternoon_commute` m
    ON m.start_station_id = s.station_id
    GROUP BY m.start_station_name, end_station
    ORDER BY trips DESC
  ```
&
``` 
SELECT m.start_station_name AS morning_start,
    m.end_station_name AS morning_end,
    COUNT(m.trip_id) AS morning_trips
 FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status` s
 JOIN `strange-radius-315021.section_99.morning_commute` m
 ON m.start_station_id = s.station_id
 GROUP BY m.start_station_name, morning_end
 ORDER BY morning_trips DESC

 ```

- Question 5: Which months are the least popular for bike sharing?
  * Answer: May, June, July are the three least popular months for bike sharing, with May and June having significantly less than July (~86,000 and 94,000 respectively).
  * SQL query:
``` 
SELECT
EXTRACT(MONTH FROM start_date) AS Month,
COUNT(trip_id) AS trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
GROUP BY Month
ORDER BY trips ASC
 ```
- Question 6: Which stations are least popular, where are they located?
  * Answer: The least popular active stations are: San Pedro St. at Hedding St. and Oak St. at First St.
  * SQL query:
``` 
SELECT i.name, COUNT(trip_id) AS trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` t
JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` i
ON t.start_station_id = i.station_id
GROUP BY i.name
ORDER BY trips ASC
 ```
- Question 7: Who are getting bikes from the least rented stations?
  * Answer: From stations like Oak St. and San Pedro St. the customers are almost entirely subscribers. Other stations like Berry St. and King St. have no subscriptions.
  * SQL query:
```
SELECT i.name, SUM(
CASE
WHEN t.c_subscription_type = 'Subscriber' THEN 1
ELSE 0
END) AS Num_subs
, COUNT(trip_id) AS trips
FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` t
JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` i
ON t.start_station_id = i.station_id
GROUP BY i.name
ORDER BY trips ASC
 ```

---


## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook

At the end of this document is an example Jupyter Notebook you can take a look at and run.

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE:
- Queries that return over 16K rows will not run this way,
- Run groupbys etc in the bq web interface and save that as a table in BQ.
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"?

  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file

[Example Notebook](example.ipynb)


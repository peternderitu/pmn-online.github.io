# Data Portfolio : Excel to SQL to PowerBI

![headerimage](/assets/images/Dataportfolioheader.png)

# Table of Contents

# Objective

## What is the key pain point?
The head of marketing wants to identify the top youtubers of 2024 in the UK to decide who would be the best youtuber to run a marketing campaign throught the rest of the year.
## What is the ideal solution?
To create a dashboard that provides insights into the top Uk youtubers that include:
- subscriber count
- total views
- total videos
- engagement metrics
  
This will help the marketing team make informed decisions on which youtuber to collaborate with for their marketing campaign.

## User story
As the Head of Marketing, I want to use a dashboard that analyses YouTube channel data in the UK .
This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views.
With this information, I can make more informed decisions about which Youtubers are right to collaborate with, and therefore maximize how effective each marketing campaign is.

# Data Source

## What is the data needed to meet the objective?
- channel name
- total views
- total videos
- subscriber count
  
## Data Source
Kaggle is our data source as an Excel extract. [Found here](/assets/datasets/youtube_data_united-kingdom.csv)

# Stages
- Design
- Development
- Testing
- Analysis

# Design

## Dashboard components required
- What should the dashboard contain based on the requirements provided?
  
To understand what it should contain, we need to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?
For now, these are some of the questions we need to answer, this may change as we progress down our analysis.

## What should the dashboard look like?
1. Table
2. Treemap
3. Horizontal bar chart
4. Scorecards

# Tools

| Tool | Purpose |
|-------|---------|
| Excel | Data exploration |
| SQL | Data cleaning, analysis, validation |
| Power BI | Data visualization tool via interactive dashboards |
| Github | Project documentation host and version control |

# Development
- What is the general solution from start to finish?

1. Get the data
2. Explore data through Excel
3. Load the data into a SQL server
4. Clean the data with SQL
5. Validating/Testing the data with SQL
6. Visualize the data with Power BI
7. Generate the findings based on insights
8. Write documentation + commentary
9. Publish data and findings on github pages

## Data Exploration notes
- What are the initial thoughts after exploring the data?

1. The 4 columns that we need are present so we don't need further information from the client
2. The first column has the channel name and id separated by the '@' symbol. We need to extract the column name from this
3. The language of the column headers are different we may need to change the nomenclature
4. There are unnecessary columns that we may need to remove from the dataset during cleaning

# Data Cleaning
- What do we expect clean data to look like?(What it should contain and what constraints apply to it)

The aim is to refine our data for analysis.
The cleaned data should meet the following criteria and constraints:\
1. Only relevant data columns should be retained
2. All data types should be relevant to the contents of each column
3. No column should contain null values,should be complete records

Below are the constraints:

| Type | Description |
| ---- | --------- |
| Column | 4 in number |
| Rows | 100 rows in number |

The data schema is as follows

| Column name | Data type | Nullable |
|-------------|-----------|----------|
| channel_name | VARCHAR | NO |
| total_subscribers | INTEGER | NO |
| total_views | INTEGER | NO |
| total_views | INTEGER | NO |
| total_videos | INTEGER | NO |

- What steps are to be taken to clean the data to desired format?
  
1. Remove unnecessary columns from the dataset
2. Change column names to appropriate names
3. Extract channel name from the NOMBRE column

## Transform the data

```sql
SELECT CAST(SUBSTRING(NOMBRE,1,CHARINDEX('@',NOMBRE)-1) AS VARCHAR(100)) AS channel_name,
	total_subscribers,
	total_views,
	total_videos
	FROM youtube_data_from_python
```
## Create the SQL view

```sql
/*
1. Remove unnecessary columns from the dataset
2. Change column names to appropriate names
3. Extract channel name from the NOMBRE column
*/
CREATE VIEW view_uk_youtubers_2024 AS
	SELECT CAST(SUBSTRING(NOMBRE,1,CHARINDEX('@',NOMBRE)-1) AS VARCHAR(100)) AS channel_name,
	total_subscribers,
	total_views,
	total_videos
	FROM youtube_data_from_python
```
# Testing
Data quality checks to be performed

## Row count check

```sql
/*

1. Check if there are 100 channels(row count) -- Passed!!!
*/ 

-- 1. Row Count

SELECT COUNT(*) AS no_of_rows
FROM view_uk_youtubers_2024
```
![row_count](/assets/images/rowcountcheck.PNG)

## Column count check

```sql
/*

2. Check if the number of columns are 4 (channel count) -- Passed!!!

*/ 
SELECT COUNT(*) AS column_count
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'view_uk_youtubers_2024'
```
![column_count](/assets/images/columncount check.PNG)

## Data type check

```sql
/*

3. Check if the data types are string for column_name and the rest are integer columns(data type check) -- Passed!!!

*/
-- 3. Data type check - 
		-- channel_name - varchar
		-- total_subscribers - integer
		-- total_views - integer
		-- total_videos - integer

SELECT COLUMN_NAME,DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'view_uk_youtubers_2024'
```
![data type](/assets/images/datatypecheck.PNG)

## Duplicate Count Check

```sql
/*

4. Check if all the channel_name data is unique(unique/distinct check per record) -- Passed!!!

*/ 
-- 4. Unique record check

SELECT channel_name, COUNT(*) AS duplicate_count
FROM view_uk_youtubers_2024
GROUP BY channel_name
HAVING COUNT(*) > 1

```
![duplicates](/assets/images/duplicatechecks.PNG)

# Visualizations

## Results
- What does the dashboard look like?
  
![power bi](/assets/images/Animation.gif)

# DAX Measures

1. Total Subscribers(M)

```dax
Total Subscribers(M) =
VAR million = 1000000
VAR sumofsubs = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalsubscribers = DIVIDE(sumofsubs,million,BLANK())
RETURN totalsubscribers
```
2. Total Views(B)

```dax
Total Views(B) =
VAR billions = 1000000000
VAR sumofviews = SUM(view_uk_youtubers_2024[total_views])
VAR totalviews = DIVIDE(sumofviews,billions,BLANK())
RETURN totalviews
```
3. Total Videos

```dax
Total Videos =
VAR totalvideos = SUM(view_uk_youtubers_2024[total_videos])
RETURN totalvideos
```
4. Average Views per Subscriber

```dax
Avg views per subscriber =
VAR sumofviews = SUM(view_uk_youtubers_2024[total_views])
VAR sumofsubs = SUM(view_uk_youtubers_2024[total_subscribers])
VAR avgviewspersub = DIVIDE(sumofviews,sumofsubs,BLANK())
RETURN avgviewspersub
```
5. Average Views per Video

```dax
Avg Views per Video(M) =
VAR sumofviews = SUM(view_uk_youtubers_2024[total_views])
VAR sumofvideos = SUM(view_uk_youtubers_2024[total_videos])
VAR avgviewpervideo = DIVIDE(sumofviews,sumofvideos,BLANK())
VAR finalavgviewpervideo = DIVIDE(avgviewpervideo,1000000,BLANK())
RETURN finalavgviewpervideo
```
6. Subscriber Engagement Rate

```dax
Subs engangement rate =
VAR sumofsubs = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumofvideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subengagementrate = DIVIDE(sumofsubs,sumofvideos,BLANK())
RETURN subengagementrate
```

# Analysis

# Findings
- What are our findings?

The key questions we want to answer are as below
1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

1. Who are the top 10 YouTubers with the most subscribers?

| Channel_name | Total Subscribers(M) |
| -------------|----------------------|
| NoCopyrightSounds | 33.6 |
| DanTDM | 28.6 |
| Dan Rhodes | 26.5 |
| Miss Katy | 24.5 |
| Mister Max | 24.4 |
| KSI | 24.1 |
| Jelly | 23.5 |
| Dua Lipa | 23.3 |
| Sidemen | 21.0 |
| Ali-A | 18.9 |

2. Which 3 channels have uploaded the most videos(Excluding news organizations/corporations)?

| Channel_name | Total Videos |
| -------------|--------------|
| GRM Daily | 14696 |
| YOGCAST Lewis & Simon | 6435 |
| Jelly | 6331 |

3. Which 3 channels have the most views?

| Channel_name | Total Views |
| -------------|-------------|
| DanTDM | 19.78 |
| Dan Rhodes | 18.56 |
| Mister Max | 15.97 |

4. Which 3 channels have the highest average views per video?

| Channel_name | Average views per video(M) |
| -------------|-------------------------|
| Mark Ronson | 322.79 |
| Jessie J | 59.77 |
| Dua Lipa | 57.62 |

5. Which 3 channels have the highest views per subscriber ratio?

| Channel_name | Average views per subscriber |
| -------------|-------------------------|
| GRM Daily | 1185.79 |
| Nickelodeon UK | 1061.04 |
| DisneyJunior UK | 1031.97 |

6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Channel_name | Subscriber engagement rate |
| -------------|-------------------------|
| Mark Ronson | 343000 |
| Jessie J | 110416.67 |
| Dua Lipa | 104954.95 |

# To Note
We'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client, which are the Youtube channels with the most:-
- subscribers
- total views
- videos uploaded

# Validation

1. Youtubers with the most subscribers

- Calculation breakdown
  
Campaign idea = product placement

a. NoCopyrightSounds
- Average views per video = 6.92 million
- Product cost = $5
- Potential units sold per video = 6.92 million x 2% conversion rate = 138,400 units sold
- Potential revenue per video = 138,400 x $5 = $692,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $692,000 - $50,000 = $642,000
  
b. DanTDM
- Average views per video = 5.34 million
- Product cost = $5
- Potential units sold per video = 5.34 million x 2% conversion rate = 106,800 units sold
- Potential revenue per video = 106,800 x $5 = $534,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $534,000 - $50,000 = $484,000
  
c. Dan Rhodes
- Average views per video = 11.15 million
- Product cost = $5
- Potential units sold per video = 11.15 million x 2% conversion rate = 223,000 units sold
- Potential revenue per video = 223,000 x $5 = $1,115,000
- Campaign cost (one-time fee) = $50,000
- Net profit = $1,115,000 - $50,000 = $1,065,000
  
Best option from category: Dan Rhodes

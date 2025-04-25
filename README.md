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


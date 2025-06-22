# rainfall-analysis
AWS + SNOWFLAKES + POWER BI 

# 🌧️ Rainfall, Temperature, Humidity & Yield Analysis Dashboard

This project provides an end-to-end data analytics solution using **Power BI**, **Snowflake**, and **AWS S3**. The objective is to analyze seasonal and regional agricultural patterns based on rainfall, temperature, humidity, and crop yields.

## 🔗 Dashboard Link
👉 [Live Power BI Dashboard](https://app.powerbi.com/reportEmbed?reportId=a9f5a326-8a6a-4e0e-aa7c-d90679c3c8fe&autoAuth=true&ctid=e1d99821-ef38-4f48-836f-7a7ca113dab7)

## 🧱 Architecture
AWS S3 → Snowflake (Stage & Integration) → Power BI (Live Connection)

## 📁 Repository Contents

| File Name                                       | Description                                  |
|------------------------------------------------|----------------------------------------------|
| `data_season.csv`                               | Raw dataset                                  |
| `season and rainfall and humidity analysis.pbix`| Power BI dashboard file                      |
| `document of snowflakes + aws integration.docx` | Integration guide with code                  |
| `README.md`                                     | Project overview (this file)                 |

---

## 🛠️ Snowflake Setup

### 1. Create Database, Schema & Table
```sql
CREATE DATABASE POWERBI;
CREATE SCHEMA PBI_Data;

CREATE TABLE PBI_Dataset (
    Year INT,
    Location STRING,
    Area INT,
    Rainfall FLOAT,
    Temprature FLOAT,
    Soil_type STRING,
    Irrigation STRING,
    yeilds INT,
    Humidity FLOAT,
    Crops STRING,
    price INT,
    Season STRING
);
```

### 2. Create Stage with AWS S3 Integration
```sql
CREATE STAGE POWERBI.PBI_Data.PBI_stage
URL = 's3://powerbi.anuj489project/'
STORAGE_INTEGRATION = PBI_Integration;

```

### 3. Load Data from S3 into Snowflake
 ```sql
 COPY INTO PBI_Dataset
FROM @PBI_stage
FILE_FORMAT = (TYPE = 'CSV' FIELD_DELIMITER = ',' SKIP_HEADER = 1)
ON_ERROR = 'CONTINUE';

LIST @PBI_stage;
```
###🧪 Transformations & Business Logic
 ```sql
CREATE TABLE agriculture AS
SELECT * FROM PBI_Dataset;
###Data Adjustments
UPDATE agriculture SET Rainfall = 1.1 * Rainfall;
UPDATE agriculture SET Area = 0.9 * Area;
 

ALTER TABLE agriculture ADD year_group STRING;
##Add Year Group
UPDATE agriculture SET year_group = 'Y1' WHERE Year BETWEEN 2004 AND 2009;
UPDATE agriculture SET year_group = 'Y2' WHERE Year BETWEEN 2010 AND 2015;
UPDATE agriculture SET year_group = 'Y3' WHERE Year BETWEEN 2016 AND 2019;

##Add Rainfall Group
ALTER TABLE agriculture ADD Raifall_Groups STRING;

UPDATE agriculture SET Raifall_Groups = 'low' WHERE Rainfall >= 255 AND Rainfall < 1200;
UPDATE agriculture SET Raifall_Groups = 'medium' WHERE Rainfall >= 1200 AND Rainfall < 2800;
UPDATE agriculture SET Raifall_Groups = 'high' WHERE Rainfall >= 2800;
```
📊 Power BI Dashboard Highlights

Rainfall analysis by year, season, and location

Temperature analysis by season, crop, and region

Humidity trends and crop-wise analysis

Yield & Area trends over time

Filters for:

Year groups (Y1, Y2, Y3)

Rainfall level (Low, Medium, High)

Crops, Seasons, and Locations

✅ Key Highlights
💾 Cloud Storage: Data stored in AWS S3 bucket

❄️ Snowflake: Used external stage, storage integration, and SQL transformations

📊 Power BI: Developed interactive visuals and filters for analysis

🧾 Automation Ready: Code-driven transformation pipeline

🌐 GitHub Repo: Complete project and documentation uploaded

📌 Author
Anuj Agarwal
Snowflake | AWS | Power BI | Data Analytics Enthusiast
📧 anuj489@gmail.com

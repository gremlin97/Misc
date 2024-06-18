ETL - Credit Score Data

-Features:
    Age
    Number of Past Due 1-2 months
    Number of Past Due 1-2 months but not worse
    Number of Past Due 3 months
    Income
    Number of mortgage and real estate loan lines (Home loans)
    Number of open credit (credit cards) and loan (home/car) lines
    Number of Dependents
    Revolving Utilization of Unsecured Lines:
    Total balance (due) on credit cards + other lines / (Sum(credit Limits))

-Target:
    If user is delinquent - Defaults after 90 days.

- Extraction:

    Load data from CSV in the data lake S3 bucket using AWS Glue Crawler to infer the schema of the database using a crawling process and extract the data to a Glue database in S3.

- Transformation:

    Use PySpark Glue ETL jobs to remove duplicates, drop null values, and remove outliers. Specifically, remove records of individuals below 18 years old and those with revolving ratios > 1. Standardize the dataset with mean 0 and standard deviation 10.

- Load:

    Save the dataset to the target DynamoDB (warehouse) and S3 bucket (data lake).
    
- Insights from Athena and Quicksight:

    Mean debt ratio, standard deviation, and percentile scores.

    Proportion of people who defaulted.

    Revolving debt trend: younger people are more in debt; as age increases, debt decreases.

    Monthly income bell curve from ages 18 to 90.
    
    Dataset imbalance: more individuals between 40-50 years old and more individuals with debt exceeding 90% compared to others.
    

**Situation:**
I was tasked with managing ETL processes for credit score data, aiming to extract, transform, and load data from a variety of sources into a structured format suitable for analysis.

**Task:**
The goal was to process credit score data including features such as age, income, past due amounts, and loan details. The process needed to ensure data integrity, remove outliers, standardize values, and ultimately provide insights into debt trends and default rates.

**Actions:**

**Extraction:**
- Utilized AWS Glue Crawler to automatically infer the schema of CSV data stored in an S3 bucket.
- Loaded and extracted the data into a Glue database in S3 for further processing.

**Transformation:**
- Implemented PySpark Glue ETL jobs to:
  - Remove duplicate records and drop null values to ensure data cleanliness.
  - Filter out records of individuals under 18 years old and those with revolving ratios exceeding 1.
  - Standardized the dataset to have a mean of 0 and a standard deviation of 10 to facilitate comparative analysis.

**Loading:**
- Saved the processed dataset into DynamoDB for warehousing and also stored it back into the S3 bucket as part of a comprehensive data lake strategy.

**Result:**

**Insights and Reporting:**
- Leveraged Athena and QuickSight for data analysis and visualization, generating actionable insights such as:
  - Mean debt ratios, standard deviations, and percentile scores across different demographics.
  - Identification of the proportion of individuals who defaulted on payments.
  - Analysis of revolving debt trends, highlighting that younger individuals tend to have higher debt levels which decrease with age.
  - Visualization of a monthly income bell curve spanning ages 18 to 90.
- Noted the dataset's demographic imbalance, particularly with more individuals aged 40-50 and a higher prevalence of individuals with debt exceeding 90%.

This process enabled comprehensive data management and insightful reporting, contributing to informed decision-making regarding credit risk assessment and financial trends.

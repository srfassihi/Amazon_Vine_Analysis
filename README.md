# Amazon_Vine_Analysis

## Objective

Analyze Amazon reviews conducted by members of the paid Amazon Vine Program, to see if their reviews have inherent positivity bias towards the product. Calculate stats using PySpark library in Google Colab Notebook and share results.

### Software
- Google Colab Notebook
- PySpark v3.2.1
- PostGRes Driver (42.2.16)
- AWS RDS Database (PostGres v12.9)
- PostGres Admin v6.1

### Dataset
[Amazon Reviews](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt)

### Product
[Video Games](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Video_Games_v1_00.tsv.gz)

## Results

### Deliverable 1: ETL
ETL was performed on the Amazon Review Dataset and data was imported successfully into the AWS using the following steps:

1. Load Data into Spark dataframe

![Load Data](https://github.com/srfassihi/Amazon_Vine_Analysis/blob/bd9ccbe27f5ce97967b8e2aa37f2d332b5160f71/resources/Screen%20Shot%202022-02-20%20at%205.31.31%20PM.png)
2. Drop null rows from dataframe

![Drop Nulls](https://github.com/srfassihi/Amazon_Vine_Analysis/blob/bd9ccbe27f5ce97967b8e2aa37f2d332b5160f71/resources/Screen%20Shot%202022-02-20%20at%205.31.47%20PM.png)
3. Create Dataframes to match target tables

![Create DataFrames-Products](https://github.com/srfassihi/Amazon_Vine_Analysis/blob/bd9ccbe27f5ce97967b8e2aa37f2d332b5160f71/resources/Screen%20Shot%202022-02-20%20at%205.32.09%20PM.png)
4. Connect to AWS using PostGRes Driver and write dataframe results to PostGRes tables

![Connect and Write Results](https://github.com/srfassihi/Amazon_Vine_Analysis/blob/bd9ccbe27f5ce97967b8e2aa37f2d332b5160f71/resources/Screen%20Shot%202022-02-20%20at%205.32.39%20PM.png)

### Deliverable 2: Determine Bias of Vine Reviews
Vine and Non Vine Reviews were analyzed on the Amazon Review Dataset in both the created SQL 'Vine_Table' and in a Colab Notebook. Using the 'cleaned' dataframe from the previous deliverable the following steps were followed:

1. Create Vine Table DataFrame in Colab
2. Filter using the following criteria (Total Votes >=20, (helpful_votes/total_votes)>=0.5)
3. Calculate the Total Reviews, 5-Star Count and Percent 5-Star for Vine and Non-Vine reviews
4. Compare Totals and Percentage of Totals for Paid vs. Unpaid Reviews

**Analysis Results**

![Analysis DataFrame](https://github.com/srfassihi/Amazon_Vine_Analysis/blob/bd9ccbe27f5ce97967b8e2aa37f2d332b5160f71/resources/Screen%20Shot%202022-02-20%20at%205.41.55%20PM.png)
 

## Summary

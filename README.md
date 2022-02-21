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

SQL:

SELECT VINE,
			COUNT(*) AS CNT_TOTAL
		FROM VINE_TABLE
		WHERE CAST(HELPFUL_VOTES AS FLOAT) / CAST(TOTAL_VOTES AS FLOAT) >= 0.5
			AND TOTAL_VOTES >= 20
		GROUP BY VINE

| VINE | COUNT |
|:---|:---|
| N | 40471|
| Y | 94 | 

SELECT VINE,
			COUNT(*) AS CNT_FIVE_STAR
		FROM VINE_TABLE
		WHERE CAST(HELPFUL_VOTES AS FLOAT) / CAST(TOTAL_VOTES AS FLOAT) >= 0.5
			AND TOTAL_VOTES >= 20
			AND STAR_RATING = 5
		GROUP BY VINE

| VINE | FIVE_STAR_COUNT |
|:---|:---|
| N | 15663|
| Y | 48 | 

## Summary

- Reviews produced by the Vine showed a slightly higher tendancy to positive reviews than non-paid reviews indicating a bias towards giving higher ratings.
 - For the filtered sample, there was a 51% of 5-Star reviews given by Vine versus a 39% of 5-Star reviews by others
  - 48 / 94 ratings by Vine were 5-Stars
  - 15.6k / 40.4k ratings by others were 5-Stars
- Total sample of Vine Reviews were much smaller than the population (Less than 0.2%), but out of this sample there was a stronger inclination to give higher ratings. 

### Recommendations
- Further analysis can be done using other datasets to indicate which products have stronger bias than others. 
- Specific Reviews can be audited by a third party (i.e. other consumers) to see if they are trustworthy.
- 'Post mortem' or '6-month later' review process can be done to further evaluate products AFTER the initial impression is done to get a more concrete analysis of the product.

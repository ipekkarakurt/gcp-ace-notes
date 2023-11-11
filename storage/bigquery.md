## BigQuery

**enterprise data warehouse** for petabyte-scale relational structured data.
- optimized for **large-scale, ad-hoc SQL-based analysis and reporting**
- best for: gaining organizational insights, large-scale storage & analysis, **online analytical processing (OLAP)**

- ingests large amounts of data in a batch / by streaming data directly into BQ.
- perform queries: sum, avg, count, grouping, queries for creating machine learning models
- BigQuery Data Service: automates data movement into BigQuery on a scheduled, managed basis. From Google products (Cloud Storage), AWS S3, Amazon Redshift etc

bq show --format=prettyjson [PROJECT]:[DATASET] (displays information about a data set)\
bq ls (lists datasets)

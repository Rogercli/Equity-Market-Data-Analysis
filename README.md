# Equity-Market-Data-Analysis
## Overview
  In this project, the goal was to develop an ETL pipeline to ingest, tranform, analyze, and store data produced by NYSE and NASDAQ stock exchange. 
  The final solution utilizes:
  1) Azure Blob storage
  2) DataBricks elastic cluster
  3) Pyspark Dataframes and SQL
  4) Parquet files.
  
**Step 1**: Database Table Design
* Implement schema to host the trade and quote data.
* Since trade and quote data is added on a daily basis with high volume, the design needs
to ensure the data growth over time doesn’t impact query performance. 

**Step 2**: Data Ingestion
* Parses source data (JSON and CSV) and transforms them into a common format
* Exchanges may submit data in multiple batches, therefore the platform pre-processes the
data as soon as they arrive.
* Drops any records that do not follow the schema

**Step 3**: End of Day (EOD) Batch Load
* Loads all the progressively processed data for current day into daily tables for trade and
quote.
* Since exchanges may submit the records which are intended to correct errors in previous ones
with updated information. The process discards the old record and loads only the
latest one to the table.


**Step 4**: Analytical ETL Job
* Calculate latest trade price before the quote.
* Calculate latest 30 min moving average trade price before the quote.
* Calculate bid and ask price movement (difference) from the previous day's last trade price.

**Step 5**. Pipeline Orchestration:
* Uses Databricks scheduler and chaining of notebooks to orechestrate workflow 


## Directory Details
- **Data**: Source data from exchanges, which need to first be stored into Azure Blob Containers
- **Local_env_notebooks**: Local testing of the entire pipeline
- **Prod_env_notebooks**: Notebooks for ETL stages in Azure Blob Storage and DataBricks Cluster Environment


## Testing Locally
- **Dependencies**: Spark and source data available on local environment
- **Run**: Run local notebooks with the correct configurations and paths

## Production
- **Dependencies**: Microsoft Azure Account, with Databricks cluster and Blob container resources set up. Spark configurations updated with Microsoft Account and Blob information. 
- **Set-up**: Using either Azcopy or Azure Storage-Explorer, populate the Blob container with source data. In addition, in order to make our data accessible by DataBricks, we need to mount our container onto the DataBricks cluster.
- **Run**: Run the ETL-workflow_prod notebook which chains together each stage of the ETL Pipeline

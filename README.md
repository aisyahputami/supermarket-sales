# Introduction
Our analytics team needs data related to our Supermarket sales for our monthly reporting project. Data is in CSV format which can be accessed here, [Supermarket Sales](https://www.kaggle.com/datasets/aungpyaeap/supermarket-sales). 

Business team also needs me to generate any insights we can get from those data. I’ll collaborate with a data visualization expert that will create a dashboard for the business team. I need to provide summary tables for data visualization experts, such as monthly revenue, etc.

The tools used to build the data pipeline are as follows:

1. **Kafka** as a streaming platform
2. **NiFi** as an ETL (Extract, Transform, Load) tool
3. **Informatica** as a data integration tool
4. **Google Cloud Storage** as a datalake
5. **Google BigQuery** as a data warehouse


Below is the pipeline I've built to integrate data from a CSV data source into BigQuery as a data warehouse:
![Pipeline](https://github.com/aisyahputami/supermarket-sales/blob/main/weekly_assignment-2-pipeline.png)

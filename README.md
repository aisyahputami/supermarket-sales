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

# Data Ingestion
Data ingestion is the process of retrieving, collecting, and inputting data from various sources into a specific system or data storage. It is the initial step in the data lifecycle where the required data for analysis, processing, or storage is gathered from diverse sources and fed into a centralized data environment.

The data ingestion process involves several steps, including:

**Extraction**: Data is retrieved from its source, which could be databases, flat files, streaming systems, APIs, or other data sources. The extraction process may involve direct reading from the data source or through specified interfaces (e.g., using APIs to fetch data from third-party applications).<br>
**Transformation**: Once the data is retrieved, it often needs to be altered or modified to fit the format or structure required for a particular purpose. This may involve operations such as data cleansing, format transformation, merging data from multiple sources, or applying specific business rules.<br>
**Validation**: The extracted and transformed data is then verified to ensure its quality. This may include error checking, data validation against business rules or expected formats, or duplicate checking.<br>
**Loading**: After the data is extracted, modified, and validated, it is loaded into the intended system or data storage. This could be data storage such as databases, data warehouses, cloud-based storage systems, or streaming systems for real-time analytics.

Data ingestion is crucial as it is a critical step in preparing data for further analysis, processing, or storage. This process ensures that the required data is available in the appropriate format and of good quality for use in various business applications and analyses.

For this case, a combination of Apache NiFi and Apache Kafka is used to integrate data from a CSV source to Informatica as a data sink.

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

Before we proceed, given that the source is a CSV file, we'll first convert it into JSON format using Python to simplify the process. Below is the conversion process.
![Convert CSV](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/convert-csv-to-json.png)

# Data Ingestion
Data ingestion is the process of retrieving, collecting, and inputting data from various sources into a specific system or data storage. It is the initial step in the data lifecycle where the required data for analysis, processing, or storage is gathered from diverse sources and fed into a centralized data environment.

The data ingestion process involves several steps, including:

**Extraction**: Data is retrieved from its source, which could be databases, flat files, streaming systems, APIs, or other data sources. The extraction process may involve direct reading from the data source or through specified interfaces (e.g., using APIs to fetch data from third-party applications).<br>
**Transformation**: Once the data is retrieved, it often needs to be altered or modified to fit the format or structure required for a particular purpose. This may involve operations such as data cleansing, format transformation, merging data from multiple sources, or applying specific business rules.<br>
**Validation**: The extracted and transformed data is then verified to ensure its quality. This may include error checking, data validation against business rules or expected formats, or duplicate checking.<br>
**Loading**: After the data is extracted, modified, and validated, it is loaded into the intended system or data storage. This could be data storage such as databases, data warehouses, cloud-based storage systems, or streaming systems for real-time analytics.

Data ingestion is crucial as it is a critical step in preparing data for further analysis, processing, or storage. This process ensures that the required data is available in the appropriate format and of good quality for use in various business applications and analyses.

For this case, a combination of Apache NiFi and Apache Kafka is used to integrate data from a CSV source to Informatica as a data sink.

## Setup Apache Kafka
Here's how to check the Java version. Make sure we have Java installed.
![Java version](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/java-version.png)

Then install Kafka and check the list of files in the Kafka directory.
![Kafka File Directory](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/kafka-file-directory.png)

Open a new terminal, and start Zookeeper.
![Start Zookeeper](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/start-zookeeper.png)

Open another new terminal, and start Kafka.
![Start Kafka](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/start-kafka.png)

Return to the first terminal and create a topic named 'supermarket'.
![Supermarket Topic](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/create-supermarket-topic.png)
By running this command, we will create a new topic named 'supermarket' with 3 partitions and a replication factor of 1 on the Kafka cluster running on localhost and port 9092.
![Describe Supermarket Topic](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/describe-topic-supermarket.png)

To ensure that the 'supermarket' topic has been created, we can check the list of existing topics.
![List Existing Topic](https://github.com/aisyahputami/supermarket-sales/blob/main/kafka-setup/list-existing-topic.png)

## Setup Apache NiFi
Open a new terminal and start NiFi. Ensure that we have installed NiFi beforehand.
![Start Nifi](https://github.com/aisyahputami/supermarket-sales/blob/main/nifi-setup/start-nifi.png)

Open https://localhost:8443 in your browser and login to NiFi using the username and password found in the nifi > logs folder.
![Login NiFi](https://github.com/aisyahputami/supermarket-sales/blob/main/nifi-setup/login-nifi.png)

The home view of NiFi looks like this
![NiFi Home](https://github.com/aisyahputami/supermarket-sales/blob/main/nifi-setup/home-nifi.png)



## Data Ingestion and Streaming
### ConsumerKafka
Create ConsumerKafka processor. This processor is used in Apache NiFi to retrieve data from Kafka topics.
![NiFi ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/consumer-kafka.png)

Next, configure the settings for the ConsumerKafka processor in the "Relationships" and "Properties" sections as follows
![Config Relationship ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-consumer-kafka-relationship.png)

![Config ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-consumer-kafka-properties.png)

### PutGCSObject
Then we will create a PutGCSObject processor. However, before that, let's prepare the Google Cloud Storage. First, create a bucket in GCS and name it 'weekly_assignment_2'.
![Create Bucket](https://github.com/aisyahputami/supermarket-sales/blob/main/gcs/create-bucket.png)

The bucket is already created and ready to use.
![Bucket Ready](https://github.com/aisyahputami/supermarket-sales/blob/main/gcs/bucket-ready.png)

After that, we also need to create a folder in the 'weekly_assignment_2' bucket. Name the folder 'data-lake'.
![Create Folder](https://github.com/aisyahputami/supermarket-sales/blob/main/gcs/create-data-lake.png)

The folder is ready to use.
![Folder Ready](https://github.com/aisyahputami/supermarket-sales/blob/main/gcs/data-lake-ready.png)

Next, we can create a PutGCSObject processor. This processor is used to upload data or files from FlowFiles to Google Cloud Storage (GCS).

![NiFi GCS](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/put-gcs.png)

And then, configure the settings for the PutGCSObject processor in the "Connection" and "Properties" sections as follows

![GCS Connection from](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-connection-from-gcs1.png)

![GCS Connection from2](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-connection-from-gcs2.png)

![GCS Connection to](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-gcs-connection-from.png)

![GCS Relationships](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-gcs-properties.png)

We also need to configure the NiFi Flow Configuration in the Controller Service section as follows
![Controller Service-1](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-gcs-credentials-controller.png)

![Controller Service-2](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-gcs-credentials-controller-2.png)

![Controller Service-3](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-gcs-enable-controller.png)

### LogAttribute
Create LogAttribute processor. LogAttribute processor serves as a valuable tool for monitoring, auditing, and troubleshooting data flows in Apache NiFi by providing visibility into the attributes of FlowFiles as they move through the system.

![NiFi LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/logattribute-processor.png)

Next, configure the settings for the ConsumerKafka processor in the "Connection" and "Relationships" sections as follows

![NiFi Connection LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/put-gcs-to-logattribute.png)

![NiFi Relationships LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-logattribute-relationship.png)


### ConvertRecord
Create ConvertRecord processor. This processor is used to transform the format or structure of incoming data records into a different format or structure. In this case, we can utilize it to convert data from JSON format to CSV format.

![NiFi ConvertRecord](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-convert-record.png)

And then, configure the settings for the ConvertRecord processor in the "Connection" and "Properties" sections as follows

![Connection to ConvertRecord](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/put-gcs-to-convertrecord.png)

![Connection from ConvertRecord 1](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/convertrecord-to-bq.png)

![Connection from ConvertRecord 2](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/convertrecord-to-logattribute.png)

![Properties ConvertRecord](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/convertrecord-to-logattribute.png)

We also need to configure the NiFi Flow Configuration in the Controller Service section as follows
![Convertrecord-Enable](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/enable-controller-service-convertrecord.png)

### PutBigQuery

Before creating the PutBigQuery processor, we need to prepare the dataset (named 'supermarket') and table (named 'sales') in BigQuery.

![Create Dataset](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-dataset-in-bq.png)

![Create Table 1](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-table-1.png)

![Create Table 2](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-table-2.png)

![Create Table 3](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-table-3.png)

Create PutBigQuery processor. This processor is use to ingest data from Apache NiFi into Google BigQuery.
![NiFi PutBigQuery](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/create-put-big-query.png)

And then, configure the settings for the PutBigQuery processor in the "Connection" and "Properties" sections as follows
![Connection to PutBigQuery](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-from-put-bq-to.png)

![Connection from PutBigQuery]()

![Properties PutBigQuery](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/config-put-bigquery-properties.png)

We also need to configure the NiFi Flow Configuration in the Controller Service section as follows
![Enable PutBigQuery](https://github.com/aisyahputami/supermarket-sales/blob/main/ingestion-streaming/enable-put-bigquery.png)


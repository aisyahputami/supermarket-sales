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



## Extraction
Create processor ConsumerKafka. This processor is used in Apache NiFi to retrieve data from Kafka topics.
![NiFi ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/consumer-kafka.png)

Next, configure the settings for the ConsumerKafka processor in the "Relationships" and "Properties" sections as follows
![Config Relationship ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/config-consumer-kafka-relationship.png)

![Config ConsumerKafka](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/config-consumer-kafka-properties.png)

Create LogAttribute Processor. LogAttribute Processor serves as a valuable tool for monitoring, auditing, and troubleshooting data flows in Apache NiFi by providing visibility into the attributes of FlowFiles as they move through the system.

![NiFi LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/logattribute-processor.png)

Next, configure the settings for the ConsumerKafka processor in the "Connection" and "Relationships" sections as follows

![NiFi Connection LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/logattribute-processor.png)

![NiFi Relationships LogAttribute](https://github.com/aisyahputami/supermarket-sales/blob/main/extraction/config-logattribute-relationship.png)







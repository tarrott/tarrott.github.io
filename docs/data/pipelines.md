# Data Pipelines
- facilitate the efficient flow of data from one point to another
- flow of data extraction, transformation, combination, validation, converging of data from multiple streams into one
- facilitate parallel processing using distributed data processing

1. data standardization (cleanup beforehand to avoid processing errors)
2. data processing (segregate data into flows based on business requirements & route it to specific destinations)
3. data analysis (models and machine learning)
4. data visualization (present gathered intel to stakeholders)
5. data storage & security (archive and encrypt data)

---
## Data Ingestion
- Data collection layer
- Data query layer
- Data processing layer
- Data visualization layer
- Data storage layer
- Data security layer

### Real-time streams
- important for time-critical data (medical, financial, etc.)
- can be slow and require smaller data sets
- Apache Spark, Storm, Kafka, Flink are high-performant due to in-memory storage abd examples of distributed data processing engines capable of processing real-time streaming data
- Spark integrates seemlessly with HDFS, S3, etc.

### Batch
- analyzing trends over time
- can include full data sets due to it not being time-critical
- ETL flow
- Hadoop implementing MapReduce is lower performant due to disk usage and is used in batch processing data

### Use Cases
- momving big data into a Hadoop cluster to process and run analytics
- moving data (and continuing to update) from the primary storage / legacy systems into Elasticsearch to be indexed and searched on more quickly
- processing and aggregating logs and metrics from servers, services, and IoT devices into Elasticsearch or other time-series databases to be visualized and run analytics on them
- real-time stream processing engines for real-time events (sports, financial, medical, etc.) to dump into message queues such as Kafka or stream computation frameworks (Storm, Nifi, Spark, Flink, Samza, Kinesis, etc.) to implement real-time large-scale data processing features

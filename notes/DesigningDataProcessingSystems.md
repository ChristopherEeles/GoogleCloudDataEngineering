# Designing Data Processing Systems

## Data Processing Anatomy

* **Representation**: What is the data and its structure?
* **Pipeline**: What transformations or operations are needed?
* **Infrastructure**: What services will run your pipelines?

## Data Engineering Services

### Storage and Databases

Mechanisms for transformation and analysis external to the service.

Managed:
* Cloud SQL
* Cloud Spanner
* Cloud Bigtable

Serverless:
* Cloud Storage
* Firestore

### Server-based Processing

Processing:
* Compute Engine
* App Engine
* Google Kubernetes Engine
* Cloud Functions
* Cloud Run

Managed:
* Dataproc

Serverless:
* BigQuery
* DataFlow

### Artifical Intelligence

Model Building:
* Vertex AI
* Cloud TPU
* AutoML

Pre-built Models:
* Vision API
* Speech-to-Text API
* Video Intelligence API
* Cloud Natural Language API
* Cloud Translation API

### Pre- and post-processing Services

* Transfer Appliance: Sync external data sources to the cloud
* Dataprep: Prepare of condition data before processing
* Notebooks (Vertex AI/AI Platform): Self contained workspace with code/results
* Google Data Studio: Visualization of data after processing
* Dialogflow: Chatbot service

### Infrastructure Services

Operations:
* Google Cloud Operations Suite
* Google Cloud Console and Cloud Shell
* Montoring
* Logging
* Error Reporting
* Trace
* Debugger Profiler

Messaging:
* Pub/Sub: messaging service, storage messages for up to 7 days

Automation:
* Terraform
* Cloud Composer
* Cloud APIs

Networking:
* Cloud VPN
* Partner Interconnect
* Dedicated Interconnect
* Cloud IoT Core
* Cloud Endpoints

Security:
* Google Cloud Armour
* Firewall rules
* IAM
* Cloud KMS

## Designing Flexible Data Representations

Selecting a data abstraction has certain benefits and trade-offs

### Data Representations

* Cloud Storage is an Object stored in a Bucket
* Datastore is a property, contained in an Entity and is in a Kind category
* Cloud SQL and Cloud Spanner consist of Values stored in Rows and Columns in a Table in Database

### BigQuery Concepts

Columnar data store optimized for column reads

* Project: contains users and datasets
  * Limit access to datasets and jobs
  * Manage billing
* Dataset: contains tables and views
  * Access Control Lists for Reader/Writer/Owner
  * Applied to all tables/views in dataset
* Table: collection of columns
  * Columnar storage
  * Views are virtual tables defined by SQL query
  * Tables can be external (e.g., on Cloud Storage)
* Job: potentially long running action
  * Can be cancelled

### BigQuery vs RDBMS

BigQuery:
* Each column is a seperate, compressed, encrypted file that is replicated 3+ times
* No indexes, keys, or paritions are required
* For immutable, massive datasetss
* Streaming appends are cheap, but modifying existing data is expensive

Relational Database:
* Record(row)-oriented storage
* Supports transactional updates

### Dataproc

Cloud Spark service. Spark hides data complexiting with an abstraction:
Resilient Distrubted Datasets (RDDs). This allows Spark to make decision on
your behalf.

### Dataflow

Serverless data pipelining tool for tranforming data from one source and
depositing in another. Data stored in (immutable) PCollections and each
transform is a step. All steps comprise a pipeline.

Features:
* Each step is elastically scaled
* Each Tranfrom is applied on a PCollection
* The results of an `apply()` is another PCollection
* Uses same steps for both Batch and Streaming data

### Tensorflow

Data represented in tensors, where an N-dimensional array of data. Tensors are
very good for representing mathemetical operations and makes solving ML
problems much easier.

## Desigining Data Pipelines

### Dataproc

Managed Hadoop service, including Hadoop, Pig, Hive and Spark.

Benefits
* Hadoop based, so familiar to users of this ecosystem
* Automated cluster management
* Fast cluster resize
* Flexible VM configurations

Cluster node options:
* Single node (for experimentation)
* Standard (1 primary) or High available (3 primaries) for production

HDFS:
* Use Cloud Storage for a stateless solution

HBASE:
* Use Cloud BigTable for a stateless solution

Objectives:
* Shut down the cluster when it is not actually running jobs
* Start a cluster per job or from a particular kind of work

Data storage:
* Don't use HDFS to store input/output data; use it for temporary storage
* Disk performance scales with cluster size

Cloud storage:
* Match your data location with your compute location; zone matters
* Machine types and preemptible VMs

Spark:
* Transfroms are lazy, get registered in the DAG and runs when all precursors are ready
* Actions are eagerly executed

Connectors:
* Dataproc has connectors to many GCP services as both input and output

## Dataflow Pipelines

* Write Java and Python code to deploy it to Dataflow, which then executes the pipelines.
* Open-source API (Apache Beam) can also be executed on Flink, Spark, etc.
* Parallel tasks are autoscaled by execution framework
* Same code does both real-time (streaming) and batch processing
* Can get input from mnay sources and write to several sinks while the pipeline code remains unchanged
* Can put code inside a servlet, deploy it to App Engine, and schedule a CRON task queue in App Engine to periodically execute the pipeline
* Dataflow supports side inputs, which allows different transfromation on data in the same pipeline
* Security is implemeted using user roles to access the Dataflow resources

### Pipelines

* Rendered in the UI as a diagram along with the code
* Transforms can be executed in parallel

### Operations

Pipelines are often organized into Map and Reduce sequences. Side inputs are
parallel paths of execution making different transfromations on the same source
data.

* ParDo: Allows for parallel processing
* Map: 1:1 relationship between input and output in Python
* FlatMap: For non 1:1 relationships, using with generator in Python
* .apply(ParDo): Java for Map and FlatMap
* GroupBy: Shuffle
* GroupByKey: Explicit shuffle
* Combine: Aggregate values

### Templates

Allow data pipeline consumers to use prebuilt pipelines. Seperate concerns from
producers and consumers of the data pipelines.

## BigQuery and Dataflow Solutions

Two services: a front end service that does analysis and a back-end service
the does storage. Together these services form a Data Warehouse solution.

Access to datasets in BigQuery is controlled at many levels, from the project
and dataset to the individual table, column and row.

Features:
* Near real-time analysis of massive datasets
* NoOps
* Pay for use
* Data storage is inexpensive; queries charged on amount of data processed
* Durable (replicated), inexpensive storage
* Immutable audit logs
* Mashing up different datasets to derive new insights
* Interactive analysis of petabyte-scale databases
* Fimiliar, SQL 2011 query language and functions
* Many ways to ingest, transform, load and export data to/from BigQuery
* Nested and repetaed fields, user defined functions in JavaScript
* Structured data, primarily for analytics, latency of seconds is okay

### BigQuery Solutions

BigQuery analysis engine usually used with BigQuery storage (Tables), but
also has connectors for Cloud Storage (Files) and Cloud BigTable (NoSQL).
Enabled by cloud network with Petabit speeds.

### Cloud Dataproc

Can use output BigQuery storage from Dataproc and Dataflow. Allows
interoperability of different analytic engines based on the specific use case.

## Designing Data Processing Infrastructure

### Ingest Solution

Ingest from CSV, JSON or AVRO formats as well as load data directly from Cloud
Storage.

CLI: via `gsutil` tool for Cloud Storage and `bq` for BigQuery
Web UI: point and click uploads
API: REST APIs can be interected with directly (this is what the CLI tools use in the back-end)

### Ingesting to BigQuery

Three ways:
* Files on disk, Cloud Storage or Firestore
* Stream data in
* Federated sources

### Options for Transferring Storage

Think about the the 3Vs of data: velocity, volume and variety.

Use `gsutil` for uploading files, use Storage Transfer Service when the data
in in another location (e.g., another cloud) and use the Storage Transfer
Appliance when data is too big or sensitive to transfer electronically.

## Pub/Sub

* NoOps, serverless global message queue
* Asyncrhonous; publisher never wiats and subscriber can get message new or within 7 days
* At-least-once delivery quarantee
* Push subscription and pull subscription delivery flows
* 100s of milliseconds -- fast!

### Common Applications

Connects applications via a common messaging infrastructure, simplifying
event distribution using a high available asynchronous bus.

Features:
* Application connection
* Asynchronous
* Prevent over-provisioning
* Exactly once, ordered processing (with Dataflow)

### Serverless Analytic Solutions

* Pub/Sub for ingest
* Dataflow for processing
* BigQuery for (interactive) analysis
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

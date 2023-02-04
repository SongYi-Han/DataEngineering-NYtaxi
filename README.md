**Project infrastructure and tech stack**  
* Google Cloud Platform (GCP): Cloud-based auto-scaling platform by Google
* Google Cloud Storage (GCS): Data Lake
* BigQuery: Data Warehouse
* Terraform: Infrastructure-as-Code (IaC)
* Docker: Containerization
* SQL: Data Analysis & Exploration
* Prefect: Workflow Orchestration
* dbt: Data Transformation
* Spark: Distributed Processing
* Kafka: Streaming


# Module1: Data Pipeline with Docker and GCP 
* **Docker** 
  - containerize data ingestion porocess with Docker
* **GCP**
  - Setting up infrastructure on GCP with Terraform
  

# Module 2: Workflow Orchestration with Prefect
  * Prefect flow, task(ETL), blocks and collection
    * link
  * ETL with GCP & Prefect
    * Putting data to Google Cloud Storage
    *  From GCS to BigQuery
  * Parametrizing workflows
  * Schedules & Docker Storage with Infrastructure
    * Flow code storage
    * Running tasks in Docker
  * Prefect Cloud and additional resources
    * Running flows on GCP

# Module 3: Data Warehousing with Bigquery
  * Partitioning and clustering
  * BigQuery best practices
  * Internals of BigQuery
  * Integrating BigQuery with Airflow
  * BigQuery Machine Learning
  * Deployment
  
  # Module 4: Analytics (data transformation and visualization)
  * Data modeling with dbt + BigQuery in cloud
    * method 1) set up in web browser
      * link 
    * method 2) set up in CLI
      * install dbt
      * setup profiles.yml and run `dbt init`
  * running a dbt project in production
  * visualize with data studio 

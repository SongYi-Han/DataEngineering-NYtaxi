# Implement Data ingestion pipeline with Docker and Terraform on GCP


## 📁 datapipeline-docker : Containerize Data Pipeline with Docker
ㄴ data_ingestion.py  
ㄴ docker-compoese.yaml  


<img width="424" alt="Screen Shot 2023-01-31 at 21 04 11" src="https://user-images.githubusercontent.com/40763359/215870275-6658038f-d2ac-48af-9a97-5b565ec128bc.png">

Best practice for isolation : Create two containers for database server(postgres and pgAdmin) and another container to run python script

* **container1 : Running the ingestion script**
  - write data ingestion python script with argparse
  - wite docker file and run with `docker run`
  
* **container2,3 : Running Postgres and pgAdmin**
  - write Docker-compose YAML file and run multiple containers with `$ docker-compose up`


## 📁 gcp-terraform : Creating GCP Infrastructure with Terraform  
ㄴmain.tf  
ㄴvariable.tf  

**Execution**
```
# Refresh service-account's auth-token for this session
gcloud auth application-default login

# Initialize state file (.tfstate)
terraform init

# Check changes to new infra plan
terraform plan -var="project=<your-gcp-project-id>"

# Create new infra
terraform apply -var="project=<your-gcp-project-id>"

# Delete infra after your work, to avoid costs on any running services
terraform destroy

```

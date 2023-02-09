# Implement Data ingestion pipeline with Docker and GCP 


## Containerize Data Pipelinewith Docker

<img width="424" alt="Screen Shot 2023-01-31 at 21 04 11" src="https://user-images.githubusercontent.com/40763359/215870275-6658038f-d2ac-48af-9a97-5b565ec128bc.png">

Create two containers(postgres and pgAdmin) and another container to run python script

* **Running the ingestion script**
  - write data ingestion python script with argparse
  - Dockerizing the ingestion script
  
* **Running Postgres and pgAdmin with Docker-Compose**
  - write Docker-compose YAML file
  - Running multiple containers with `$ docker-compose up`


## Creating GCP Infrastructure with Terraform
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

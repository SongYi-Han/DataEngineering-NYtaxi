# Data Pipeline with Docker and GCP 

<img width="424" alt="Screen Shot 2023-01-31 at 21 04 11" src="https://user-images.githubusercontent.com/40763359/215870275-6658038f-d2ac-48af-9a97-5b565ec128bc.png">

## Containerize Data Pipelinewith Docker
Create two containers(postgres and pgAdmin) and load the data with python script

* **Running postgres container** 
  ```
   docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
  ```

    * How to connect to postgres
      * method1) using `pgcli`in command line
    
      ```
      pip install pgcli
      ```
      ```
      pgcli -h localhost -p 5432 -u root -d ny_taxi
      ```
      
      * mehotd2) using sqlalchemy in python script or jupyter notebook
          * checkout the `data_ingestion.py`
    
    
* **Running pgAdmin container and connect to postgres with docker network**
  - set up docker network 
  ```
  docker network create pg-network
  ```
  - Run Postgres 
  ```
  docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v c:/Users/alexe/git/data-engineering-zoomcamp/week_1_basics_n_setup/2_docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
  ```
  - Run pgAdmin
  ```
  docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin-2 \
  dpage/pgadmin4
  ```
- connect to pgAdmin web interface and create server 
  - host name : pg-database
  
* **running the ingestion script**
  - write data ingestion python script with argparse
  - Dockerizing the ingestion script
  
* **Anotehr way: Running Postgres and pgAdmin with Docker-Compose**
  - write Docker-compose YAML file
  - Running multiple containers with `docker-compose up`
  
 
 ## Setting up the environment on GCP VM
* Generating SSH keys
* Creating a virtual machine on GCP
* Connecting to the VM with SSH
* Installing Anaconda
* Installing Docker
* Creating SSH config file
* Accessing the remote machine with VS Code and SSH remote
* Installing docker-compose
* Installing pgcli
* Port-forwarding with VS code: connecting to pgAdmin and Jupyter from the local computer
* Installing Terraform
* Using sftp for putting the credentials to the remote machine
* Shutting down and removing the instance
 

## Creating GCP Infrastructure with Terraform
###  Execution
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

### Initial GCP Setup

1. Create an account with your Google email ID 
2. Setup your first [project](https://console.cloud.google.com/) if you haven't already
    * eg. "Project name", and note down the "Project ID" (we'll use this later when deploying infra with TF)
3. Setup [service account & authentication](https://cloud.google.com/docs/authentication/getting-started) for this project
    * Grant `Viewer` role to begin with.
    * Download service-account-keys (.json) for auth.
4. Setup permission 
   * [IAM Roles](https://cloud.google.com/storage/docs/access-control/iam-roles) for Service account:
   * Go to the *IAM* section of *IAM & Admin* https://console.cloud.google.com/iam-admin/iam
   * Click the *Edit principal* icon for your service account.
   * Add these roles in addition to *Viewer* : **Storage Admin** + **Storage Object Admin** + **BigQuery Admin**

   
### Setting up the environment on GCP VM
* Generating SSH keys to access from local
  * create ssh key (https://cloud.google.com/compute/docs/connect/create-ssh-keys)  
      by running `$ ssh-keygen -t rsa -f ~/.ssh/KEY_FILENAME -C USERNAME -b 2048` in your local command window
  * upload public key to computer engine - metadata
  
* Creating a VM instance

* Connecting to the VM with SSH
  * method1) Copy external IP of VM and connect with `$ ssh -i ~/.ssh/gcp usernamge@VM_External_IP`
  * method2) set up config file and connect with simple `$ ssh hostname(e.g.de-gcp)`
  ```
  cd ~/.ssh
  touch config
  
  Host de-gcp
    HostName 34.65.57.140
    User songyi
    IdentityFile ~/.ssh/gcp
  ```  

* install packages in VM
  * Installing Anaconda :
   ```
   wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh 
   bash Anaconda3-2018.12-Linux-x86_64.sh
   source .bashrc
   ```
  
  * Installing Docker : 
  ```
  sudo apt-get update
  sudo apt-get install docker.io
  ```

* Setup VS code to Access the remote machine with VS Code and SSH remote
   * install `remote ssh` extension
   * go to `connect host` in pallette  

* Installing docker-compose  
```
# install docker-compose
$ wget https://github.com/docker/compose/releases/download/v2.15.1/docker-compose-linux-x86_64 -O docker-compose

# make it executable
$ chmod +x docker-compose

# change path 
$ nano .bashrc    
  export PATH ="{HOME}/bin:${PATH}"
$ source .bashrc  

# test if docker compose is running
$ docker-compose up -d  
$ docker ps 
```
  
* Installing pgcli  

  `conda install -c conda-forge pgcli`
  `pip install `
  `pgcli -h localhost -U root -d ny_taxi`

* Port-forwarding with VS code: connecting to pgAdmin and Jupyter from the local computer

* load data by running code in Jupyter notebook or python
```
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz"

python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url=${URL}
```

* Installing Terraform  
```
$ wget https://releases.hashicorp.com/terraform/1.3.7/terraform_1.3.7_linux_amd64.zip

$ sudo apt-get install unzip 

$ unzip terraform_1.3.7_linux_amd64.zip
```

* Using sftp for putting the credentials to the remote machine  
   * local
   ```
   $ sftp hostname(de-gcp)  
   $ mkdir .cp
   $ put zoomcamp-de-377019-d4f0c120b492.json
   ```
   * remote
   ```
   #setting crefentials 
   `$ export GOOGLE_APPLICATION_CREDENTIALS=~/.gc/zoomcamp-de-377019-d4f0c120b492.json.json`   
   `$ gcloud auth activate-service-account --key-file $GOOGLE_APPLICATION_CREDENTIALS`
   ```

* running terraform 
```
$ terraform init
$ terraform plan 
$ terraform apply
```

* Shutting down and removing the instance
 `$ sudo shutdown now`
   
 

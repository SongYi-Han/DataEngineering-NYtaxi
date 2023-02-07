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
4. Create SSH key
5. Set environment variable to point to your downloaded GCP keys:
   ```shell
   export GOOGLE_APPLICATION_CREDENTIALS="<path/to/your/service-account-authkeys>.json"
   
   # Refresh token/session, and verify authentication
   gcloud auth application-default login
   ```
   
### Setting up the environment on GCP VM
* Generating SSH keys
  * create ssh key : https://cloud.google.com/compute/docs/connect/create-ssh-keys  
      `ssh-keygen -t rsa -f ~/.ssh/KEY_FILENAME -C USERNAME -b 2048`
  * upload public key to computer engine - metadata
  
* Creating a VM instance

* Creating SSH config file
  * config 
  ```
  Host de-gcp
    HostName 34.65.57.140
    User songyi
    IdentityFile ~/.ssh/gcp
  ```

* Connecting to the VM with SSH
  * Copy external IP of VM and connect with `ssh -i ~/.ssh/gcp usernamge@VM_External_IP`
  * Installing Anaconda : ` wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh` `source .bashrc`
  `bash Anaconda3-2018.12-Linux-x86_64.sh`
  * Installing Docker : `sudo apt-get update` `sudo apt-get install docker.io`
  

* Setup VS code to Access the remote machine with VS Code and SSH remote

* Installing docker-compose  
`https://github.com/docker/compose/releases/download/v2.15.1/docker-compose-linux-x86_64 -O docker-compose`
`chmod +x docker-compose`
`nano .bashrc`    
  ```
  export PATH ="{HOME}/bin:${PATH}"
  ```
  
  `source .bashrc`  
  
  `docker-compose up -d`  
  
  `docker ps` 
  
* Installing pgcli  

  `conda install -c conda-forge pgcli`
  `pip install `
  `pgcli -h localhost -U root -d ny_taxi`

* Port-forwarding with VS code: connecting to pgAdmin and Jupyter from the local computer

* load data by running code in Jupyter notebook

* Installing Terraform  
`$ wget terraform`
`$ sudo `
`$ unzip`

* Using sftp for putting the credentials to the remote machine  
`$ sftp hostname`  
`$ export GOOGLE_APPLICATION_CREDENTIALS=~/ .gc/ny-rides.json`  
`$ gcloud auth activate-service-account --key-file $GOOGLE_APPLICATION_CREDENTIALS`  
`$ terraform init`
`$ terraform plan`
`$ terraform apply`


* Shutting down and removing the instance
 `$ sudo shutdown now`
   
 

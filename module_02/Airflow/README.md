## Project Structure:

* `./dags` - `DAG_FOLDER` for DAG files (use `./dags_local` for the local ingestion DAG)
* `./logs` - contains logs from task execution and scheduler.
* `./plugins` - custom plugins and helper funtion

 ## Airflow setup with docker
 ### Pre-Reqs

1. For the sake of standardization across this workshop's config,
    rename your gcp-service-accounts-credentials file to `google_credentials.json` & store it in your `$HOME` directory
    ``` bash
        cd ~ && mkdir -p ~/.google/credentials/
        mv <path/to/your/service-account-authkeys>.json ~/.google/credentials/google_credentials.json
    ```

2. You may need to upgrade your docker-compose version to v2.x+, and set the memory for your Docker Engine to minimum 5GB
(ideally 8GB). If enough memory is not allocated, it might lead to airflow-webserver continuously restarting.

3. Python version: 3.7+


### Airflow Setup

1. Create a new sub-directory called `airflow` in your `project` dir (such as the one we're currently in)

2. **Set the Airflow user**:

    On Linux, the quick-start needs to know your host user-id and needs to have group id set to 0. 
    Otherwise the files created in `dags`, `logs` and `plugins` will be created with root user. 
    You have to make sure to configure them for the docker-compose:

    ```bash
    mkdir -p ./dags ./logs ./plugins
    echo -e "AIRFLOW_UID=$(id -u)" > .env
    ```

    On Windows you will probably also need it. If you use MINGW/GitBash, execute the same command. 

    To get rid of the warning ("AIRFLOW_UID is not set"), you can create `.env` file with
    this content:

    ```
    AIRFLOW_UID=50000
    ```

   
3. **Import the official docker setup file** from the latest Airflow version:
   ```shell
   curl -LfO 'https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml'
   ```
   It could be overwhelming to see a lot of services in here. 
   But this is only a quick-start template, and as you proceed you'll figure out which unused services can be removed.
   Eg. [Here's](docker-compose-nofrills.yml) a no-frills version of that template.

5. **Docker Build**:

    When you want to run Airflow locally, you might want to use an extended image, 
    containing some additional dependencies - for example you might add new python packages, 
    or upgrade airflow providers to a later version.
    
    Create a `Dockerfile` pointing to Airflow version you've just downloaded, 
    such as `apache/airflow:2.2.3`, as the base image,
       
    And customize this `Dockerfile` by:
    * Adding your custom packages to be installed. The one we'll need the most is `gcloud` to connect with the GCS bucket/Data Lake.
    * Also, integrating `requirements.txt` to install libraries via  `pip install`

6. **Docker Compose**:

    Back in your `docker-compose.yaml`:
   * In `x-airflow-common`: 
     * Remove the `image` tag, to replace it with your `build` from your Dockerfile, as shown
     * Mount your `google_credentials` in `volumes` section as read-only
     * Set environment variables: `GCP_PROJECT_ID`, `GCP_GCS_BUCKET`, `GOOGLE_APPLICATION_CREDENTIALS` & `AIRFLOW_CONN_GOOGLE_CLOUD_DEFAULT`, as per your config.
   * Change `AIRFLOW__CORE__LOAD_EXAMPLES` to `false` (optional)

7. Here's how the final versions of your [Dockerfile](./Dockerfile) and [docker-compose.yml](./docker-compose.yaml) should look.



 ### Execution
 
  1. Build the image (only first-time, or when there's any change in the `Dockerfile`, takes ~15 mins for the first-time):
     ```shell
     docker-compose build
     ```
     or (for legacy versions)
   
     ```shell
     docker build .
     ```

 2. Initialize the Airflow scheduler, DB, and other config
    ```shell
    docker-compose up airflow-init
    ```

 3. Kick up the all the services from the container:
    ```shell
    docker-compose up
    ```

 4. In another terminal, run `docker-compose ps` to see which containers are up & running (there should be 7, matching with the services in your docker-compose file).

 5. Login to Airflow web UI on `localhost:8080` with default creds: `airflow/airflow`

 6. Run your DAG on the Web Console.

 7. On finishing your run or to shut down the container/s:
    ```shell
    docker-compose down
    ```

    To stop and delete containers, delete volumes with database data, and download images, run:
    ```
    docker-compose down --volumes --rmi all
    ```

    or
    ```
    docker-compose down --volumes --remove-orphans
    ```
 


# BigQuery Best Practice 
* Cost reduction
  * Avoid SELECT *
  * Price your queries before running them
  * Use clustered or partitioned tables
  * Use streaming inserts with caution
  * Materialize query results in stages
* Query performance
  * Filter on partitioned columns
  * Denormalizing data
  * Use nested or repeated columns
  * Reduce data before using a JOIN
  * Do not treat WITH clauses as prepared statements

# ML with BQ 
* official document : https://cloud.google.com/bigquery-ml/docs/tutorials

<img width="652" alt="Screen Shot 2023-02-04 at 12 48 23 PM" src="https://user-images.githubusercontent.com/40763359/216765967-fda6ffda-2ffa-4049-9f6c-0aa65acb9179.png">

* Model Deployment 
```
$ gcloud auth login

$ bq --project_id taxi-rides-ny extract -m nytaxi.tip_model gs://taxi_ml_model/tip_model

$ mkdir /tmp/model

$ gsutil cp -r gs://taxi_ml_model/tip_model /tmp/model

$ mkdir -p serving_dir/tip_model/1

$ cp -r /tmp/model/tip_model/* serving_dir/tip_model/1

$ docker pull tensorflow/serving

$ docker run -p 8501:8501 --mount type=bind,source=pwd/serving_dir/tip_model,target= /models/tip_model -e MODEL_NAME=tip_model -t tensorflow/serving &

$ curl -d '{"instances": [{"passenger_count":1, "trip_distance":12.2, "PULocationID":"193", "DOLocationID":"264", "payment_type":"2","fare_amount":20.4,"tolls_amount":0.0}]}' -X POST http://localhost:8501/v1/models/tip_model:predict

http://localhost:8501/v1/models/tip_model

```

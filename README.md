# MLOps

```shell
gcloud iam service-accounts create SERVICE_ACCOUNT_NAME \
  --description="DESCRIPTION" \
  --display-name="DISPLAY_NAME"

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/aiplatform.customCodeServiceAgent

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/aiplatform.admin

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/storage.objectAdmin
```

## hands-on Vertex AI Custom Training gcloud cli
1. Create a service account and grant the necessary permissions

```shell
gcloud iam service-accounts create SERVICE_ACCOUNT_NAME \
  --description="DESCRIPTION" \
  --display-name="DISPLAY_NAME"

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/aiplatform.customCodeServiceAgent

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/aiplatform.admin

gcloud projects add-iam-policy-binding udemy-mlops \
    --member=serviceAccount:vertexai-sa@udemy-mlops.iam.gserviceaccount.com \
    --role=roles/storage.objectAdmin
```

2. Build the image locally

```shell
docker build -t vertex-bikeshare-model .
```

3. Tag the image locally

```shell
docker tag vertex-bikeshare-model gcr.io/udemy-mlops/vertex-bikeshare-model
```

4. Push the image to GCR

```shell
docker push gcr.io/udemy-mlops/vertex-bikeshare-model
```

5. Submit a custom model training job using gcloud

```shell
gcloud ai custom-jobs create --region=us-central1 \
--project=udemy-mlops \
--worker-pool-spec=replica-count=1,machine-type='n1-standard-4',container-image-uri='gcr.io/udemy-mlops/vertex-bikeshare-model' \
--service-account=SERVICE_ACCOUNT
--display-name=bike-sharing-model-training
```


---


## CI CD

1. create bucket
2. upload dataset
3. 

```shell
# Assign Service account user role to the service account 
gcloud projects add-iam-policy-binding udemy-mlops \
--member=serviceAccount:1090925531874@cloudbuild.gserviceaccount.com --role=roles/aiplatform.admin
```

Cloud Build

1. Build Docker Image
2. Push Docker Image To GCR
3. Execute Tests
4. Submit Custom Training Job
5. Upload Model to Model Registry
6. Fetch Model ID
7. Create Endpoint
8. Deploy Model Endpoint

### model_training_code.py

1. load data
2. preprocess data (rename columns, drop columns, one-hot-encodings)
3. train test split
4. train model
5. dump model("gs://sid-vertex-mlops/bike-share-rf-regression-artifact/model.joblib")
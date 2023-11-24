### Build docker image ###
```console
docker build -t docker-spark:0.0.0.1 .
```

### Create service account ###
```console
kubectl create serviceaccount sa-spark
```

### Get master URL ###
```console
kubectl cluster-info
```

Replace https with k8s.

### Run spark submit ###
```console
./bin/spark-submit --master k8s://192.168.49.2:8443 --deploy-mode cluster --name spark-docker --class org.example.Main --conf spark.executor.instances=2 --conf spark.kubernetes.container.image=docker-spark:0.0.0.1 --conf spark.kubernetes.authenticate.driver.serviceAccountName=sa-spark --conf spark.kubernetes.namespace=d4rkc0de --conf spark.kubernetes.file.upload.path=/tmp/ "local:///opt/spark/work-dir/app.jar"
```

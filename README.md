Dockerfile and entrypoint.sh can be found here: https://github.com/apache/spark-docker

The used namespace is called ```d4rkc0de``` for the below commands.

### Build docker image ###
```console
docker build -t docker-spark:0.0.0.1 .
```

### Create service account and give it the edit role on the cluster ###
```console
kubectl create serviceaccount sa-spark
kubectl create clusterrolebinding spark-role --clusterrole cluster-admin --serviceaccount=d4rkc0de:sa-spark --namespace=d4rkc0de
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

### Run using a pod template ###
```console
kubectl apply -f pod.yaml
```

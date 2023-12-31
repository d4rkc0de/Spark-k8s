There is a Dockerfile and entrypoint.sh per spark version, you can found them here: https://github.com/apache/spark-docker

This project use spark v3.3.2, if you want to use a Dockerfile of another version, you should add this to your new Dockerfile: 
```console
COPY app.jar /opt/spark/work-dir/
```

The used namespace is called ```d4rkc0de``` for the below commands.

### Build docker image ###
```console
docker build -t docker-spark:0.0.0.1 .
```

### Create service account and give it the edit role on the cluster ###
```console
kubectl create serviceaccount sa-spark
kubectl create clusterrolebinding spark-role --clusterrole=edit --serviceaccount=d4rkc0de:sa-spark --namespace=d4rkc0de
```

### Get master URL ###
```console
kubectl cluster-info
```

Replace https with k8s and replace the ```--master``` parameter with the new master url.

### Run using spark submit ###
```console
./bin/spark-submit --master k8s://192.168.49.2:8443 --deploy-mode cluster --name spark-docker --class org.example.Main --conf spark.executor.instances=2 --conf spark.kubernetes.container.image=docker-spark:0.0.0.1 --conf spark.kubernetes.authenticate.driver.serviceAccountName=sa-spark --conf spark.kubernetes.namespace=d4rkc0de --conf spark.kubernetes.file.upload.path=/tmp/ "local:///opt/spark/work-dir/app.jar"
```

### Run using a pod template ###
```console
kubectl apply -f pod.yaml
```

### Build docker image ###

docker build -t docker-spark:0.0.0.1 .

### Run spark sumbmit ###

./bin/spark-submit --master k8s://https://192.168.49.2:8443 --deploy-mode cluster --name spark-docker --class org.example.Main --conf spark.executor.instances=2 --conf spark.kubernetes.container.image=docker-spark:0.0.0.1 --conf spark.kubernetes.authenticate.driver.serviceAccountName=sa-spark --conf spark.kubernetes.namespace=d4rkc0de --conf spark.kubernetes.file.upload.path=/tmp/ "local:///opt/spark/work-dir/app.jar"

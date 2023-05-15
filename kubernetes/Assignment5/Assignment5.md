# Create Deployment object for MetadataService , Mongo Database 
    NOTE: Avoid using ReplicaSet or Pod directly. Best practice is to use Deployment for everything even if you have just one pod.

    MetdataService pod deployment -> metadata-service-pod-deployment.yaml
    Mongo db pod deployment -> mongodb-pod-deployment.yaml
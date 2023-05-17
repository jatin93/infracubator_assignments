# Use a env mongo URL in metadata service
# Create a configmap for mongo configuration url and pass it as ENV to metadata-service under key: MONGODB_URI
    configmap -> metadata-service-configmap.yaml
    MetadataService pod with configmap -> metadata-service-pod-configmap-deployment.yaml
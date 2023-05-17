# Use a env mongo URL in metadata service
# Create a configmap for mongo configuration url and pass it as ENV to metadata-service under key: MONGODB_URI
    Create secret
        kubectl create secret generic metadata-service-secret --from-literal=MONGODB_URI=mongodb://mongo/metadata

    MetadataService pod with secret -> metadata-service-pod-configmap-deployment.yaml
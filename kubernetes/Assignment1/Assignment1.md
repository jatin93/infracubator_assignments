# Assignment 1

# Create a Pod specification for the metadataservice
    image : luckyganesh/metadata-service:v1.0 ( for M1 chip - luckyganesh/metadata-service:v1.0-arm )
    Port to be exposed 8080

    kubectl create -f <path-to-file-containing-pod-creation-spec.yaml>
    kubectl create -f pod-defination.yaml

# Check the logs
    kubectl logs -f <podName> <containerName>
    kubectl logs -f metadataservice metaservice

# Exec into the container using /bin/sh command.
    kubectl exec -it <podname>  <command>
    kubectl exec -it metadataservice  /bin/sh

# Check the Pod IP
    kubectl get pods metadataservice -o yaml
    Find the field podIP
        OR
    kubectl describe pod metadataservice
    IP field under the pod details above the container details

# Describe the POD
    kubectl describe pod <podname>
    kubectl describe pod metadataservice

# Hit the POD ip from either insider minikube ( minikube ssh ) or colima ( colima ssh ) based on whatever you are using.
    minikube ssh
    curl http://10.244.0.14:8080/

    Response
        {"timestamp":"2023-05-10T11:19:42.853+0000","status":404,"error":"Not Found","message":"No message available","path":"/"}$ 

# Assignment 2

# Create a replicaset for metadataservice and deploy in k8s cluster
    file -> replicaset.yaml

# Add liveness & readiness probes using spring actuator's /actuator/health endpoint
    file -> replicaset.yaml

# POST to create an meta entry in the database 
    minikube ssh
    curl --header "Content-Type: application/json" --request POST --data '{"group":"sunitparekh","name":"city","value":"Pune"}' http://<pod-ip:8080/metadata
    IP for first pod = 10.244.0.26
    IP for first pod = 10.244.0.25

# GET all meta entry posted in step 1
    minikube ssh
    curl http://<pod-Ip>:8080/metadata
    IP for first pod = 10.244.0.26
    IP for first pod = 10.244.0.25
# Create NodePort Service for the MetadataService and access following via NodePort from host machine
    file -> service-definition.yaml

# POST to create an meta entry in the database
## Hitting the service from host outside the minikube
    minikube ip -> 192.168.64.2
    nodePort ->  30080
    curl --header "Content-Type: application/json" --request POST --data '{"group":"sunitparekh","name":"city","value":"Pune"}' http://192.168.64.2:30080//metadata
        RESPONSE
            {"id":"645cd1b95aaef20001d8f26a","message":"Successfully saved metadata."}%

## Hitting the service from inside the minikube 
    service ip -> kubectl describe service metadatanodeport-service
    IP:  10.97.11.252
    port -> 8080

    minikube ssh
    curl --header "Content-Type: application/json" --request POST --data '{"group":"inside-minikube","name":"city","value":"Pune"}' http://10.97.11.252:8080/metadata
        RESPONSE
            {"id":"645cd9295aaef20001d8f275","message":"Successfully saved metadata."}$


# GET all meta entry posted in step 1
## Hitting the service from host outside the minikube
    minikube ip -> 192.168.64.2
    nodePort ->  30080

    curl http://192.168.64.2:30080/metadata
        RESPONSE
            [{"group":"inside-minikube","lastUpdatedTs":"2023-05-11T12:05:30.006","name":"city","id":"645cd9295aaef20001d8f275"},{"group":"test data","lastUpdatedTs":"2023-05-11T12:05:30.006","name":"city","id":"645cd4bf5aaef20001d8f271"},{"group":"sunitparekh","lastUpdatedTs":"2023-05-11T12:05:30.007","name":"city","id":"645cd1b95aaef20001d8f26a"}]%


## Hitting the service from inside the minikube 
    minikube ssh
    curl http://10.97.11.252:8080/metadata
        RESPONSE
            [{"group":"inside-minikube","lastUpdatedTs":"2023-05-11T12:07:21.648","name":"city","id":"645cd9295aaef20001d8f275"},{"group":"test data","lastUpdatedTs":"2023-05-11T12:07:21.648","name":"city","id":"645cd4bf5aaef20001d8f271"},{"group":"sunitparekh","lastUpdatedTs":"2023-05-11T12:07:21.649","name":"city","id":"645cd1b95aaef20001d8f26a"}]$

    

# NOTE: If GET returning empty array (no result) or only few entries, goto nextexercise to learn why?
    Getting all entries from inside the minikube and also from host machine
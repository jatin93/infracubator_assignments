# Create NodePort Service for the MetadataService and access following via NodePort from host machine
    file -> service-definition.yaml

# POST to create an meta entry in the database
    curl --header "Content-Type: application/json" --request POST --data '{"group":"sunitparekh","name":"city","value":"Pune"}' http://10.99.223.15:8080//metadata

    curl --header "Content-Type: application/json" --request POST --data '{"group":"sunitparekh","name":"city","value":"Pune"}' http://10.244.0.28:8080/metadata

    node-ip -> kubectl describe service metadatanodeport-service
    IP:  10.99.223.15
    Endpoints:  10.244.0.28:8080

    curl http://10.99.223.15:8080/metadata
    curl http://10.244.0.28:8080/metadata


# GET all meta entry posted in step 1
    node-ip -> kubectl describe service metadatanodeport-service
    IP:  10.99.223.15
    Endpoints:  10.244.0.28:8080

    curl http://10.99.223.15:8080/metadata
    curl http://10.244.0.28:8080/metadata



# NOTE: If GET returning empty array (no result) or only few entries, goto nextexercise to learn why?
    Getting all entries
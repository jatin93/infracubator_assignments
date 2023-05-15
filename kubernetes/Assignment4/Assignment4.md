
# Deploy new Pod using mongodb image (https://hub.docker.com//mongo)
    file -> mongo-pod.yaml
    kubectl create -f mongo-pod.yaml

# Create ClusterIP service for mongodb with service name as mongo, so that database is available via mongo DNS name (mongo database runs by default on 27017 port)
    file -> mongo-service.yaml
    kubectl create -f mongo-service.yaml

# Change image in MetadataService to sunitparekh/metadata:v2.0 (connect to database at mongo hostname by passing MONGODB_URI=mongodb://mongo/metadata environment variable inside container spec)
    file -> pod-by-replicaset.yaml
    kubectl create -f pod-by-replicaset.yaml

# Try again POST and GET for Metadata Service, everything work as expected. NOTE: Remember to start mongo first and then meta service.
    minikube ip
        192.168.64.6
    

    POST Request
        curl --header "Content-Type: application/json" --request POST --data '{"group":"inside-minikube","name":"city","value":"Pune"}' http://192.168.64.6:30080/metadata {"id":"64624d091e58ca0001a36bba","message":"Successfully saved metadata."}%

        curl --header "Content-Type: application/json" --request POST --data '{"group":"fomo","name":"city","value":"Pune"}' http://192.168.64.6:30080/metadata {"id":"64624d151e58ca0001a36bbb","message":"Successfully saved metadata."}

        curl --header "Content-Type: application/json" --request POST --data '{"group":"yolo","name":"city","value":"Pune"}' http://192.168.64.6:30080/metadata {"id":"64624d211e58ca0001a36bbc","message":"Successfully saved metadata."}%

        curl --header "Content-Type: application/json" --request POST --data '{"group":"hello","name":"city","value":"Pune"}' http://192.168.64.6:30080/metadata {"id":"64624d2b1e58ca0001a36bbd","message":"Successfully saved metadata."}%


    GET Request
        curl http://192.168.64.6:30080/metadata
    
    GET Response
        [{"group":"hello","lastUpdatedTs":"2023-05-15T15:18:16.215","name":"city","id":"64624d2b1e58ca0001a36bbd"},{"group":"yolo","lastUpdatedTs":"2023-05-15T15:18:16.247","name":"city","id":"64624d211e58ca0001a36bbc"},{"group":"fomo","lastUpdatedTs":"2023-05-15T15:18:16.247","name":"city","id":"64624d151e58ca0001a36bbb"},{"group":"inside-minikube","lastUpdatedTs":"2023-05-15T15:18:16.248","name":"city","id":"64624d091e58ca0001a36bba"}]%

    POST Request
        curl --header "Content-Type: application/json" --request POST --data '{"group":"test-round2","name":"city","value":"Pune"}' http://192.168.64.6:30080/metadata {"id":"646251f9705c2700016b23f2","message":"Successfully saved metadata."}%

    Get Request
        curl http://192.168.64.6:30080/metadata
    
    GET Response
        [{"group":"test-round2","lastUpdatedTs":"2023-05-15T15:38:42.522","name":"city","id":"646251f9705c2700016b23f2"},{"group":"hello","lastUpdatedTs":"2023-05-15T15:38:42.523","name":"city","id":"64624d2b1e58ca0001a36bbd"},{"group":"yolo","lastUpdatedTs":"2023-05-15T15:38:42.523","name":"city","id":"64624d211e58ca0001a36bbc"},{"group":"fomo","lastUpdatedTs":"2023-05-15T15:38:42.523","name":"city","id":"64624d151e58ca0001a36bbb"},{"group":"inside-minikube","lastUpdatedTs":"2023-05-15T15:38:42.523","name":"city","id":"64624d091e58ca0001a36bba"}]%


# Note
    In assignment 4 unlike assignment 3 there was a single shared db which accross all the pods for metedataservice hence immaterial of the pod serving the post request the data insertion was done in the mongodb due to which when we made a get call all the inserted records are displayed


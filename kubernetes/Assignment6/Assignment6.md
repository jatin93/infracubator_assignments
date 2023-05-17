# Run the mongo database with the persistent volume using hostPath Read more documentation on how to setup mongodb with custom data folder path https://hub.docker.com//mongo/ (the mount path for mongo should be /data/db)

## Changes made to add a PersistentVolume
    Modifed the mongodb-pod-deployment.yaml file by adding the following changes
        - Under Pod Spec
            volumes:
                - name: mongo-pod-volume
                hostPath:
                    path: /home/docker/my-local-path/
                    type: Directory
        - Under ContainerSpec
            volumeMounts:
            - mountPath: /data/db
              name: mongo-pod-volume
    

    Under Pod Spec in volumes the value for hostPath /home/docker/my-local-path/ is created in minikube vm steps take for it are
        minikube ssh
        mkdir my-local-path
        cd my-local-path
        pwd command returns the path /home/docker/my-local-path/

    
    Under ContainerSpec the mountPath is /data/db which is the suggested path inside of the mongo container to be used since its where
    MongoDB by default will write its data files.

## Steps to test this
    Create the path /home/docker/my-local-path/ by doing minikube ssh
    In a new terminal hit following commands
        kubectl create -f mongodb-pod-deployment.yaml
        kubectl create -f service-mongo.yaml
        kubectl create -f metadata-service-pod-deployment.yaml
        kubectl create -f nodeport-service.yaml
        minikube ip

        curl --header "Content-Type: application/json" --request POST --data '{"group":"test-round2","name":"city","value":"Pune"}' http://192.168.64.11:30080/metadata

        curl --header "Content-Type: application/json" --request POST --data '{"group":"yolo","name":"city","value":"Pune"}' http://<minikube-ip>:30080/metadata

        curl --header "Content-Type: application/json" --request POST --data '{"group":"fomo","name":"city","value":"Pune"}' http://192.168.64.11:30080/metadata

        curl http://192.168.64.11:30080/metadata
            RESPONSE 
                [
                    {"lastUpdatedTs":"2023-05-17T15:03:15.577","group":"fomo","name":"city","id":"6464ec8387be2700011c0854"},{"lastUpdatedTs":"2023-05-17T15:03:15.579","group":"yolo","name":"city","id":"6464ec7887be2700011c0853"},{"lastUpdatedTs":"2023-05-17T15:03:15.579","group":"test-round2","name":"city","id":"6464ec6487be2700011c0852"}
                ]%


# Delete and Recreate MongoDB pod and see no data loss
        Now delete the mongo pod by comand 
            kubectl delete deployment mongo-deployment
        
        Hit the api call curl http://192.168.64.11:30080/metadata
            there should be no response as the mongodb pod is deleted

        Create a new deployment for mongo by
            kubectl create -f mongodb-pod-deployment.yaml

        Hit the api call curl http://192.168.64.11:30080/metadata again
            The response should be same as before deletion
                [
                    {"lastUpdatedTs":"2023-05-17T15:07:18.455","group":"fomo","name":"city","id":"6464ec8387be2700011c0854"},{"lastUpdatedTs":"2023-05-17T15:07:18.482","group":"yolo","name":"city","id":"6464ec7887be2700011c0853"},{"lastUpdatedTs":"2023-05-17T15:07:18.482","group":"test-round2","name":"city","id":"6464ec6487be2700011c0852"}
                ]%


# Note
    The reason for this data consistency is the persistent volume of type hostPath which is set in the Pod & container spec.
    Local minikube path of /home/docker/my-local-path/ is mounted to the /data/db path of the mongodb pod.
    So whatever data is stored in /data/db in the mongodb pod container is also stored in local minikube path /home/docker/my-local-path/
    Hence post deletion when a new pod is created its mapped to the local minikube path which has the data of old pod and when the new pod is
    up and running on hitting the get api call we get he old data in response.

    However on deleting the minikube session by doing minikube delete we remove the hostPath /home/docker/my-local-path/
    so in the next run when we again do minikube start and run the command to create mongo pod mounting to the local hostpath it fails
    beacuse in new minikube session/node that local path is not available








curl --header "Content-Type: application/json" --request POST --data '{"group":"yolo","name":"city","value":"Pune"}' http://192.168.64.10:30080/metadata
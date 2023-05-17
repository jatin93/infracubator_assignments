
# Run the mongo database with the persistent volume using PV and PVC
# Use hostPath to define PV

    PersistentVolume -> persistent-volume.yaml
    PersistentVolumeClaim -> persistent-volume-claim.yaml
    Using PersistentVolumeClaim to create a pod -> mongodb-pod-deployment-pvc.yaml

    kubectl create -f persistent-volume.yaml
    kubectl create -f persistent-volume-claim.yaml
    kubectl create -f mongodb-pod-deployment-pvc.yaml

    Since the hostPath(/home/docker/my-local-path) used in PersistentVolume (persistent-volume.yaml) is same as used in Assignment6,
    and the minikube session is also same so the data created for Assignment6 is also returned in the get curl call

    GET request
    curl http://192.168.64.11:30080/metadata
        [
            {"group":"fomo","lastUpdatedTs":"2023-05-17T17:24:54.102","name":"city","id":"6464ec8387be2700011c0854"},
            {"group":"yolo","lastUpdatedTs":"2023-05-17T17:24:54.155","name":"city","id":"6464ec7887be2700011c0853"},
            {"group":"test-round2","lastUpdatedTs":"2023-05-17T17:24:54.155","name":"city","id":"6464ec6487be2700011c0852"}
        ]%

    POST request
    curl --header "Content-Type: application/json" --request POST --data '{"group":"pv-test","name":"city","value":"Pune"}' http://192.168.64.11:30080/metadata

    POST request
    curl --header "Content-Type: application/json" --request POST --data '{"group":"pvc-test","name":"city","value":"Pune"}' http://192.168.64.11:30080/metadata

    GET request
    curl http://192.168.64.11:30080/metadata
        [
            {"group":"pvc-test","lastUpdatedTs":"2023-05-17T17:28:24.867","name":"city","id":"64650eaf29526c0001947684"},
            {"group":"pv-test","lastUpdatedTs":"2023-05-17T17:28:24.872","name":"city","id":"64650ea529526c0001947683"},
            {"group":"fomo","lastUpdatedTs":"2023-05-17T17:28:24.872","name":"city","id":"6464ec8387be2700011c0854"},
            {"group":"yolo","lastUpdatedTs":"2023-05-17T17:28:24.872","name":"city","id":"6464ec7887be2700011c0853"},
            {"group":"test-round2","lastUpdatedTs":"2023-05-17T17:28:24.872","name":"city","id":"6464ec6487be2700011c0852"}
        ]%


# Delete and Recreate MongoDB pod and see no data loss

    kubectl delete deployment mongo-deployment
    
    curl http://192.168.64.11:30080/metadata
        The call fails

    Create a mongo-deployment again
        kubectl create -f mongodb-pod-deployment-pvc.yaml
    
    curl http://192.168.64.11:30080/metadata
    [
        {"group":"pvc-test","lastUpdatedTs":"2023-05-17T17:53:03.464","name":"city","id":"64650eaf29526c0001947684"},
        {"group":"pv-test","lastUpdatedTs":"2023-05-17T17:53:03.493","name":"city","id":"64650ea529526c0001947683"},
        {"group":"fomo","lastUpdatedTs":"2023-05-17T17:53:03.493","name":"city","id":"6464ec8387be2700011c0854"},
        {"group":"yolo","lastUpdatedTs":"2023-05-17T17:53:03.493","name":"city","id":"6464ec7887be2700011c0853"},
        {"group":"test-round2","lastUpdatedTs":"2023-05-17T17:53:03.493","name":"city","id":"6464ec6487be2700011c0852"}
    ]%


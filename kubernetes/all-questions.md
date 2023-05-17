Create NodePort Service for the MetadataService and access following
via NodePort from host machine
→ POST to create an meta entry in the database
curl --header "Content-Type: application/json" \
--request POST \
--data '{"group":"sunitparekh","name":"city","value":"Pune"}' \
http://node-ip:nodeport/metadata
GET all meta entry posted in step 1
curl http://node-ip:nodeport/metadata/
NOTE: If GET returning empty array (no result) or only few entries, goto next
exercise to learn why?




Deploy new Pod using mongodb image (https://hub.docker.com//mongo)
Create ClusterIP service for mongodb with service name as mongo, so that
database is available via mongo DNS name (mongo database runs by default
on 27017 port)
Change image in MetadataService to sunitparekh/metadata:v2.0
(connect to database at mongo hostname by passing
MONGODB_URI=mongodb://mongo/metadata
environment variable inside
container spec)
Try again POST and GET for Metadata Service, everything work as expected.
NOTE: Remember to start mongo first and then meta service.



Create Deployment object for
MetadataService
Mongo Database
NOTE: Avoid using ReplicaSet or Pod directly. Best practice is to use
Deployment for everything even if you have just one pod.


Run the mongo database with the persistent volume using hostPath
Read more documentation on how to setup mongodb with custom data
folder path https://hub.docker.com//mongo/ (the mount path for
mongo should be /data/db)
Delete and Recreate MongoDB pod and see no data loss


→ Run the mongo database with the persistent volume using PV and PVC
-> Use hostPath to define PV
→ Delete and Recreate MongoDB pod and see no data loss


Use a env mongo URL in metadata service
Create a configmap for mongo configuration url and pass it as ENV to metadata-service under key: MONGODB_URI


Use a secret mongo URL in metadata service
Create a K8S Secret Object with mongo configuration url and pass it as ENV to metadata-service under key: MONGODB_URI
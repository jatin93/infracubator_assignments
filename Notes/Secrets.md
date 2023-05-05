# List secrets
    kubectl get secrets 

# View secret details with value 
    kubectl get secrets dashboard-token -o yaml

# View secrets
    kubectl describe secrets

# Create secret and attach to a pod

apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  DB_Host: sql01
  DB_User: root
  DB_Password: password123

            OR 

kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123

 To create pod 
    apiVersion: v1 
kind: Pod 
metadata:
  labels:
    name: webapp-pod
  name: webapp-pod
  namespace: default 
spec:
  containers:
  - image: kodekloud/simple-webapp-mysql
    imagePullPolicy: Always
    name: webapp
    envFrom:
    - secretRef:
        name: db-secret
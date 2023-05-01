# infracubator_assignments

# Docker Solutions added under each md file

# Commands
    docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
    docker run -v /opt/data:/var/lib/mysql -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 mysql
    docker run -d -p 5000:5000 --restart=always --name my-registry registry:2
    docker stop $(docker ps -a -q)
    docker rm $(docker ps -a -q)

      git clone -b remotes/origin/redis https://github.com/docker-ninja/go-app.git




# K8
ubectl run nginx --image nginx
kubectl describe pod newpods-fclmj
If you are not given a pod definition file, you may extract the definition to a file using the below command:
  kubectl get pod <pod-name> -o yaml > pod-definition.yaml
Use the kubectl edit pod <pod-name> command to edit pod properties.

kubectl get namespaces
kubectl get pods -n research
kubectl get pods --all-namespaces
to create pod in specific namespace add namespace option under metadata at level of label
kubectl get svc -n <namespace>

POD
Create an NGINX Pod

kubectl run nginx --image=nginx



Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml



Deployment
Create a deployment

kubectl create deployment --image=nginx nginx



Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run -o yaml



Generate Deployment with 4 Replicas

kubectl create deployment nginx --image=nginx --replicas=4



You can also scale deployment using the kubectl scale command.

kubectl scale deployment nginx --replicas=4



Another way to do this is to save the YAML definition to a file and modify

kubectl create deployment nginx --image=nginx--dry-run=client -o yaml > nginx-deployment.yaml



You can then update the YAML file with the replicas or any other field before creating the deployment.



Service
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors)

Or

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml (This will not use the pods' labels as selectors; instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work well if your pod has a different label set. So generate the file and modify the selectors before creating the service)



Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml

(This will not use the pods' labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.



Reference:

https://kubernetes.io/docs/reference/kubectl/conventions/
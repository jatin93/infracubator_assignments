# Get k8 pod logs
  kubectl -n elastic-stack logs kibana

# Exec into pod to view file contents
  kubectl -n elastic-stack exec -it app -- cat /log/app.log

# Edit the pod to add a sidecar container to send logs to Elastic Search. Mount the log volume to the sidecar container.Only add a new container. Do not modify anything else. Use the spec provided below.

apiVersion: v1 
kind: Pod 
metadata:
  labels:
    name: app
  name: app
  namespace: elastic-stack 
spec:
  containers:
  - image: kodekloud/event-simulator
    name: app
    imagePullPolicy: Always
    volumeMounts:
    - mountPath: /log
      name: log-volume
  - image: kodekloud/filebeat-configured
    name: sidecar
    imagePullPolicy: Always
    volumeMounts:
    - mountPath: /var/log/event-simulator/
      name: log-volume
  volumes:
  - name: log-volume
    hostPath:
      path: /var/log/webapp
      type: DirectoryOrCreate


# Create a pod red to use an initContainer that uses the busybox image and sleeps for 20 seconds
apiVersion: v1
kind: Pod
metadata:
  name: red
  namespace: default
spec:
  containers:
  - command:
    - sh
    - -c
    - echo The app is running! && sleep 3600
    image: busybox:1.28
    name: red-container
  initContainers:
  - image: busybox
    name: red-initcontainer
    command: 
      - "sleep"
      - "20"


# A new application orange is deployed. There is something wrong with it. Identify and fix the issue.
  Take the existing yaml file into dummy.yaml
  kubectl get pod orange -o yaml > dummy.yaml

  Delete the old pod
  kubectl delete pod orange

  fix the issue and run cmd
  kubectl create -f dummy.yaml 

# rediness probe with spec 
  ## Pod Name: simple-webapp-2
  ## Image Name: kodekloud/webapp-delayed-start
  ## Readiness Probe: httpGet
  ## Http Probe: /ready
  ## Http Port: 8080

    get current pod defination details
    kubectl get pod simple-webapp-2 -o yaml > pod-def.yaml

    Edit pod-def.yaml to add rediness probe
    apiVersion: v1
    kind: Pod
    metadata:
      creationTimestamp: "2023-05-05T19:00:47Z"
      labels:
        name: simple-webapp
      name: simple-webapp-2
      namespace: default
      resourceVersion: "1243"
      uid: 4fb469b0-5388-43bd-bd24-a5bfb95b1784
    spec:
      containers:
      - env:
        - name: APP_START_DELAY
          value: "80"
        image: kodekloud/webapp-delayed-start
        imagePullPolicy: Always
        name: simple-webapp
        readinessProbe: 
          httpGet:
            path: /ready
        port: 8080

# Create pods with a livenessProbe using the given spec
  ## Pod Name: simple-webapp-1
  ## Image Name: kodekloud/webapp-delayed-start
  ## Liveness Probe: httpGet
  ## Http Probe: /live
  ## Http Port: 8080
  ## Period Seconds: 1
  ## Initial Delay: 80
  ## Pod Name: simple-webapp-2
  ## Image Name: kodekloud/webapp-delayed-start
  ## Liveness Probe: httpGet
  ## Http Probe: /live
  ## Http Port: 8080
  ## Initial Delay: 80
  ## Period Seconds: 1

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2023-05-05T19:00:47Z"
  labels:
    name: simple-webapp
  name: simple-webapp-2
  namespace: default
  resourceVersion: "1243"
  uid: 4fb469b0-5388-43bd-bd24-a5bfb95b1784
spec:
  containers:
  - env:
    - name: APP_START_DELAY
      value: "80"
    image: kodekloud/webapp-delayed-start
    imagePullPolicy: Always
    name: simple-webapp
    livenessProbe: 
      httpGet:
        path: /live
        port: 8080
      initialDelaySeconds: 80
      periodSeconds: 1

# kubectl logs
  kubectl logs -f <podname> <containerNAme>

# Get resources based on selector
  kubectl get pods/replicaset/deployment --selector LabelName=LabelValue
  kubectl get all --selector LabelName=LabelValue,LabelName1=LabelValue1,LabelName2=LabelValue2
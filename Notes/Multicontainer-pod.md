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

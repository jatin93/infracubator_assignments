# Update the environment variable on the POD to use only the APP_COLOR key from the newly created ConfigMap.Note: Delete and recreate the POD. Only make the necessary changes. Do not modify the name of the Pod.

    config-map.yaml

    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: webapp-config-map
    data:
    APP_COLOR: darkblue
    APP_OTHER: disregard

    kubectl create -f config-map.yaml

    apiVersion: v1
    kind: Pod
    metadata:
    labels:
        name: webapp-color
    name: webapp-color
    namespace: default
    spec:
    containers:
    - env:
        - name: APP_COLOR
        valueFrom:
        configMapKeyRef:
            name: webapp-config-map
            key: APP_COLOR
        image: kodekloud/webapp-color
        name: webapp-color

    kubectl create -f poddefination.yaml
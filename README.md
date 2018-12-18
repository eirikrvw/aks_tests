# Testing 

This repo is created for testing of Pega deployment in containers only. It is
simple a proof of concept and getting to know/understand the way we can do this.

# Commands


## ACS
In some cases a private registry is used (needed) to build the containers running. 
Commands below are used to build and push an image in the registry so that it
can be used by kubernetes.

Initially the kubernetes cluster needs to be setup with the correct secrets to be 
able to login to ACS. 

```
# kubectl create secret docker-registry pega-acr-auth \
        --docker-server pegaregistry.azurecr.io i
        --docker-username <username> 
        --docker-password <password> 
        --docker-email test
# kubectl get secret pega-acr-auth --output="jsonpath={.data.\.dockerconfigjson}" | base64 --decode
```

In the case below a pega image is build and later on push

```
# docker build -t pega:0.1 -f Dockerfile .
# docker login https://pegaregistry.azurecr.io/
# docker tag pega:0.1 pegaregistry.azurecr.io/pega:0.1
# docker push pegaregistry.azurecr.io/pega:0.1
```

To reference this container in a pods deployment see the example
below

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: pega
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: pega
    spec:
      containers:
        - name:  pega
          image: pegaregistry.azurecr.io/postgres:pega:0.1
          imagePullPolicy: "Always"
          ports:
            - containerPort: 80
              name: http
            - containerPort: 443
              name: https
```

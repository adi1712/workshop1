# Deploying Application to Kubernetes from Scratch

## Install Kubectl

```
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/kubernetes.list 
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
## Install Minikube

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
Install as per latest release
https://github.com/kubernetes/minikube/releases

``` minikube start``` it will start a kubernetes minikube cluster
check the cluster status with ``` minikube status```

## Make the Application
Let's make a simple flask server

``` 
mkdir flask_app
cd flask_app
cat > hello.py <<EOF
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello Kubernetes!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
EOF

cat > requirements.txt <<EOF
FLask
EOF
```
## Write the Docker File
```
cat > Dockerfile <<EOF
FROM ubuntu:16.04
COPY .  /app
WORKDIR /app
RUN  apt-get update && apt-get -y upgrade
RUN  apt-get install -y python-pip
RUN  pip install -r requirements.txt
CMD  python hello.py
EOF
```
## Build the docker image
set ```eval $(minikube docker-env)``` to reuse the docker dameon from minikube
build the image with docker build
```
docker build -t hello-world:v1 .
```
check the image list with ```docker images```

## Write Kubernetes yaml file
```
cat > flask_deployment.yaml <<EOF
apiVersion: apps/v1 # 
kind: Deployment
metadata:
  name: flask-app
spec:
  selector:
    matchLabels:
      app: flask-app
  replicas: 2 # 
  template: # 
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: hello-worldv1
        image: hello-world:v1
        ports:
        - containerPort: 5000
EOF
```

## Deploy
```
kubectl apply -f my_deployment.yaml
```
check pods status with
```
kubectl get pods -l app=flask-app
```

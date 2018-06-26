# Kubernetes Object Model

To create an object, we need to provide `spec` field to kubernetes API server.Each object that you store has three parts: the metadata , a specification and the current observed status.As a user, you have to provide the metadata, and a specification in which you describe the desired state of the object

## Pods
pod is a smallest unit of kubernetes object. pod is a collection of containers which shares network namespace and volumes.
pods are ephemeral in nature, they cannot self heal self heal themselves.

## ReplicaSets
Replication controller is a part of kube-controller-manager, It makes sure the specified number of replicas for a pod is running at any
given point of time.
Replication controller only supports equality based selectors.
Replica set is more advanced then replication controller, It supports both type of selectors 

## Deployments
It is also part of kube-controller-manager, With Kubernetes Deployments, you “describe a desired state in a Deployment object, and the Deployment controller changes the actual state to the desired state. Kubernetes deployments manage stateless services running on your cluster. Their purpose is to keep a set of identical pods running and upgrade them in a controlled way – performing a rolling update by default

## Labels
labels are key-value pair that can be attached to any Kubernetes objects
Using Label Selectors we can select subset of objects
Equality Based Selectors: Filtering of objects based on label keys and values 
e.g. env==dev, we are selecting the objects where the env label is set to dev
Set Based Selectors: Filtering of objects  	
e.g with env in (dev,qa), we are selecting objects where the env label is set to dev or qa


In the following example, we have a Deployment which creates a ReplicaSet A. ReplicaSet A then creates 3 Pods. In each Pod, one of the containers uses the nginx:1.7.9 imageIn the following example, we have a Deployment which creates a ReplicaSet A. ReplicaSet A then creates 3 Pods. In each Pod, one of the containers uses the nginx:1.7.9 image

![d1] (d1.PNG)

Now, in the Deployment, we change the Pod's template and we update the image for the nginx container from nginx:1.7.9  to nginx:1.9.1. Ashave modified the Pod's template, a new ReplicaSet B gets created. This process is referred to as a Deployment rollout

![d2] (d2.PNG)

Once ReplicaSet B is ready, the Deployment starts pointing to it.

![d3] (d3.PNG)




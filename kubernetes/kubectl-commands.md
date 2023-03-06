# Kubectl commands

## Minikube

For test/local setup we can use minikube which has➖

1. Both master and worker processes running on one single node.
2. has docker runtime preinstalled
3. Creates a virtual box on your pc/ need a virtual box or hyper visor on your machine to run it
4. 1 node k8s cluster

## Kubectl

CLI tool for interacting with the cluster through API server running as one of the master processes on your local machine. We can use any UI tool or an API to interact with the API server as well but kubectl is the most powerful client tool among all. We will use kubectl cli to configure the minikube cluster and minikube cli to just start up/delete the cluster.

## Minikube cli commands

* **minikube start  —vm-driver=(name of hypervisor) -> starts minikube cluster**&#x20;
* **minikube status →** gives status of minikube cluster

## Kubectl commands

* **kubectl get nodes** → gets all nodes in the cluster, for minikube it will just show one

![image](https://user-images.githubusercontent.com/55504616/223106235-0999936e-de73-472a-a684-069e9001218b.png)

* **kubectl get pod** → gets pods running inside the node

![image](https://user-images.githubusercontent.com/55504616/223106739-362801de-cc3a-456f-b723-71bff4ac0236.png)

* **kubectl create deployment mongo-depl —image=mongo** → creates a deployment with name nginx-depl which will then in turn create a pod that runs a container image called nginx fetched from dockerhub

![image](https://user-images.githubusercontent.com/55504616/223107578-685a8659-ef29-4a32-9703-0f37898e77f3.png)

* **kubectl get deployment** → gets all deployments.

![image](https://user-images.githubusercontent.com/55504616/223108078-25479b0b-465c-4af4-b7e6-ee0a5e95a11b.png)

* **kubectl get service** → gets all services running

![image](https://user-images.githubusercontent.com/55504616/223129146-b41b173e-57d5-4322-a6e9-87d2251c189f.png)

* **kubectl get replicaset** → it gets the replica sets that were created by deployment. The pod name will be something like this: deployment.name-replicaset.id-pod.id

![image](https://user-images.githubusercontent.com/55504616/223108624-d01cb579-bfd7-48ae-aabd-fd40480777fe.png)

* **kubectl edit deployment (deployment name)** → opens the config file for deployment with which you can directly edit and save changes . Suppose you change image name then the old pod will terminate and a new pod get created with the new image running automatically. Even the old replicaset will now have no pods inside it and a new replicaset is created on its own. This is all done by Kubernetes on its own. We don’t have to manage the below working.

![image](https://user-images.githubusercontent.com/55504616/223108898-058ab4c3-6b2a-44b2-a307-eb729bda4443.png)

* **kubectl describe pod (pod name)** → gets info about the pod

![image](https://user-images.githubusercontent.com/55504616/223110303-6ab237ac-24f8-4912-a497-9e0513dbdb5b.png)

* **kubectl logs (pod name)** → gets logs of the activities happening inside the pod
* **kubectl exec -it (pod name) bash** → gets terminal for the pod container

![image](https://user-images.githubusercontent.com/55504616/223111230-7eed3120-3d76-4693-a859-26d05c9721d3.png)

* **kubectl delete deployment (deployment name)** → deletes it

![image](https://user-images.githubusercontent.com/55504616/223113146-424fa44d-f323-412d-af47-c876506e3f6e.png)

* **kubectl apply -f (file name)** → runs whatever is written inside the file. Like if we have config file for creating deployment then we can simply write all our options/configuration inside and then just run this command to apply that configuration.

![image](https://user-images.githubusercontent.com/55504616/223115912-4fdd7338-0cd2-4f77-ac49-7e89dd01a77e.png)
![image](https://user-images.githubusercontent.com/55504616/223115948-0b706808-98ba-4471-91ef-51e3824be47e.png)

* **kubectl delete -f (file name)**

![image](https://user-images.githubusercontent.com/55504616/223116284-8fe2c512-1f2f-44d2-b3ec-8cebbb33a11e.png)

* **kubectl describe service (service name)**
* **kubectl get pod -o wide** → gets more info on each pod

![image](https://user-images.githubusercontent.com/55504616/223118014-3aae7822-d2ec-48ec-8578-118b75d8a57e.png)

* **kubectl get deployment (deployment name) -o yaml** → gets current status of deployment from etcd and shows output as yaml file

![image](https://user-images.githubusercontent.com/55504616/223125847-757fc3ad-f5eb-4fd8-a3c1-630b709d39ec.png)

* **kubectl get all** -> gets all components inside the k8s cluster

![image](https://user-images.githubusercontent.com/55504616/223126076-ff70624e-ddc4-4649-8c01-4b0fce057de9.png)

* **kubectl get secret** -> gets all secrets&#x20;
* **kubectl create namespace (namespace name)** -> creates a namespace

![image](https://user-images.githubusercontent.com/55504616/223126340-ae46a408-b388-47c3-babe-4f29b8e2ad17.png)

* **kubectl apply -f (file name) --namespace=(namespace name)** -> creates component in the given namespace
* **kubectl get configmap** -> gets all configmaps&#x20;
* **kubectl get configmap -n (namespace name)** -> 'get' command by default checks in the default NS so to get configmap in a different namespace we have to tell the name of that namespace as well
* **kubectl get endpoints** -> get endpoints of all pods under a service

![image](https://user-images.githubusercontent.com/55504616/223128812-dc1f6f54-777b-43c9-9e3e-c4255041dc1c.png)

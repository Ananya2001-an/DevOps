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
* **kubectl get pod** → gets pods running inside the node
* **kubectl create deployment nginx-depl —image=nginx** → creates a deployment with name nginx-depl which will then in turn create a pod that runs a container image called nginx fetched from dockerhub
* **kubectl get deployment** → gets all deployments.
* **kubectl get service** → gets all services running
* **kubectl get replicaset** → it gets the replica sets that were created by deployment. The pod name will be something like this: deployment.name-replicaset.id-pod.id
* **kubectl edit deployment (deployment name)** → opens the config file for deployment which you can directly edit and save changes . Suppose you change image name then the old pod will terminate and a new pod get created with the new image running automatically. Even the old replicaset will now have no pods inside it and a new replicaset is created on its own. This is all done by Kubernetes on its own. We don’t have to manage the below working.
* **kubectl describe pod (pod name)** → gets info about the pod
* **kubectl logs (pod name)** → gets logs of the activities happening inside the pod
* **kubectl exec -it (pod name)** → gets terminal for the pod container
* **kubectl delete deployment (deployment name)** → deletes it
* **kubectl apply -f (file name)** → runs whatever is written inside the file. Like if we have config file for creating deployment then we can simply write all our options/configuration inside and then just run this command to apply that configuration.
* **kubectl delete -f (file name)**
* **kubectl describe service (service name)**
* **kubectl get pod -o wide** → gets more info on each pod
* **kubectl get deployment (deployment name) -o yaml** → gets current status of deployment from etcd and shows output as yaml file
* **kubectl get all** -> gets all components inside the k8s cluster
* **kubectl get secret** -> gets all secrets&#x20;
* **kubectl create namespace (namespace name)** -> creates a namespace
* **kubectl apply -f (file name) --namespace=(namespace name)** -> creates component in the given namespace
* **kubectl get configmap** -> gets all configmaps&#x20;
* **kubectl get configmap -n (namespace name)** -> 'get' command by default checks in the default NS so to get configmap in a different namespace we have to tell the name of that namespace as well
* **kubectl get endpoints** -> get endpoints of all pods under a service

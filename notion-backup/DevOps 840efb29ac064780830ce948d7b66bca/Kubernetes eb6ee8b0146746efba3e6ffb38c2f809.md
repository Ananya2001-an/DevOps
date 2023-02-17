# Kubernetes

## Points to discuss

1. What is Kubernetes
2. K8s architecture
3. Main K8s components
4. Minikube and Kubectl - Local setup
5. Main Kubectl commands - K8s CLI
6. K8s YAML configuration file
7. K8s Namespaces - organize your components
8. K8s Ingress
9. Helm - Package manager
10. Volumes - Persisting data in K8s
11. The StatefulSet - Deploying Stateful apps
12. K8s services

## Need of Kubernetes

It’s a **container orchestration tool**. Rise of monolith to microservices architecture led to rise in usage of containers but maintaining those thousands of containers was not easy so we wanted an orchestration tool like Kubernetes. It has 3 advantages➖

1. High availability and downtime
2. Scaling and high performance
3. Disaster recovery - backup 

## Kubernetes Components

### Node and Pods

A **node/worker node** is simply a server or a VM on which you app container would run. 

**Pod** is the smallest unit of K8s and is an abstraction over the container. It means that regardless of which container like Docker you are using to run your app we will only interact with the Kubernetes layer i.e. the at the Pod level.

### Service and Ingress

Now each Pod has an IP address with which it can communicate with the other Pods. But if somehow a Pod dies due to some app failure then in that case we have to replace it with another pod with a different ip which complicates things since we have to connect with that pod again and again using a  different ip. So to remove that issue we have a permanent ip address solution called as a **service**. Since lifecycle of a service has no connection with the pod the endpoints remain the same. 

Now to connect from the browser to this app running on the node we need an **external service** like this http://node-ip:node-port but this is not good for the end product so instead we want a service like this [https://domain-name](https://domain-name) which is easier to remember. So we use **Ingress** to make a request on this url and it then does port forwarding accordingly.

Also we don’t want to connect to a Pod that runs our database or something confidential on a public request hence for it we have an **internal service**.

### ConfigMap and Secret

Let’s say there’s an app running inside your Pod which has access to another Pod running your database using its service. Now if by any chance that service endpoint changes then we would have to make those changes for the url in the app itself and rebuild the image and again deploy it to the repository to finally run it. This is a tedious task since the url is generally available inside the build image only.

So to avoid this we have an external config inside the node which is called **ConfigMap** that has the service url you need and you can access it as an env variable or a properties file inside your pod. But this is not useful for storing passwords or any information that should be hidden from users since the ConfigMap has textual information not encoded in any form. Hence for that reason we have another config called **Secrets** which is base64 encoded and is used to store the secret information like passwords needed by your pod.

### Volumes

If database pod is restarted it will lose all its logged data and hence we need a way to persist it. So we use volumes which are either local meaning we have a replica of the pod data inside the K8s cluster on some hard drive or we have some remote storage option where the data is being stored outside the cluster maybe using some cloud service, etc. So basically K8s doesn’t manage your data instead you are the one responsible for handling it.

### Deployment and StatefulSet

## Important commands:

- kubectl get pod → gets all running pods in the cluster
- kubectl create deployment
- kubectl delete deployment
-
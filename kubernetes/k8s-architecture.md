# K8s architecture

In each **worker node** we observe the following➖

1. It can have multiple pods running on it.
2. It has 3 processes installed mandatorily:

* **Container runtime** - Like docker which is actually running our image inside the pod
* **Kubelet** - It’s a Kubernetes process that is responsible for handling interaction between the node and the container runtime. It’s the one that starts the pod with a container inside. Assigning resources like CPU, RAM from node to that pod running container inside.
* **Kube proxy -** it has an intelligent forwarding logic inside which makes sure that the requests that are being forwarded to the pods are using less network overhead like for example if a pod has to connect to another pod providing DB service we need to check if it’s available on the same node or not. This makes sure that the service is not forwarding requests blindly to whatever pod replica is available.

In each **control plane** (earlier called as the master node) **** we observe the following➖

1. It is the one with which we interact to connect to our cluster. We can use a client like Kubernetes dashboard, kubectl, etc., so that we can make queries.
2. It has 4 main processes:

* **Api server** - It is the one with which our client interacts and sends queries like creating pods, monitoring them and more. This behaves as the cluster gateway. Also handles gateway authentication. Since there is a single entry point to the cluster through means of master, it ensures security.
* **Scheduler - I**t basically handles the scheduling of pods in nodes depending on the resources that are currently being used inside the node. If a node has lesser resources compared to others it will be chosen. But the actual allotment is done by Kubelet. Scheduler just decides which one should be selected.
* **Controller manager -** This handles the restarting of pods. It will identify if there is any node with failing pods and then request the scheduler to decide which new node to run the pod on again. Finally kubelet executes the decision.
* **etcd -** It is the cluster brain. It has all the data regarding changes being made in the cluster like which pods failed, to which new node the pod was allotted, etc. Using etcd rest of the processes like controller, scheduler can work.

We can have more than 1 control plane in a cluster that are responsible for managing the worker nodes. Hence creating replicas of all processes in each node and having a distributed storage for etcd.

**Control plane has less resources whereas worker nodes has more resources since it does the actual work of running pods.**

<figure><img src="../.gitbook/assets/EC733F5E-409C-41AC-ACC8-82641A75C8E9.jpeg" alt=""><figcaption><p>cluster architecture</p></figcaption></figure>

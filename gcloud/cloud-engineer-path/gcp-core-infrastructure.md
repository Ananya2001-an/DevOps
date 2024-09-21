## Intro to GCP

**IaaS** offerings provide raw compute, storage, and network capabilities, organized virtually into resources that are similar to physical data centers.
> Compute Engine is an example of a Google Cloud IaaS service.

**PaaS** offerings, on the other hand, bind code to libraries that provide access to the infrastructure application needs. This allows more resources to be focused on application logic.
> App Engine is an example of a Google Cloud PaaS service.

> In the IaaS model, customers pay for the resources they allocate ahead of time; in the PaaS model, customers pay for the resources they actually use.

A **zone** is an area where Google Cloud resources get deployed. For example, let’s say you launch a virtual machine using Compute Engine–more about Compute Engine in a bit–it will run in the zone that you specify to ensure resource redundancy.

There are two types of quotas: rate quotas and allocation quotas. Both are applied at the project level.
1. Rate quotas reset after a specific time. For example, by default, the GKE service implements a quota of 3,000 calls to its API from each Google Cloud project every 100 seconds. After that 100 seconds, the limit is reset.

2. Allocation quotas govern the number of resources you can have in your projects. For example, by default, each Google Cloud project has a quota allowing it no more than 15 Virtual Private Cloud networks.


## Resources and access in cloud

![image](https://github.com/user-attachments/assets/cc622474-b18b-41aa-80c4-96da2823a860)

![image](https://github.com/user-attachments/assets/960aeedd-886c-4b39-8c48-1153405f5199)

Let’s say you have an application running in a virtual machine that needs to store data in Google Cloud Storage, but you don’t want just anyone on the Internet to have access to that data–just that particular virtual machine.

You can create a service account to authenticate that VM to Cloud Storage.

So, if a service account has been granted Compute Engine’s Instance Admin role, this would allow an application running in a VM with that service account to create, modify, and delete other VMs.

![image](https://github.com/user-attachments/assets/ac29b51e-e032-4fdd-9b73-20e3436ec483)

With a tool called Cloud Identity, organizations can define policies and manage their users and groups using the Google Admin console. 

![image](https://github.com/user-attachments/assets/7823980f-6214-4ea6-9d30-50b2c6e2ee36)


## VMs and Networks in cloud

A **virtual private cloud**, or **VPC**, is a secure, individual, private cloud-computing model hosted within a public cloud – like Google Cloud!
On a VPC, customers can run code, store data, host websites, and do anything else they could do in an ordinary private cloud, but this private cloud is hosted remotely by a public cloud provider. This means that VPCs combine the scalability and
convenience of public cloud computing with the data isolation of private cloud computing.

![image](https://github.com/user-attachments/assets/8b8b9797-a3b7-4fe9-b688-318ab3918974)

![image](https://github.com/user-attachments/assets/1f4c7587-bad9-4f7b-b482-d00a918e62fc)

![image](https://github.com/user-attachments/assets/995effeb-ddb2-4d26-b723-3a52cb1ce778)

You can use this system to accelerate content delivery in your application using Cloud CDN - Content Delivery Network. This means your customers will experience lower network latency, the origins of your content will experience reduced load, and you can
even save money.

![image](https://github.com/user-attachments/assets/fde7e8a4-7037-4863-b0c9-f4c451901e5f)

Many Google Cloud customers want to connect their Google Virtual Private Cloud networks to other networks in their system, such as on-premises networks or networks in other clouds. There are a number of effective ways to accomplish this.

![image](https://github.com/user-attachments/assets/6d474514-5480-4d7d-b17e-9eb4d713411a)

Google Cloud Virtual Private Cloud (VPC) provides networking functionality to Compute Engine virtual machine (VM) instances, Kubernetes Engine containers, and App Engine flexible environment. In other words, without a VPC network you cannot create VM instances, containers, or App Engine applications. Therefore, **each Google Cloud project has a default network** to get you started.

You can think of a VPC network as similar to a physical network, except that it is virtualized within Google Cloud. A VPC network is a global resource that consists of a list of regional virtual subnetworks (subnets) in data centers, all connected by a global wide area network (WAN). VPC networks are logically isolated from each other in Google Cloud.

Each Google Cloud project has a default network with subnets, routes, and firewall rules. The default network has a subnet in each Google Cloud region. Each subnet is associated with a Google Cloud region and a private RFC 1918 CIDR block for its internal IP addresses range and a gateway.

Each VPC network implements a distributed virtual firewall that you can configure. Firewall rules allow you to control which packets are allowed to travel to which destinations. Every VPC network has two implied firewall rules that block all incoming connections and allow all outgoing connections.

Notice that there are **4 Ingress firewall rules** for the default network:
- default-allow-icmp
- default-allow-rdp
- default-allow-ssh
- default-allow-internal

These firewall rules allow ICMP, RDP, and SSH ingress traffic from anywhere (0.0.0.0/0) and all TCP, UDP, and ICMP traffic within the network (10.128.0.0/9). The Targets, Filters, Protocols/ports, and Action columns explain these rules.

> Without a VPC network, there are no routes and no firewall rules!

> You cannot create a VM instance without a VPC network!

If you ever delete the default network, you can quickly re-create it by creating an auto mode network as you just did. After recreating the network, allow-internal changes to allow-custom firewall rule.

The External IP addresses for both VM instances are ephemeral. If an instance is stopped, any ephemeral external IP addresses assigned to the instance are released back into the general Compute Engine pool and become available for use by other projects.
When a stopped instance is started again, a new ephemeral external IP address is assigned to the instance. Alternatively, you can reserve a static external IP address, which assigns the address to your project indefinitely until you explicitly release it.

You can SSH because of the allow-ssh firewall rule, which allows incoming traffic from anywhere (0.0.0.0/0) for tcp:22.

> The 100% packet loss indicates that you cannot ping mynet-eu-vm's external IP. This is expected because you deleted the allow-icmp firewall rule!

> The 100% packet loss indicates that you cannot ping mynet-eu-vm's internal IP. This is expected because you deleted the allow-custom firewall rule!


## Storage in cloud

Object storage is a computer data storage architecture that manages data as “objects” and not as a file and folder hierarchy (file storage), or as chunks of a disk (block storage). These objects are stored in a packaged format which contains the binary form of the actual data itself, as well as relevant associated meta-data (such as date created, author, resource type, permissions, etc), and a globally unique identifier. 
These unique keys are in the form of URLs, which meansobject storage interacts well with web technologies. Data commonly stored as objects include video, pictures, and audio recordings.

![image](https://github.com/user-attachments/assets/13f1e62a-7b5f-40c5-8997-3734e8d38843)

![image](https://github.com/user-attachments/assets/f3304095-8a92-4e70-bb25-68523f6a6a9e)

![image](https://github.com/user-attachments/assets/63a5b8d9-3a7a-4c2d-b137-5109331c6240)

Cloud Storage also provides a feature called Autoclass, which automatically transitions objects to appropriate storage classes based on each object's access pattern.

![image](https://github.com/user-attachments/assets/72e8d4a5-9404-4396-b700-eb0c9a6e070a)

![image](https://github.com/user-attachments/assets/4de78ad5-7a57-4028-a490-af678a3c14f1)

![image](https://github.com/user-attachments/assets/2e4f0056-f5bf-4f12-9853-0efdbb2faf53)

![image](https://github.com/user-attachments/assets/680abc18-04e3-40ff-aefe-e5fe8c1fdff6)

![image](https://github.com/user-attachments/assets/cb65a6ac-90ee-45d8-a374-2f4df2a95588)


## Containers in the cloud

![image](https://github.com/user-attachments/assets/b3b76348-c8dc-42f6-9c81-3bf174af81d9)

![image](https://github.com/user-attachments/assets/62a52cee-cbf3-4eb1-95e1-c05371f70350)

![image](https://github.com/user-attachments/assets/a38f633f-0093-4d1f-bb57-dcd191098f35)

![image](https://github.com/user-attachments/assets/4f96480e-bc40-437b-ae5f-d923d98d8741)


## Applications in the cloud

**Cloud Run** is a managed compute platform that enables you to run stateless containers that are invocable via HTTP requests. Cloud Run is serverless: it abstracts away all infrastructure management, so you can focus on what matters most — building great applications.

![image](https://github.com/user-attachments/assets/8af9a95b-f3b8-4fb6-8b2f-8161c5ae47d7)

Cloud Run is built from Knative, letting you choose to run your containers either fully managed with Cloud Run, or in your Google Kubernetes Engine cluster with Cloud Run on GKE.

Cloud Run automatically and horizontally scales your container image to handle the received requests, then scales down when demand decreases. In your own environment, you only pay for the CPU, memory, and networking consumed during request handling.

The pricing model on Cloud Run is unique; as you only pay for the system resources you use while a container is handling web requests, with a granularity of 100ms, and when it is starting or shutting down.

![image](https://github.com/user-attachments/assets/b18d6419-aae3-4518-801d-4837d4844232)


## Prompt Engineering

![image](https://github.com/user-attachments/assets/6e7edb72-d32b-403d-8647-43b6dc00ad6b)

![image](https://github.com/user-attachments/assets/af0a778a-fa21-4c59-8205-c095fc4fc9f5)

But sometimes the LLM gives a completely wrong answer. This is called a hallucination. **Hallucinations** are words or phrases that are generated by the model that are often nonsensical or grammatically incorrect. This happens because LLMs can only understand the information they were trained on.

![image](https://github.com/user-attachments/assets/0d67eb8a-a1e9-430e-8409-cb2385fa2c28)

![image](https://github.com/user-attachments/assets/fbcf301b-a8e7-4ada-907f-226bffc6e0d9)


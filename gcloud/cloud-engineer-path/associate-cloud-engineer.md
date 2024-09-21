## Setting up a cloud solution environment

Example: Cymbal superstore

- Cymbal Superstore plans to migrate three applications to the cloud: its supply chain application, which will run on VMs; its containerized ecommerce application, which will use GKE; and its transportation management application, which will use Cloud Functions to track truck location.

![Screenshot 2024-09-17 022337](https://github.com/user-attachments/assets/880ba1bb-a3a8-4209-92ed-53d40aee561a)

- There are multiple projects under the B2B supply chain app folder, one for each environment related to continuous integration/continuous delivery: a development, staging, and production environment.

- Setting up the cloud environment also involves granting members **IAM** roles to ensure they have the right access to projects depending on their needs of their job and role at Cymbal Superstore.

![Screenshot 2024-09-17 022428](https://github.com/user-attachments/assets/8cbf985d-3bd4-4621-8660-498a5e557889)

- **Google Cloud Observability** provides metrics and logging services for all your services, resources, and projects in your cloud architecture.

![Screenshot 2024-09-17 022444](https://github.com/user-attachments/assets/3a093383-fed2-4711-a800-4b6d37025646)

![Screenshot 2024-09-17 022454](https://github.com/user-attachments/assets/52784d2a-f296-4a6c-a432-666bbe561427)



## Planning and configuring a cloud solution

![Screenshot 2024-09-17 025042](https://github.com/user-attachments/assets/654bbd92-8409-4cd3-b3a5-12083af84bfc)

1. Cymbal Superstore’s ecommerce application is currently implemented with containers, so you decide to use Google Kubernetes Engine (GKE) for compute.

    - For storage, you select Spanner to house Cymbal Superstore's ecommerce data because it allows for global availability and low latency.

    - Seeing as the ecommerce system is a web-based application, you will need an external http(s) load balancer. With GKE, you can do this by implementing an ingress object with a “gce” ingress class.

![Screenshot 2024-09-17 025104](https://github.com/user-attachments/assets/46405c13-74cb-4d66-8474-3f7e17df25d6)

2. Logistics’ transportation management system currently uses an on-premise message broker. Together with the cloud architect, you choose Pub/Sub as a solution to collect sensor data from trucks for analysis in the cloud.
    - You will use Cloud Functions to pull data from Pub/Sub and start a Dataflow pipeline.

    - Dataflow will feed data into Bigtable to store your sensor data.

    - You can run federated queries in BigQuery to visualize your data in Looker.
  
![Screenshot 2024-09-17 025247](https://github.com/user-attachments/assets/95d144ac-94f9-418b-9911-04ece012a242)

3. Cymbal Superstore’s existing supply chain application is implemented using virtual machines with a Linux operating system and a common Linux, Apache, MySql and PHP (LAMP) development stack.

    - After analyzing this application’s needs, the cloud architect determines that the recommended cloud solution is to migrate this application to Compute Engine virtual instances.

    - The data solution uses a Cloud SQL instance configured with a MySQL database.

    - The recommended network access would be external https access to the region for partners and internal connectivity between the application and the backing database service.

![Screenshot 2024-09-17 025357](https://github.com/user-attachments/assets/486e46dc-8d58-4f51-8bf0-4fe581e362a8)


### imp notes

![Screenshot 2024-09-17 131042](https://github.com/user-attachments/assets/9f4a9cce-b1e6-4306-8164-71214b198880)

![Screenshot 2024-09-17 131057](https://github.com/user-attachments/assets/0ed92421-4356-48b1-a29e-e174169ee422)

![Screenshot 2024-09-17 131224](https://github.com/user-attachments/assets/39fcb351-4ee9-45a7-9fd8-1f6c40577b76)

![Screenshot 2024-09-17 131247](https://github.com/user-attachments/assets/2c58659a-84f7-443d-8c9b-67c0911664ff)

![Screenshot 2024-09-17 131312](https://github.com/user-attachments/assets/cdad4eaf-f510-4a57-a7cc-7b431221094c)

![Screenshot 2024-09-17 131339](https://github.com/user-attachments/assets/7bf13f4a-39d3-424a-b6cf-730a5210dbc4)

![Screenshot 2024-09-17 131355](https://github.com/user-attachments/assets/257c3b78-f829-416d-a2f7-a8a223b80b10)


## Deploying and implementing a Cloud solution

![Screenshot 2024-09-17 132225](https://github.com/user-attachments/assets/74719513-e9e3-40ff-b456-524ed8f8227a)

![Screenshot 2024-09-17 161806](https://github.com/user-attachments/assets/004bfb6a-b17d-496c-a24d-8115345df5e2)

![Screenshot 2024-09-17 161700](https://github.com/user-attachments/assets/256ec7d3-f352-473b-bd19-6f8e180333aa)

![Screenshot 2024-09-17 161636](https://github.com/user-attachments/assets/1eacf169-9edd-4fa2-8d4b-c37d2bbd8db4)

![Screenshot 2024-09-17 161522](https://github.com/user-attachments/assets/3c297ec7-114d-4ccf-a6c7-e4cca39c82ee)

![Screenshot 2024-09-17 161446](https://github.com/user-attachments/assets/ed99016c-ec03-4f3f-a189-9cbd6c4a3392)


## Ensuring successful operation of cloud solution

To implement network load balancing you create a service object with these settings:
-  type: LoadBalancer.
-  Set External Traffic Policy to cluster or local

Cluster - traffic will be load balanced to any healthy GKE node and then kube-proxy
will send it to a node with the pod.

Local - nodes without the pod will be reported as unhealthy. Traffic will only be sent to
nodes with the pod. Traffic will be sent directly to pod with source ip header info
included.

To implement external http(s) load balancing create an ingress object with the
following settings:
- Routing depends on URL path, session affinity, and the balancing mode of
backend Network endpoint groups (NEGS)
- The object type is ingress.
- Using ingress.class: “gce” annotation in the metadata deploys an
external load balancer.
- External load balancer is deployed at Google Point


To implement an internal http(s) load balancer create an ingress object with the
following settings:
- Routing depends on URL path, session affinity, and balancing mode of the
backend NEGS.
- The object kind is ingress.
- Metadata requires an Ingress.class: “gce-internal” to spawn an
internal load balancer.
- Proxies are deployed in a proxy only subnet in a specific region in your VPC.
- Only NEGs are supported. Use the following annotation in your service
metadata:
- cloud.google.com/neg: '{"ingress": true}'
- Forwarding rule is assigned from the GKE node address range.

![Screenshot 2024-09-21 125150](https://github.com/user-attachments/assets/b658da87-23a7-488c-b35d-9b1782538418)


How does autoscaling work in Cloud Run?
Cloud Run automatically scales the number of container instances required for each
deployed revision. When no traffic is received, the deployment automatically scales to
zero.
Other ways you can affect Cloud Run autoscaling:
- CPU utilization, with a default 60% utilization.
- Concurrency settings, with a default of 80 concurrent requests. You can
increase it to 1000. You can also lower it if you need to.
- Max number of instances limits total number of instances. It can help you
control costs and limit connections to a backing service. Defaults to 1000.
Quota increase required if you want more.
- Min number of instances keeps a certain number of instances up. You will
incur cost even when no instances are handling requests

![Screenshot 2024-09-21 125423](https://github.com/user-attachments/assets/68f3487e-3a8e-405e-8ad7-f15a72f6211d)


## Configuring Access and security

- Service accounts are designed to provide permissions for machine-to machine or service-to-service communications in Google Cloud.

![Screenshot 2024-09-21 131042](https://github.com/user-attachments/assets/439031c9-2cc2-404c-908c-374cb6eab85b)
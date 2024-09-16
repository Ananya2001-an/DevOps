## Setting up a cloud solution environment

Example: Cymbal superstore

- Cymbal Superstore plans to migrate three applications to the cloud: its supply chain application, which will run on VMs; its containerized ecommerce application, which will use GKE; and its transportation management application, which will use Cloud Functions to track truck location.

- There are multiple projects under the B2B supply chain app folder, one for each environment related to continuous integration/continuous delivery: a development, staging, and production environment.

- Setting up the cloud environment also involves granting members **IAM** roles to ensure they have the right access to projects depending on their needs of their job and role at Cymbal Superstore.

- **Google Cloud Observability** provides metrics and logging services for all your services, resources, and projects in your cloud architecture.

![Screenshot 2024-09-17 022454](https://gist.github.com/user-attachments/assets/7aadbe2d-15d4-496d-83d7-f4af09261f70)

![Screenshot 2024-09-17 022444](https://gist.github.com/user-attachments/assets/82f62c73-3761-4261-b361-2bed9e096230)

![Screenshot 2024-09-17 022428](https://gist.github.com/user-attachments/assets/5c2fc54c-3789-4ff1-8fe0-cebf5bd245bc)

![Screenshot 2024-09-17 022337](https://gist.github.com/user-attachments/assets/3f3348bb-c99c-40f8-b7b8-896e02beb137)


## Planning and configuring a cloud solution

1. Cymbal Superstore’s ecommerce application is currently implemented with containers, so you decide to use Google Kubernetes Engine (GKE) for compute.

    - For storage, you select Spanner to house Cymbal Superstore's ecommerce data because it allows for global availability and low latency.

    - Seeing as the ecommerce system is a web-based application, you will need an external http(s) load balancer. With GKE, you can do this by implementing an ingress object with a “gce” ingress class.

2. Logistics’ transportation management system currently uses an on-premise message broker. Together with the cloud architect, you choose Pub/Sub as a solution to collect sensor data from trucks for analysis in the cloud.
    - You will use Cloud Functions to pull data from Pub/Sub and start a Dataflow pipeline.

    - Dataflow will feed data into Bigtable to store your sensor data.

    - You can run federated queries in BigQuery to visualize your data in Looker.

3. Cymbal Superstore’s existing supply chain application is implemented using virtual machines with a Linux operating system and a common Linux, Apache, MySql and PHP (LAMP) development stack.

    - After analyzing this application’s needs, the cloud architect determines that the recommended cloud solution is to migrate this application to Compute Engine virtual instances.

    - The data solution uses a Cloud SQL instance configured with a MySQL database.

    - The recommended network access would be external https access to the region for partners and internal connectivity between the application and the backing database service.

![Screenshot 2024-09-17 025357](https://gist.github.com/user-attachments/assets/1fae3aa1-41f7-4293-8406-b46afc74466a)

![Screenshot 2024-09-17 025247](https://gist.github.com/user-attachments/assets/20ab928e-f030-4a5b-a6df-e3db8837a6a9)

![Screenshot 2024-09-17 025104](https://gist.github.com/user-attachments/assets/93ac34b6-dbd3-40ea-81a1-9bd063ac70a6)

![Screenshot 2024-09-17 025042](https://gist.github.com/user-attachments/assets/2b38a7b2-403b-4978-bcb2-f3464f9cb485)
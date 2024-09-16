# Overview of GCP

<figure>
    <img src="https://github.com/user-attachments/assets/fc04eb51-d22a-4ea4-a9e2-ba4be84b7ad2"/>
    <figcaption>
        <p>GCP resources</p>
    </figcaption>
</figure>

1. **Global resources** can be accessed by any other resource, across regions and zones. Examples include disk images, disk snapshots, and networks.

2. **Regional resources** are redundantly deployed across multiple zones within a region. Regional resources can be accessed only by resources that are located in the same region. Examples include App Engine applications and regional managed instance groups.

3. **Multiregional services** are redundantly distributed within and across regions. These services optimize availability, performance, and resource efficiency.

4. **Zonal resources** can be accessed only by resources that are located in the same zone. An example of a zonal resource is a Compute Engine virtual machine (VM) instance.

For example, consider the following resources:

- Globally: a network that can be accessed by all resources.
- In each region: IP addresses that provide external access to resources only within a single region.
- In each zone: disks that can connect to VMs that are in the same zone.

## imp points to note

- Resources within a single project can work together easily, for example by communicating through an internal network, subject to the regions-and-zones rules. A project can't access another project's resources unless you use **Shared VPC** or **VPC Network Peering**.

- Shared VPC allows an organization to connect resources from multiple projects to a common **Virtual Private Cloud (VPC) network** so that they can communicate with each other securely and efficiently by using internal IP addresses from that network. When you use Shared VPC, you designate a project as a host project and attach one or more other service projects to it. The VPC networks in the host project are called Shared VPC networks. 

- Access gcloud using google console, cloud shell, API client libs, gcloud cli or IaC.

- Consider a zone as a single failure domain within a region. To deploy fault-tolerant applications with high availability and help protect against unexpected failures, you might deploy your applications across multiple zones in a region.

- The maintenance policy that you set for VMs is distinct from maintenance policies that you set for services that run on your VMs. For example, GKE deploys clusters on Compute Engine VMs. You can set maintenance policies to control when some GKE cluster maintenance happens, but those policies don't prevent automatic maintenance triggered on the underlying Compute Engine VMs.

- Google operates a global network of peering **points of presence (POPs)**, which means that customer traffic can travel within the Google network until it's close to its destination, providing users with a better experience and better security.

- **CUDs** provide discounted prices for eligible Google Cloud resources in exchange for purchasing committed use contracts (also known as commitments).


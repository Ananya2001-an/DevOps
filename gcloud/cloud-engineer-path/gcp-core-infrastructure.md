##



## VMs and Networks in cloud

Google Cloud **Virtual Private Cloud (VPC)** provides networking functionality to Compute Engine virtual machine (VM) instances, Kubernetes Engine containers, and App Engine flexible environment. In other words, without a VPC network you cannot create VM instances, containers, or App Engine applications. Therefore, **each Google Cloud project has a default network** to get you started.

You can think of a VPC network as similar to a physical network, except that it is virtualized within Google Cloud. A VPC network is a global resource that consists of a list of regional virtual subnetworks (subnets) in data centers, all connected by a global wide area network (WAN). VPC networks are logically isolated from each other in Google Cloud.

> Each Google Cloud project has a default network with subnets, routes, and firewall rules. The default network has a subnet in each Google Cloud region. Each subnet is associated with a Google Cloud region and a private RFC 1918 CIDR block for its internal IP addresses range and a gateway.

Each VPC network implements a distributed virtual firewall that you can configure. Firewall rules allow you to control which packets are allowed to travel to which destinations. Every VPC network has two implied firewall rules that block all incoming connections and allow all outgoing connections.

Notice that there are 4 Ingress firewall rules for the default network:
- default-allow-icmp
- default-allow-rdp
- default-allow-ssh
- default-allow-internal

> Note: These firewall rules allow ICMP, RDP, and SSH ingress traffic from anywhere (0.0.0.0/0) and all TCP, UDP, and ICMP traffic within the network (10.128.0.0/9). The Targets, Filters, Protocols/ports, and Action columns explain these rules.

> Note: Without a VPC network, there are no routes and no firewall rules!

> Note: As expected, you cannot create a VM instance without a VPC network!

> Note: If you ever delete the default network, you can quickly re-create it by creating an auto mode network as you just did. After recreating the network, allow-internal changes to allow-custom firewall rule.

> Note: The External IP addresses for both VM instances are ephemeral. If an instance is stopped, any ephemeral external IP addresses assigned to the instance are released back into the general Compute Engine pool and become available for use by other projects.
When a stopped instance is started again, a new ephemeral external IP address is assigned to the instance. Alternatively, you can reserve a static external IP address, which assigns the address to your project indefinitely until you explicitly release it.

> Note: You can SSH because of the allow-ssh firewall rule, which allows incoming traffic from anywhere (0.0.0.0/0) for tcp:22. The SSH connection works seamlessly because Compute Engine generates an SSH key for you and stores it in one of the following locations:
By default, Compute Engine adds the generated key to project or instance metadata.
If your account is configured to use OS Login, Compute Engine stores the generated key with your user account.
Alternatively, you can control access to Linux instances by creating SSH keys and editing public SSH key metadata.

> Note: The 100% packet loss indicates that you cannot ping mynet-eu-vm's external IP. This is expected because you deleted the allow-icmp firewall rule!

> Note: The 100% packet loss indicates that you cannot ping mynet-eu-vm's internal IP. This is expected because you deleted the allow-custom firewall rule!


# AWS VPC

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. Subnets are a range of IP addresses in your VPC

VPC with private and public subnets 

![image](https://user-images.githubusercontent.com/104793540/187453805-a0c70218-f378-477b-8675-bb1659d49754.png)

https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html - How Amazon VPC works
https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnets-commands-example.html - Create an IPv4-enabled VPC and subnets using the AWS CLI

Documentation on how create the following AWS resources:
- A VPC
- Two subnets
- An internet gateway
- A route table
- An EC2 instance

## Creating VPC
Follow the steps in this section to create a VPC. When you create a VPC, you have two options:
- VPC, subnets, and other VPC resources: Creates a VPC, subnets, NAT gateways, and VPC endpoints.
- VPC only: Creates only a VPC without any additional resources like subnets or NAT gateways within the VPC.

#### What is CIDR block - how to create 1 - how create CIDR blocks form the VPC CIDR block?
Classless Inter-Domain Routing (CIDR) notation is a way of representing an IP address and its network mask. The format of these addresses is as follows:

![image](https://user-images.githubusercontent.com/104793540/187456957-38970773-bfb3-42bd-83ca-8b911905a309.png)

- To create a VPC, subnets, and other VPC resources:
- open the Amazon VPC console at https://console.aws.amazon.com/vpc/
- In the navigation pane, choose Your VPCs, Create VPC.
- Under Resources to create, choose VPC and more.
- Modify the options as needed:
-   **IPv4 CIDR block:** Choose an IPv4 CIDR for the VPC. This option is required.
-   **IPv6 CIDR block:** Choose an IPv6 CIDR for the VPC.

##### Route tables
- A route table contains a set of rules, called routes, that are used to determine where network traffic from your VPC is directed. You can explicitly associate a subnet with a particular route table. Otherwise, the subnet is implicitly associated with the main route table.
- Each route in a route table specifies the range of IP addresses where you want the traffic to go (the destination) and the gateway, network interface, or connection through which to send the traffic (the target).

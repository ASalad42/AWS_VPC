# AWS VPC

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. Subnets are a range of IP addresses in your VPC

VPC with private and public subnets 

![image](https://user-images.githubusercontent.com/104793540/187453805-a0c70218-f378-477b-8675-bb1659d49754.png)

https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnets-commands-example.html

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
- To create a VPC, subnets, and other VPC resources:
- open the Amazon VPC console at https://console.aws.amazon.com/vpc/
- In the navigation pane, choose Your VPCs, Create VPC.
- Under Resources to create, choose VPC and more.
- Modify the options as needed:
-   **IPv4 CIDR block:** Choose an IPv4 CIDR for the VPC. This option is required.
-   **IPv6 CIDR block:** Choose an IPv6 CIDR for the VPC.

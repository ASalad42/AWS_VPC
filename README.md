# AWS VPC

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. Subnets are a range of IP addresses in your VPC

VPC with private and public subnets 

![image](https://user-images.githubusercontent.com/104793540/187638918-849ba0a6-101d-4f07-b10c-54960215ab40.png)

https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html - How Amazon VPC works
https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnets-commands-example.html - Create an IPv4-enabled VPC and subnets using the AWS CLI
https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html - Work with VPCs

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

The NAT gateway - managed by AWS. simply create the NAT gateway, specify what subnet it should reside in, and associate an Elastic IP address, and AWS will manage all other configuration.
- db subnet is private. So it has no access to the Internet gateway, and therefore the Internet, so how to download? A NAT gateway sits within the public subnet.because it sits within the public subnet, it has a route out to the Internet gateway, and to the Internet.
- NAT gateway takes request, goes via the Internet gateway, and download the appropriate software that's required, and send it back to the EC2 instance requesting it.


#### Classless Inter-Domain Routing (CIDR)
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

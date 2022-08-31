# Creating AWS VPC Infrastructure

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


NACL vs Security group 
- Security group is the firewall of EC2 Instances.
- Network ACL is the firewall of the VPC Subnets.
- Security groups are tied to an instance whereas Network ACLs are tied to the subnet.
- Network ACLs are applicable at the subnet level, so any instance in the subnet with an associated NACL will follow rules of NACL.
![image](https://user-images.githubusercontent.com/104793540/187655034-8d3d1a12-d97e-43b8-aaed-a211707b61ce.png)

stateless vs stateful 
- **Security groups are stateful**. This means any changes applied to an incoming rule will be automatically applied to the outgoing rule. e.g. If you allow an incoming port 80, the outgoing port 80 will be automatically opened.
- **Network ACLs are stateless**. This means any changes applied to an incoming rule will not be applied to the outgoing rule. e.g. If you allow an incoming port 80, you would also need to apply the rule for outgoing traffic.

- Occurrence: Subnet can have only one NACL, whereas Instance can have multiple Security groups.
- Rule Destination: **Security group rule** allow CIDR, IP, Security group as destination but **Network ACL rule** only allow CIDR as destination.

REF: https://medium.com/awesome-cloud/aws-difference-between-security-groups-and-network-acls-adc632ea29ae - AWS: Difference between Security Groups and Network Access Control List (NACL)

## Building Infrastructure

![image](https://user-images.githubusercontent.com/104793540/187665698-67e4fdbc-475d-4fe8-963c-e4c99659ea10.png)


### Step 1 Create a VPC with CIDR block 10.0.0.0/16
![image](https://user-images.githubusercontent.com/104793540/187661089-c6fadabf-660d-4186-aa47-b408ac45ed57.png)
![image](https://user-images.githubusercontent.com/104793540/187661385-6d3e8c71-3e79-410c-a3cc-dcf255ca1e2e.png)

### Step 2 Internet gateway  & 2.1 – add IG to your VPC - name

![image](https://user-images.githubusercontent.com/104793540/187661807-590ab766-1c49-4f68-8d87-c67f8393dbaf.png)
![image](https://user-images.githubusercontent.com/104793540/187661915-cf720a02-1847-4ebe-be87-bb5605477260.png)
![image](https://user-images.githubusercontent.com/104793540/187662130-ed314317-76af-49fb-ac1a-71d3fdfc00f7.png)
![image](https://user-images.githubusercontent.com/104793540/187662187-85737de6-ea8b-4499-8605-851f1273c5bf.png)
![image](https://user-images.githubusercontent.com/104793540/187662236-d85524cf-3b95-40dc-987f-efe1e42950bd.png)

### Step 3 Create public subnet (CIDR: 10.0.1.0/24)

![image](https://user-images.githubusercontent.com/104793540/187662447-45c74044-194a-4e59-b522-f15f569f3158.png)
![image](https://user-images.githubusercontent.com/104793540/187662924-4b525eb4-5b0b-4fe1-9d22-c1e627750090.png)
![image](https://user-images.githubusercontent.com/104793540/187663022-e4dc7e4b-86cc-4c07-9e54-76eba74dc470.png)

### Step 4 Create RT > edit rules to add SN
![image](https://user-images.githubusercontent.com/104793540/187663422-352e3350-bfe2-4e58-a74f-78d00034bc4f.png)
![image](https://user-images.githubusercontent.com/104793540/187663607-a7862d13-a6e3-434a-8f4f-f9ee45c8f8b7.png)
![image](https://user-images.githubusercontent.com/104793540/187663767-55d49808-dada-4411-b900-f80a31af7115.png)
![image](https://user-images.githubusercontent.com/104793540/187663863-809c474f-e1ab-4701-ae78-b8172c529d5c.png)
installing ig door into plot 
![image](https://user-images.githubusercontent.com/104793540/187664056-c367cc56-b193-4740-a22d-cc123be29fa4.png)
global and local 
![image](https://user-images.githubusercontent.com/104793540/187664171-e95fb000-8306-44c5-8932-58104e717a9f.png)

### Test by deploying app ami 

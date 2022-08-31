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

### Step 2 Internet gateway rule  

![image](https://user-images.githubusercontent.com/104793540/187661807-590ab766-1c49-4f68-8d87-c67f8393dbaf.png)
 2.1 – add IG to your VPC - name
![image](https://user-images.githubusercontent.com/104793540/187661915-cf720a02-1847-4ebe-be87-bb5605477260.png)
![image](https://user-images.githubusercontent.com/104793540/187662130-ed314317-76af-49fb-ac1a-71d3fdfc00f7.png)
![image](https://user-images.githubusercontent.com/104793540/187662187-85737de6-ea8b-4499-8605-851f1273c5bf.png)
![image](https://user-images.githubusercontent.com/104793540/187662236-d85524cf-3b95-40dc-987f-efe1e42950bd.png)

### Step 3.1 Create public subnet (CIDR: 10.0.1.0/24)

![image](https://user-images.githubusercontent.com/104793540/187662447-45c74044-194a-4e59-b522-f15f569f3158.png)
![image](https://user-images.githubusercontent.com/104793540/187662924-4b525eb4-5b0b-4fe1-9d22-c1e627750090.png)
![image](https://user-images.githubusercontent.com/104793540/187663022-e4dc7e4b-86cc-4c07-9e54-76eba74dc470.png)

#### app security group
![image](https://user-images.githubusercontent.com/104793540/187686814-e3ee7cbb-d715-45dc-8039-9d1339f084ed.png)
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html - Work with security groups

![image](https://user-images.githubusercontent.com/104793540/187691760-3bb18ec3-8165-47d8-ace2-6809bde45efb.png)


### Step 3.2 Create priavte subnet (CIDR: 10.0.1.0/24)

![image](https://user-images.githubusercontent.com/104793540/187682649-fdf87a18-8310-4b24-b09e-5181f057bc02.png)
![image](https://user-images.githubusercontent.com/104793540/187682688-e181cc52-fa9d-4295-b209-2f788fa4ec26.png)

#### DB security group

![image](https://user-images.githubusercontent.com/104793540/187719779-27fb1d9f-3ad4-4dac-be1e-30c851cb0e5f.png)

##### debugging "The source needs to be a CIDR block or a Security Group ID.":
- use cidr of your public subnet so any machine on public subnet can talk to db on port 27017
- ![image](https://user-images.githubusercontent.com/104793540/187728881-9c774573-b8a1-4963-bbdb-7d210aef5450.png)
![image](https://user-images.githubusercontent.com/104793540/187734891-865852f4-8bfc-4353-aa91-354e648c63c5.png)

### Step 4 Create Route Table (& edit rules to add SN)
#### Public 

![image](https://user-images.githubusercontent.com/104793540/187663422-352e3350-bfe2-4e58-a74f-78d00034bc4f.png)
![image](https://user-images.githubusercontent.com/104793540/187663607-a7862d13-a6e3-434a-8f4f-f9ee45c8f8b7.png)
![image](https://user-images.githubusercontent.com/104793540/187663767-55d49808-dada-4411-b900-f80a31af7115.png)
![image](https://user-images.githubusercontent.com/104793540/187663863-809c474f-e1ab-4701-ae78-b8172c529d5c.png)

installing ig door into plot 

![image](https://user-images.githubusercontent.com/104793540/187708214-cb4bf469-9067-47e6-af2e-d193e212f954.png)

global and local 
make sure ig route is 0.0.0.0/0
![image](https://user-images.githubusercontent.com/104793540/187707771-9f163f24-7814-40db-b5b4-10cf48c2802f.png)

launched app in public subnet:
- ami with npmservice 

![image](https://user-images.githubusercontent.com/104793540/187709974-374b65f4-1e75-4256-ad3b-14a163d9756f.png)
![image](https://user-images.githubusercontent.com/104793540/187713026-fb01017c-11ff-4e97-8e9e-300573095a95.png)
![image](https://user-images.githubusercontent.com/104793540/187712953-65ff6c39-69f3-42d2-ade4-0c4c9fc30db8.png)


#### Private 

![image](https://user-images.githubusercontent.com/104793540/187683242-28fb6117-be28-4f6d-8c02-071e45da3084.png)
![image](https://user-images.githubusercontent.com/104793540/187683317-34dedcdc-6ca3-433a-bd3b-953d2923d790.png)

associate only private subnet with this route table 

![image](https://user-images.githubusercontent.com/104793540/187683909-ee668cea-1520-481a-b94c-0d415f913cc3.png)
![image](https://user-images.githubusercontent.com/104793540/187683980-b753bb4a-d960-4337-af27-7f4ce42b5c44.png)

launch db in private subnet:
- use private ip of db ec2 in DB_HOST
- sudo nano .bashrc 
- export DB_HOST=mongodb://10.0.12.127:27017/posts
- printenv DB_HOST

debugging:
- ps aux 
- `sudo kill -9 2246`
- keep changing number at the end 
- ![image](https://user-images.githubusercontent.com/104793540/187732631-7c4ea0ff-a50c-4eec-a3ff-acc42b1e9a48.png)
- if persists try: 
- `sudo systemctl disable npm.service`
- `sudo systemctl kill npm.service`
- `sudo systemctl stop npm.service`
- `npm start`

without ssh into db you get:
![image](https://user-images.githubusercontent.com/104793540/187733267-004faebb-bd57-4d21-8cb8-cf7ce68cb272.png)


# VPCs

## What are VPCs ? 
- Stands for Virtual Private Cloud
![alt text](image.png)
- A vpc is an isolated network within a cloud provider eg- AWS.
- It allows you to manage cloud resources in an environment that you control. Resembles your own data center but within the cloud.

## Why Implement a VPC ? 
- **Increased security** — A VPC provides an isolated network environment. You can control inbound and outbound traffic using security groups and routing rules. This helps prevent unauthorised access and protects sensitive data.

- **High customisability** — You can design the network to fit your needs eg- choose your own IP address ranges, create public and private subnets, define custom route tables. 

- **Simpler than running your own private cloud**— Instead of building physical cloud infrastructure, a VPC lets you use managed networking within the public cloud. You get the benefits of a private network without the complexity of maintaining hardware.

## Diagram of the concept of a VPC
![alt text](image-1.png)

## Diagram of a VPC setup
![alt text](image-2.png)

## Creating a VPC:
1. Go to VPC section on AWS and click create VPC
![alt text](<Screenshot 2026-01-14 121931.png>)
2. Name VPC 
3. Input IPv4 CIDR 10.0.0.0/16
![alt text](image-4.png)
1. All other settings can be left the same and tags should have auto filled.
2. Now can create VPC  
![alt text](image-5.png)
![alt text](image-6.png)

## Now need to create subnet

1. Go to subnets section on AWS
![alt text](image-7.png)
2. Click Create Subnet
3. Select your VPC that you made
![alt text](image-8.png)
4. Add a public subnet with name public-subnet
5. Availability zone eu-west-1a
6. IPv4 subnet CIDR block 10.0.2.0/24
![alt text](image-9.png)
7. Add a new subnet to create a private subnet 
8. Name private-subnet
9. Availability Zone eu-west-1b
10. IPv4 subnet CIDR block 10.0.3.0/24
![alt text](image-10.png)
11. Click Create subnet

## Next need to create an Internet gateway

1. Go to Internet gateway section on AWS
![alt text](image-11.png)
2. Create Internet Gateway
3. Name it tech518-joshna-2tier-vpc-ig 
4. click create interent gateway
![alt text](image-12.png)

## Next need to attach Internet gateway to VPC 

1. can either click the attach to VPC message that shows up to attatch or click on actions then attach to VPC.
![alt text](image-13.png)
2. select your vpc and click attach interent gateway
![alt text](image-14.png)

## Next need to create route tables 

1. Go to route tables section on AWS 
![alt text](image-15.png)
2. click create route tables
3. name accordingly tech518-joshna-2tier-vpc-public-rt
4. select your VPC 
5. click create route table
![alt text](image-16.png)
6. Need to associate the route table with a subnet
7. go to the subnet associations section 
8. click edit subnet associations
![alt text](image-17.png)
9. select the public subnet and save associations
![alt text](image-18.png)
10. need to add a route to the internet gateway so go to the routes section and click edit routes.
![alt text](image-19.png)
11. Click add route to existing route 
12. destination 0.0.0.0/0
13. target Internet gateway 
14. select your internet gateway
15. click save changes
![alt text](image-20.png)
16. check how your resource maps look to see if its setup correctly
![alt text](image-21.png)

## to check everything is set up correctly launch a DB and app instance

1. go to EC2 on aws and click launch instance to launch a new db instance 
2. name your instance for you database 
3. select you database AMI
![alt text](image-22.png)
4. instance type is t3 micro 
5. choose your key pair 
6. and click edit in network settings as need to edit vpc and add security group
![alt text](image-23.png)
7. select your vpc 
8. and your private subnet as its for your database 
9. disable public ip 
10. create a new security group 
11. name security group 
12. leave the ssh inbound rule 
![alt text](image-24.png)
13. add a custom tcp security rule 
14. port range 27017
15. source is 10.0.2.0/24 - our public subnet ip address so it only has access to that.
![alt text](image-25.png)
16. can launch db instance
17. need to launch another instance for the app 
18. name instance tech518-joshna-app-in-vpc
19. select your app AMI
![alt text](image-26.png)
20. instance type t3 micro 
21. select your key pair 
22. edit network settings 
![alt text](image-27.png)
23. select your vpc 
24. select your public subnet 
25. enable public ip address 
26. name security group tech518-joshna-public-app-sg
27. create a new security group 
28. leave the ssh setting and a new security rule 
![alt text](image-28.png)
29. type custom tcp 
30. port 80
31. source 0.0.0.0/0
![alt text](image-29.png)
32. go down to user data in advanced details and add user data script to launch app.
33. make sure to replace db host ip address with the private ip address of your db
![alt text](image-30.png)
34. to check everything is working go on to the public ip address of the app and check you can see the database 
![alt text](image-31.png)



## Deletion order:
1. Delete running instances, first the app ec2 then the db instance.
2. Delete any security groups made for vpc 
3. Delete the VPC this will remove all components


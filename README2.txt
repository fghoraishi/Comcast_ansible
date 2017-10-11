Pre-Staging a Company VPC on AWS:

I created Virtual Private Cloud (VPC) for Company A using private IP CIDR 10.0.0.0/16
I then created three subnets:

- public network 1 - 10.0.1.0/24 with IGW and NATGateway in AZ east-1a
- Pulic network 2 - 10.0.3.0/24 in AZ east-1b

- Private Subnet - 10.0.2.0/24 with route to the internet via the NATGateway.

Webservers will be in public facing subnet and MySQL DB will be in the private subnet.

There are two Security Groups.

- Public SG - allows https/http and Mysql port 1433 from Private subnet, and SSH (only from my MAC IP) into the Public facing subnet
- Private SG - only allows ssh from Jump Server in the Public Subnet IPs and MySQL port 1433 from public subnet IPs. All other incoming networks are blocked.

Public Subnet users can ssh to Private network users via the jump server.


Ansible Controller setup on my MAC:

Ansible was installed on my Mac computer.

Created a remote access user on AWS for an IAM user called ÒfaadÓ.

Generated the keys for programable access:

Export AWS_ACCESS_KEY_ID
Export AWS_SECRET_ACCESS_KEY

Imported these keys into /etc/profile on my Mac so they will be available for use without sending it out via yml playbook.

I created two playbook in Ansible:

- createservers.yml - creates Instances in AWS in my VPC and assigned the right security group.
- configserver.yml - installs software and configures the system.

I find this solution of two playbooks will allow for a more robust solution. All modification to existing Instances via configserver.yml can be done without potential risk to modifying the infrastructure servers which were created earlier by createservers.yml. 

Additionally, all servers can be put in a Auto Scaling service.



Target Instances:

On target Instances to allow https using certs, I installed:

Yum Install http
yum Install mod_ssl 
Install index.html copied from my MAC home dir

Verified that Netstat -tupan show https listening port 80 and 443

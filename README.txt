Export keys on your system:

Export AWS_ACCESS_KEY_ID
Export AWS_SECRET_ACCESS_KEY

Imported these keys into /etc/profile on my Mac so they will be available for use without sending it out via yml playbook.

I created two playbook in Ansible:

- createservers.yml - creates Instances in AWS in my VPC and assigned the right security group.

- configserver.yml - installs software and configures the system.


Public IP of the instance needs to be copied into the hosts file manually prior to running the configserver.yml

Ports:
Only http and https ports on open on the webserver. 
When running configserver.yml, additional ssh port will be open temparary to do the config. This ssh access is limited to access from my Mac computer IP.

ansible-playbook -i hosts configserver.yml




# ConfigManager
Configuration Management

This repository contains Ansible playbooks to create an ec2 instance and to do the other required steps.

The createEC2Instance.yml playbook contains all the EC2 Instance details, and also creates one. In order to use it correctly, the machine running this Ansible playbook must have python 3, ansible and boto3 installed, which are required for ec2 instances management. Also, it's required to add an user on IAM at AWS with AmazonEC2FullAccess policy and get its Access Key ID and Secret Access key to put it into .boto file, which can be found on home/{your-user}/.boto (if it doesn't exist, just create one). Your .boto file should look like this:

.boto file

[Credentials]
aws_access_key_id = {created user aws access key id}
aws_secret_access_key = {created user aws secret access key}

After that, make sure that you have amazon.aws collection installed, in order to use createEC2Instance playbook. To check whether it is installed, run `ansible-galaxy collection list`. If it is not installed, run `ansible-galaxy collection install amazon.aws`. After that, it's important to make sure that you have your ansible hosts file configurated. To check you have your hosts file configurated, use `ansible all --list-hosts` or `ansible-inventory --list`. If there are no hosts configured, you might want to create or edit the hosts file, which is located on etc/ansible/hosts. For this first step, there's no need to have any hosts configured, but if you want to, you can have your hosts file looking like this:

hosts file
[local]
localhost

To run the createEC2Instance you use the following command : `ansible-playbook createEC2Instance.yml`

Now, for the following part, we have configmanager playbook, which is responsible for
- Copying the script nice-script.sh, that contains the command to list all mounted filesystems, to the remote machine into /etc/skel
- Creating a user with
    - Username: john
    - Home: /better-place/john
    - User ID: 1234
- Enables User john to run whoami command with sudo without providing his password
- Install packages Tmux, vim, unzip(used to install Terraform) and Terraform CLI

User john have no password, so in order to change to john user after connecting to your remote machine, just use su john and it's done.

To run the configmanager playbook just run `ansible-playbook configmanager.yml
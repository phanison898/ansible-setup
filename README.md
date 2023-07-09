### Working with Ansible

#### 1. Installation

```bash
sudo yum update -y
sudo amazon-linux-extras install epel -y
sudo amazon-linux-extras install ansible2 -y
```

#### 2. Copy hosts .pem file into ~/.ssh folder of ansible server

Inorder to make connection between ansible master server to host (slave) server, we need to establish ssh connection. For that we need to get the .pem file from aws and copy it into ansible server

```bash

# cd to ~/.ssh folder
cd ~/.ssh

#create a file called secret.pem in same directory and copy the content from your aws pem file into this secret.pem file
sudo vim private_key.pem

#Apply readonly permission to secret.pem file
sudo chmod 400 private_key.pem
```

#### 3. Add private_key.pem file to ssh-agent

```bash
ssh-agent bash
ssh-add ~/.ssh/private_key.pem
```

#### 4. Create inventory.ini file and add host ip address like below (add as per you hosts)

```ini
[webservers]
server1 ansible_host=192.168.0.1 ansible_user=ec2-user ansible_ssh_private_key=~/.ssh/private_key.pem

-------or-------

[webservers]
192.168.0.1

[webservers:vars]
ansible_user=ec2-user
ansible_ssh_private_key=~/.ssh/private_key.pem

[dbservers]
192.168.0.1
192.168.0.2
192.168.0.3

[dbservers:vars]
ansible_user=ec2-user
ansible_ssh_private_key=~/.ssh/private_key.pem
```

#### Test the configuration by running ping command

```yml
# ansible <hostname> -m ping
ansible -i inventory.ini -m ping dbservers
```

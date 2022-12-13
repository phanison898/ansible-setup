### Working with Ansible

#### 1. Installation

```bash
sudo yum update -y
sudo amazon-linux-extras install epel -y
sudo amazon-linux-extras install ansible2 -y
```

#### 2. Copy .pem file

Inorder to make connection between ansible master server to host (slave) server, we need to establish ssh connection. For that we need to get the .pem file from aws and copy it into ansible server

```bash
pwd                                       #which outputs /home/ec2-user

#create a file called secret.pem in same directory and copy the content from your aws pem file into this secret.pem file
sudo vim secret.pem

# Add readonly permission to secret.pem file
sudo chmod 400 secret.pem
```

### 3. Add hosts to ansible inventory file

```bash
# Go to below directory
cd /etc/ansible/

# Edit hosts file with vim
vim hosts
```

Add host ip address like below (add as per you hosts)

```ini
mail.example.com ansible_ssh_private_key=/home/ec2-user/secret.pem

[webservers]
foo.example.com
bar.example.com

[webservers:vars]
ansible_user=ec2-user
ansible_ssh_private_key=/home/ec2-user/secret.pem

[dbservers]
one.example.com
two.example.com
three.example.com

[dbservers:vars]
ansible_user=ec2-user
ansible_ssh_private_key=/home/ec2-user/secret.pem
```

#### Test the configuration by running below command

```yml
# ansible <hostname> -m ping
ansible dbservers -m ping
```

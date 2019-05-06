# HW2-DevOps

##### Homework 2

This homework is to configure a running mattermost server with Ansible.

Environment:

* VM

I initialize a vagrant VM by 

```
vagrant init
```

and change the Vagrantfile

```
# Every Vagrant development environment requires a box. You can search for
# boxes at https://vagrantcloud.com/search.
config.vm.box = "ubuntu/xenial64"

...

# Create a private network, which allows host-only access to the machine
# using a specific IP.
config.vm.network "private_network", ip: "192.168.33.200"
```

and then

```
vagrant up
```

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/vagrant-vm.png)


Inside vagrant VM, install python in order to let ansible to work in VM

```
sudo apt-get install python
```

Note:

Check ~/.ssh/known_hosts file on the MacBook Pro. If there exist an entry like below, just delete that entry. In order to protect privacy, I replace the hash value with *.

```
[127.0.0.1]:2222 ecdsa-sha2-nistp256 *************************************************************=
```

* Ansible

Ansible is installed on my MacBook Pro, which is used to configure the remote host (i.e., vagrant VM).

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/ansible-version.png)

Therefore, ansible playbooks are executed on my MacBook Pro.
In order to achieve good practice, I separate the scripts into two playbooks files (mysql.yml and mattermost.yml), and run each playbooks two file to check for being  be idempotent. In addition, I specify the VM into the hosts file.

Steps:

* Install MySQL.
 
 Run the playbook for the first time
 
 ```
 ansible-playbook -i hosts mysql.yml
 ```
 
![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mysql-first.png)

Check MySQL installation in vagrant VM

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mysql-start.png)
 
 Run the same playbook for the second time
 
![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mysql-second.png)

* Install and configure Mattermost

Run the playbook for the first time

```
ansible-playbook -i hosts mattermost.yml
```

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mattermost-first.png)

I have add some checks before creating the admin user, normal user and team. For this time, we create them through the playbook tasks.

After this, we can log into the system console

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mattermost-console.png)
 
In addition, we have made several configurations, e.g., smtp setting, file storage, etc., as required in the installation instructions.

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/smtp-setting.png)

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/email-notification.png)

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/storage-setting.png)
 
We log in the chat server with a demo:
 
![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/chat-server.png)

We run the same playbook the second time

![image](https://github.ncsu.edu/rshu/HW2-DevOps/blob/master/results/mattermost-second.png)

Since we have already admin user, normal user and team, we skip such tasks in the playbooks.

* Screencast

 [Google Drive Link to Screencast](https://drive.google.com/open?id=1wiIIUV-tKNbQ4TCS_LZ6FdrYlRPFU0mF)

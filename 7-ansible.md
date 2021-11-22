# SSH to EC2 via Ansible 
Simply connect to an exsisted EC2 with Ansible. Sees it as a connection test

Requirements:
- Virtualenv installed
- [EC2 Instance Created](https://github.com/alliehayashi/Tutorials/blob/master/0-create-aws-ec2.md)
  
Using: 
- macOS Mojave 10.14.6
- python 3.8.2
- zsh 5.8 (x86_64-apple-darwin18.7.0)
- ansible 4.8.0
- ansible-core 2.11.6
>ref: [How to Connect AWS to Ansible](https://networknuts.net/how-to-connect-aws-to-ansible/)
---
## 1. Setups
### Create a virtualenv 
```bash
$ virtualenv <ansible>    #ansible->virtualenv name
```
### Install Ansible
```
$ pip install ansible 
```
it'll take some time to download 🌝
### Package Versions
```markdown
$ pip list
  
Package      Version
------------ -------
ansible      4.8.0
ansible-core 2.11.6
cffi         1.15.0
cryptography 35.0.0
Jinja2       3.0.3
MarkupSafe   2.0.1
packaging    21.3
pip          21.3.1
pycparser    2.21
pyparsing    3.0.6
PyYAML       6.0
resolvelib   0.5.4
setuptools   58.3.0
wheel        0.37.0
```
---
## 2. Connect to EC2 with key
### Change the Key Access
Locate to your key file, and ensure your key is not publicly viewable.   
(The key will not be able to use if it's too open)
```markdown
$ chmod 400 <your_key.pem>
```
### Connect to EC2
```markdown
$ ssh -i <where_your_key_is>/<your_key>.pem ubuntu@<ip>
```
![4](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/04-ssh%20-i%20~:downloads:allie_ec2.pem%20ubuntu%4018.170.55.26.png)  


---
## 3. Connect to EC2 via Ansibe
After successfully ssh to EC2, we can now try to coonect via Ansible 🥳
### Ansible.cfg Settings
`ansible.cfg` is the confige file of Ansible
```bash
$ vim ansible.cfg  
  
--> insert   
[defaults]
inventory = hosts    #hosts could be change to other names
host_key_checking = False    
```
### Inventory Setting
`Inventory` is the place where we store the infos of remote servers  
As the command above, the inventory is named `hosts`.   
- `dev` : host name (name it yourself)
- `test` : server name (name it yourself)
- `ip` : EC2 ip (see on console)
- `ubuntu` : user name (see on sonsole)
```bash
$ vim hosts  

--> insert   
[dev] 
test ansible_ssh_host=<ip> ansible_user=ubuntu ansible_ssh_private_key_file=<where_your_key_is>/<your_key>.pem
```
![9](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/09-ec2-connect.png)

### Run Ansible Ping Module
```
$ ansible all -m ping
```
![10](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/10-ansible-all-m-ping.png)

### Print sth out
```
$ ansible all -m command -a 'echo Hello World on my host.'
```
![3](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/03-ansible%20all%20-m%20command%20-a%20'echo%20Hello%20World%20on%20my%20host.'.png)

### So know we can know that the connnections are all fine according to the testing, let's move to the next step! [PlayBook Setting]()
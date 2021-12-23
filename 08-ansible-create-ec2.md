# Create AWS EC2 with Ansible (create EC2)
Requirements:
- AWS CLI
- Virtualenv
- boto3

Using: 
- macOS Mojave 10.14.6
- python 3.8.2
- zsh 5.8 (x86_64-apple-darwin18.7.0)
- ansible 4.8.0
- ansible-core 2.11.6

>ref:  [ANSIBLE - CREATING AN EC2 INSTANCE & ADDING KEYS TO AUTHORIZED_KEYS](https://www.bogotobogo.com/DevOps/Ansible/Ansible-aws-creating-ec2-instance.php)

---

## 1. Activate Virtualenv
建立 `virtualenv`，以下動作都會在虛擬環境中進行。指令步驟可參考[此篇](https://github.com/alliehayashi/Tutorials/blob/master/3-virtualenv-setup.md)

---
## 2. 執行 Playbook
因為已經有建好 `security group`, `subnet`...等等了，所以會以既有的東西，以對照 console的方式進行

### ＃yml 語法
Playbook 是以 yml 寫成，語法有些比較特別的地方，以下列出一些較常見的：
-  `---` 是yml檔的開頭必配
-  變數呈現方式： `"{{ 變數 }}"`

先放上 `launch-ec2.yml` 完整程式碼：
```yml
---

-  hosts: localhost
   gather_facts: False
   vars:
     region: eu-west-2
     instance_type: t2.micro
     ami: ami-0fdf70ed5c34c5f52
     keypair: allie_ec2
   tasks:
     -  name: Create an ec2 instance
        ec2:
          key_name: "{{ keypair }}"
          instance_type: "{{ instance_type }}"
          image: "{{ ami }}"
          wait: yes
          region: "{{ region }}"
          count: 1
          instance_tags:
            Name: allie-instance-ansible
          vpc_subnet_id: subnet-0c8cdf5d876dda6d7
          # group --> security group
          group: allie-security-group
          assign_public_ip: yes
        register: ec2
```

```
$ ansible-playbook -vvv launch-ec2.yml
```
加上 `-vvv` 用於顯示執行過程，有助於看出目前執行的過程/找出出錯的部分。也可以只使用`-v` 或是完全不加，`v` 的多寡就是代表詳細的程度  
  
執行結果: 
![12](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/12-ansible-playbook.png)  

打開 console 就能看到創建好的 instance
![13](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/13-instance-created.png)

---
## (補充) Playbook 詳細內容
這裡會將 `launch-ec2.yml` 的設定跟 console 對照，拆解
### ＃hosts, gather facts, vars
```yml
-  hosts: localhost
   gather_facts: False
   vars:
     region: eu-west-2
     instance_type: t2.micro
     ami: ami-0fdf70ed5c34c5f52
     keypair: allie_ec2
```
- `gather_facts`: 預設是 True ，也就是一開始會先去收集 host 上的環境資訊，所以機器一多的話就會花很多時間，所以可以關掉
- `region`: 選擇要創建 instance 的地區
- `instance_type` : 選擇 free tier 的 t2.micro  
  
![14](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/14-choose-instance-type.png)
- `ami` : 不同 region 間 ami-id 就算是同一台機器，但還是會不一樣。所以要注意  
  
![16](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/16-ami-id.png)
- `keypair` : 從創建好中的 keypair 選擇  
  
![15](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ansible/15-key-pairs.png)
### ＃tasks
```yml
tasks:
     -  name: Create an ec2 instance
        ec2:
          key_name: "{{ keypair }}"
          instance_type: "{{ instance_type }}"
          image: "{{ ami }}"
          wait: yes
          region: "{{ region }}"
          count: 1
          instance_tags:
            Name: allie-instance-ansible
          vpc_subnet_id: subnet-xxxxxxxxxxxxxxxxx
          # group --> security group
          group: allie-security-group
          assign_public_ip: yes
          #instance_profile_name: GSBU-SVD
        register: ec2
```
- name : 這個 task 的名字
- ec2 
  - key_name : key pair 
  - image : ami ID 
  - wait : wait for the instance to reach its desired state before returning
  - count : 要 launch 的 `indstance 數量`
  - instance_tags : a hash/dictionary of tags to add to the new instance or for starting/stopping instance by tag。  
  底下的 name 就是 instance 的名字
  - vpc_subnet_id : `subnet 的 id`， 可以去 console 找
  - group : security group
  - assign_public_ip : when provisioning within vpc, assign a `public IP address`

>更多參數參考: [Ansible Document](https://docs.ansible.com/ansible/2.3/ec2_module.html)  
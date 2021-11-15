# Create AWS EC2

## 1. Login
登入 IAM 帳號後，switch 到 `Camelot_Dev` Role  
![1](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/1-aws-login.png)

找到 EC2  

![2](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/2-select-ec2.png)
  
這裡使用 `London(eu-east-2` region 做示範

---
## 2. Launch Instances
### 點擊 `Launch Instances`  
![3](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/3-click-launch-instances.png)  
## Step 1: Choose an Amazon Machine Image (AMI)

  

這裡選擇 `Ubuntu Server 16.04 LTS (HVM), SSD Volume Type`

![4](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/4-choose-ubuntu.png)
## Step 2: Choose an Instance Type 
選擇 free tier `t2.micro`  

![5](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/5-choose-t2.png)
## Step 3: Configure Instance Details
選擇 `VPC` 以及 `subnet`，這邊使用已建好的 public access subnet 設置  

![6](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/6-configure-details.png)
## 4. Step 4: Add Storage
skip  

![7](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/7-add-storage.png)

## 5. Step 5: Add Tags
skip  

![8](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/8-add-tags.png)

## Step 6: Configure Security Group
設定 `inbound/outbound traffic rules`. 這裡使用已設好之 `allow all traffic` 設定  
⚠️ 如果沒設好的話，會影響到 internet access 的部分  

![9](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/9-configure-secruity-group.png)

## Step 7: Review Instance Launch
確定是否設定都正確  

![10](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/10-review.png)
### 選擇 Launch 之後，設定 key pair
`key pair`: 用於連接 EC2 的密碼  

![11](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/11-create-key.png)
### 成功 Launch 
Initalize 可能會需要一點時間  

![12](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/12-launch.png)

---
## 3. ssh 到 EC2
在 instance 頁面找到 `connect`:  
 
![18](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/18-connect-page.png)
```
$ chomd 400 <keypair>
$ ssh -i"<keypair>ubuntu@<your_ip>
```
## 連線成功！
![16](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/16-connected.png)

⚠️ 如果沒有先 `$ chomd 400 <keypair>` 的話：  
  
![17](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ec2/17-too-open.png)



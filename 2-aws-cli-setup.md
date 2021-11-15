# AWS CLI Setups for Mac
This tutorial will guide you from `setting up CLI` to `how to assume roles with MFA verification`.  
  
Requirements:
- pip3
- AWS  
  - IAM account
  - access key
  - secret key
 
Using:  
* macOS Mojave 10.14.6
* zsh 5.8 (x86_64-apple-darwin18.7.0)
* AWS CLI V2


---
## 1. Download CLI
```bash
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```
跑完一長串會要求 `輸入pwd`    

installer: Package name is AWS Command Line Interface
installer: Installing at base path /
installer: The install was successful.

>ref: [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

---
## 2. Verify that the shell can find and run the aws command in your $PATH
```bash
$ which aws    #儲存位置
  
->  /usr/local/bin/aws
```
>

```
$ aws --version  
  
->  aws-cli/2.3.4 Python/3.8.8 Darwin/18.7.0 exe/x86_64 prompt/off  
```


---
## 3. Configure AWS Account

```
$ aws configure
```
照著提示輸入相對應資料：  
  
AWS Access Key ID [None]:  `your access key`  
AWS Secret Access Key [None]:   `your secret access key`  
Default region name [None]: `your_region`   
Default output format [None]: `json`

---
## 4. 檢查設定值
```bash
$ aws sts get-caller-identity    # (use :q to exit )  

{
    "UserId": "`your_user-id`",  
    "Account": "`your_account_id`",  
    "Arn": "arn:aws:iam::`your_iam_id`:  user/`your_account`"
}
```

```bash
$ ls -la    #檢查是否有成功安裝.aws  
  
-->  total 552  
...  
drwxr-xr-x    4 allielin  staff    128 Nov  5 17:56 .aws
```


```bash
$ cat config    #查看所有profile  
    
[default]  
region = eu-west-2  
output = json
```


```bash
$ cat credentials    #check keys  
   
[default]  
aws_access_key_id = `your_access_key`  
aws_secret_access_key = `your_secret_key`
```


>ref: [Command structure in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html)   
[Configuration basics - Quick configuration with aws configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
---
## 5. 安裝 Assume Role
```
$ brew install remind101/formulae/assume-role
```

>ref: [remind/assume-role](https://github.com/remind101/assume-role)

---
##  6. Add New Role
這邊示範的 new role 以 profile `test` 來演示，suit yourself with your own name
```bash
$ vim ~/.aws/config    #從vim編輯config
```
### 在 config 裡新建 profile   
``` 
[default]  
region = eu-west-2  
output = json

[profile <test>]  
region = eu-west-2  
output = json
```
### credentials 設定

```bash
$ vim ~/.aws/credentials    #從vim編輯credentials
```
  
- Please note that youu don't have to add 'profile' before `test` here.

```
[default]  
aws_access_key_id = <access_key>  
aws_secret_access_key = <secret key>
```
### MFA 認證及 switch role
- `mfa_serial`的設定適用登入時有設置 MFA 認證   
- 需要 assume role 時，需要先登入自己的IAM帳號（這裡放在 `default` 裡），再切換帳號。所以我們在 `test` 裡面加入一行 `source_profile  = default`填寫，就能吃到`default`的key了。
```
[<test>]  
role_arn = arn:aws:iam::<role_id>:role/<role>  
source_profile = <default>  
mfa_serial = arn:aws:iam::<iam_id>:mfa/<account>
```

---
## 7. Assume Role
``` bash
$ eval $(assume-role -duration 8h0m0s test)
``` 
`-duration` 是可以保持登入狀態的時間，這邊的話是8h0m0s，也就是8小時  
最後面的 `test` 則是前面創建的 profile  

執行後會要求輸入 `MFA code`
```bash
$ env    #查看現在的role
```
在最下面可以找到 `ASSUMED_ROLE`，裡面應該會是創建的 profile name。
  
ASSUMED_ROLE=`test`  
_=/usr/bin/env  

---
## 8. 更方便的設定 (for zsh)
If you use `$ eval $(assume-role)` frequently, you might want to create this...

```
$ vim ~/.zshrc    #open vim
```
將 `alias assume-role='function(){eval $(command assume-role $@);}'` 加到最底下
```
$ assume-role -duration 8h0m0s <test>
```
這樣以後只要打這行就能直接 assume role 囉 🥳
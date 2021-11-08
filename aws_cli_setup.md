# AWS CLI Setups for Mac
This tutorial will guide you from `setting up CLI` to `how to assume roles with MFA` verification. 
* Enviorment: MacOS
* using zsh
---
## Download CLI
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
## Verify that the shell can find and run the aws command in your $PATH
```bash
$ which aws
```
>
/usr/local/bin/aws
```
$ aws --version
```
aws-cli/2.3.4 Python/3.8.8 Darwin/18.7.0 exe/x86_64 prompt/off

---
## Configure AWS Account

```
$ aws configure
```
照著提示輸入相對應資料：  
AWS Access Key ID [None]:  `your access key`  
AWS Secret Access Key [None]:   `your secret access key`  
Default region name [None]: `your_region`   
Default output format [None]: `json`

---
## 檢查設定值
```bash
$ aws sts get-caller-identity    # (use :q to exit )
```
 
{
    "UserId": "`your_user-id`",  
    "Account": "`your_account_id`",  
    "Arn": "arn:aws:iam::`your_iam_id`:  user/`your_account`"
}

>ref: 

```bash
$ ls -la    #檢查是否有成功安裝.aws
```
total 552  
...  
drwxr-xr-x    4 allielin  staff    128 Nov  5 17:56 `.aws`

```bash
$ cat config    #查看所有profile
```
[default]  
region = eu-west-2  
output = json

```bash
$ cat credentials    #check keys
```
[default]  
aws_access_key_id = `your_access_key`  
aws_secret_access_key = `your_secret_key`

>ref: [Command structure in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html)   
[Configuration basics - Quick configuration with aws configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
---
## 安裝 Assume Role
```
$ brew install remind101/formulae/assume-role
```

>ref: [remind/assume-role](https://github.com/remind101/assume-role)

---
##  Add New Role
這邊示範的 new role 以 `test` 來演示，suit yourself with your own name
```bash
$ vim ~/.aws/config    #從vim編輯config
```
在vim裡面新建一個 `test` profile   
  
[default]  
region = eu-west-2  
output = json

[profile `test`]  
region = eu-west-2  
output = json
```bash
$ vim ~/.aws/credentials    #從vim編輯credentials
```
輸入assume role的資料，由於先登入`default`之後才能切換帳號，所以`source_profile`填寫`default`，就能吃到`default`的key了。  
另外，因為team有設置MFA認證所以會有 `mfa_serial`的設定  
Please notice that youu don't have to add 'profile' before `test` here.
  
[default]  
aws_access_key_id = `access_key`  
aws_secret_access_key = `secret key`    

[`test`]  
role_arn = arn:aws:iam::`role_id`:role/`role`  
source_profile = `default`  
mfa_serial = arn:aws:iam::`iam_id`:mfa/`account`

---
## Assume Role
``` bash
$ eval $(assume-role -duration 8h0m0s test)
``` 
`-duration` 是可以保持登入狀態的時間，這邊的話是8h0m0s，也就是8小時  
最後面的 `test` 就是前面創建的 profile  

執行後會要求輸入 `MFA code`
```bash
$ env    #查看現在的role
```
在最下面可以找到 `ASSUMED_ROLE`，裡面應該會是創建的 profile name。
  
ASSUMED_ROLE=`test`  
_=/usr/bin/env  

## 更方便的設定 (for zsh)
If you use eval $(assume-role) frequently, you may want to create a alias for it:
```bash
home$ ls -la    #顯示隱藏檔案在內的所有檔案
```
-rw-r--r--    1 allielin  staff   4435 Nov  5 16:44 `.zshrc`
```
$ vim ~/.zshrc    #open vim
```
將 `alias assume-role='function(){eval $(command assume-role $@);}'` 加入最底下
```
$ assume-role -duration 8h0m0s test
```
這樣以後只要打這行就能直接 assume role 囉
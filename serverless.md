# Serverless Setup
Mac
## Download CLI
```
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```
跑完一長串會要求 `輸入pwd`
>Output:  
installer: Package name is AWS Command Line Interface
installer: Installing at base path /
installer: The install was successful.

ref: [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

---
## verify that the shell can find and run the aws command in your $PATH
```
$ which aws
```
>Output: /usr/local/bin/aws
```
aws --version
```
>Output: aws-cli/2.3.4 Python/3.8.8 Darwin/18.7.0 exe/x86_64 prompt/off
---
## Configure AWS Account

```
$ aws configure
```
>Output:  
AWS Access Key ID [None]:  `your access key`  
AWS Secret Access Key [None]:   `your secret access key`  
Default region name [None]: `eu-west-2 `   
Default output format [None]: `json`

ref: [Configuration basics - Quick configuration with aws configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
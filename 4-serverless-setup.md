# Serverles Setup for Mac
If you are trying to deploy a Serverless Web with AWS Lambda, and needs to switch roles for access. Then this is for you.

Requirements:
- pip3
- Virtualenv installed
- AWS IAM account (access key + secret key)

Using: 
- macOS Mojave 10.14.6
- zsh 5.8 (x86_64-apple-darwin18.7.0)
- aws-cli (2.3.4)
- Serverless: 
  -  Framework Core: 2.65.0 
  - Plugin: 5.5.1 
  - SDK: 4.3.0 
  - Components: 3.17.2

  
>ref:  
[AWS Lambda + Serverless Framework + Python — A Step By Step Tutorial — Part 1 “Hello World”](https://faun.pub/aws-lambda-serverless-framework-python-part-1-a-step-by-step-hello-world-4182202aba4a)  
[Hello World Python Example](https://www.serverless.com/framework/docs/providers/aws/examples/hello-world/python)

---
## 1. Activate Virtualenv

**Haven't setup yet?** [follow steps here](https://github.com/alliehayashi/Tutorials/blob/master/virtualenv-setup.md)

---
## 2. Install and Configure Serverless Framework
### Install Serverless
```bash
$ sudo npm install -g serverless
```

### Initialize 
- 記得路徑要 cd 到要建立的 folder 底下
```bash
$ serverless create --template aws-python3 --name hello-world
```
建立成功後，應該會看到多出了 `.gitignore`, `handler.py`, `serverless.yml` 這幾個檔案

### Role/Region Setting (Additional)
- 如果不需要 switch role/ 換 region，可跳過此部分
```bash
$ vim serverless.yml    #從vim直接修改
```
找到 `provider`，進行更改
- 這邊將 `region` 改到 London(eu-west-2)，並且加入不同的 `role` 設定
- 請依自身狀況斟酌修改 
```yml
provider:
  name: aws  
  runtime: python3.8  
  lambdaHashingVersion: 20201221  
  region: <eu-west-2>
  role: arn:aws:iam::xxxxxxxxxxx:role/<Role_Name>>
```
---
## 3. Deploy Function

使用 profile deploy
- 適用：在 `.aws/config` 裡已設定profile，並將 access & secret key存入`.aws/credentials`
- remember to change the profile name as well

```
$ serverless deploy --aws-profile <test>  
```
![Image](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/1-serverless-deploy--aws-profile.png)
### Console 對照
 - S3 Bucket
![S3](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/9-buckets.png)  

 - CloudFormation Stacks
![stacks](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/10-stacks.png) 

 - Lambda Functions
![fun](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/13-lambda.png)

---
## 4. Add Event to Function

```bash
$ vim serverless.yml    #從vim直接修改
```

找到 `functions` 加入 `events` 區塊
- `path`這裡使用 hello 作為示範，之後的 url就會是 `.... /hello`。

```yml
functions:
  hello:
    handler: handler.hello
    events:
      - http:
        path: <hello>
        method: get
```

---
## 5. Deploy Lambda
  
```
$ serverless deploy -v  
```
- `-v` / `--verbose` = show all stack events during deployment
- 注意：如果權限需要 assume role 的話，記得下 `$ eval $(assume-role -duration 8h0m0s <profile>)` 做切換動作，並從 `$ env` 確認有切換成功
- [click here](https://github.com/alliehayashi/Tutorials/blob/master/aws_cli_setup.md) for assume role settings tutorial  
### 執行結果
![output](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/8-aws-deploy-v-with-url.png)

### 網頁畫面
![web_page](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/4-aws-webpage-output.png)

---
## 6. 其他操作
### 刪除 Function
```
$ serverless remove
```
因為資料其實是會先上傳到 S3，再由CloudFormation 進行 deploy。 所以必須先刪除 S3 裡的資料才能刪除 Cloud Formation 上面的 Stack
  
![remove](https://github.com/alliehayashi/Markdown_Pictures/raw/master/aws-setup/12-sls-remove.png)
### 更換Assume Role
```
$ unset AWS_ACCESS_KEY_ID
$ eval $(assume-role -duration 8h0m0s <profile>)
``` 
必須先清除 `AWS_ACCESS_KEY_ID`，才能進行 assume role 的動作，否則雖然看起來好像有切換成功，但實際上還是沒吃到新的設定噢

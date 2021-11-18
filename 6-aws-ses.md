# Configure SES via AWS CLI
Requirements:
- pip3
- Virtualenv installed
- AWS IAM account (access key + secret key)

Using: 
- macOS Mojave 10.14.6
- zsh 5.8 (x86_64-apple-darwin18.7.0)
- aws-cli (2.3.4)
>ref: [Amazon Simple Email Service [SES] Using CLI](https://www.howtoinmagento.com/2018/09/amazon-simple-email-service-ses-using.html)
---
## 1. Login 
First, log into the account that has permission to use SES service

--- 
## 2. Verify Source Email
### Verify Sender Email
```markdown
$ aws ses verify-email-identity --email-address <sender-email> --region <region>
```
### Verify Receiver Email
```markdown
$ aws ses verify-email-identity --email-address <reciever-email> --region <region>
```
### Verify
you will receive emails from AWS to verify the email address:  

![2](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/02-email-verify.png) 

### Check the Verified Email List
you can check either by `CLI command`: 
```
$ aws ses list-identities
```
![6](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/06-aws%20ses%20list-identities.png)
 
or on `Console` : 
  
![1](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/01-verified.png)

---
## 3. Send Email
```markdown
$ aws ses send-email --from <sender-email> --to <reciever-email> --subject "<subject>" --text "<content>" --region <region>
```
OR
```markdown
$ aws ses send-email \  
--from "<sender-email>" \  
--destination "ToAddresses=<reciever-email>" \  
--message "Subject={Data=from ses,Charset=utf8},Body={Text={Data=ses says hi,Charset=utf8},Html={Data=,Charset=utf8}}"
```
Output:  

![8](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/08-send.png)
### Email Quota
```markdown
$ aws ses get-send-quota --region <region>
```
![9](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/09-send-quota.png)

---
## 4. Error Messages
### # Email address is not verified
![7](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ses/07-not%20verified.png)
happens if your sender/reciever email ain't verified yet

❯❯❯ make sure both sender/reciever email **ARE VERIFIED**
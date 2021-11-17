# Encrpyt/Decrpyt files with KMS on Mac/Linux

>AWS Key Management Service (AWS KMS) is a service that combines secure, highly available hardware and software to provide a key management system scaled for the cloud. AWS KMS uses customer master keys (CMKs) to encrypt your Amazon S3 objects.  

Requirements:    
  - AWS IAM account (access key + secret key)  
  
Using: 
- macOS Mojave 10.14.6
- zsh 5.8 (x86_64-apple-darwin18.7.0)
- aws-encryption-sdk (3.1.0)
- aws-encryption-sdk-cli (4.1.0)
  
ref:
- [AWS CLI Command Line Refernace-Encrpy](https://docs.aws.amazon.com/cli/latest/reference/kms/encrypt.html)  
- [AWS CLI Command Line Refernace-Decrpyt](https://docs.aws.amazon.com/cli/latest/reference/kms/decrypt.html)

---
## 1. Enviornment Setup
### Create Virtualenv
first, [create a virtualenv](https://github.com/alliehayashi/Tutorials/blob/master/03-virtualenv-setup.md) `using-kms` 
```bash
$ virtualenv -p python3 <using-kms>
$ cd <using-kms>
$ . bin/activate   #activate virtualenv 
```
### Install AWS Encryption CLI
```
$ pip3 install aws-encryption-sdk-cli
```
---
## 2. Create KMS key
### Login IAM account before you start
### Generate key
```
$ aws kms create-key
```
```bash
$ aws kms create-key --description "<allie's key>"    #create key with description
```
the key will be generated automatically:
![4](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/04-aws%20kms%20create-key.png)
### Check the created keys

```markdown
$ aws kms list-keys
```
the key will be showing like this, it
s hard to identify which key you want to use (unlast you only have a pair of key)
![9](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/09-list-key.png)
### Give the key an alias
" alias `allie-kms-key` " is the alias; and input the key id we just generated 
```
$ aws kms create-alias --alias-name alias/<allie-kms-key> --target-key-id <key_id>
```
now we can use another command 
```
$ aws kms list-aliases
```
![6](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/06-aws%20kms%20list-aliases.png)
`KeyId` = `TargetKeyID`
### Console
![5](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/05-key-on-console.png)

---
## 3. Encrypt
let's create a `demo.txt` file to demonstrate how this works
```bash
$ echo "hi demo" >> demo.txt  #create 'demo.txt' and input "hi demo"
$ cat demo.txt  #print out the file

-> hi demo
```
Please note:
- `\` means we still have next line
- `key_id` is the key you've created in the first step
- --plaintext fileb:// `demo.txt` \ --> the file you want to encrpyt. Use the full path of file if the file is not under the folder you're operating 
- --decode > `demo-encrypt` --> the encryptd file name you want to name after
```bash
$ aws kms encrypt \
--key-id <key_id> \
--plaintext fileb://demo.txt \
--output text \
--query CiphertextBlob | base64 \
--decode > demo-encrypt
```
as you can see, we created a `demo-encrypt` file:
![7](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/07-ls-encrpyt.png)  
and the contenct inside the file is encrypted
![1-cat-enrwcrypt](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/01-cat-encrpyt.png)

---
## 4. Decrpyt
quite same as the encryption, but still got some differnece:
- --ciphertext-blob fileb://`demo-encrypt` \ --> the file you want to decrypt
- --decode > `demo-decrypt.txt` --> the decrypted file name
```
$ aws kms decrypt \
--key-id <key_id> \
--ciphertext-blob fileb://demo-encrypt \
--output text \
--query Plaintext | base64 \
--decode > demo-decrypt.txt
```  
here we created the `demo-decrypt.txt` file
![8](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/08-ls-decrpyt.png)  
and it's decrypted successfully  
![2-cat-decrypt](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/02-cat-decrpyt.png)

---
## 5. Upload to S3
We can simplying drag the encrypted file via `console` or use the `CLI command` :
```bash
$ aws s3 ls    #list the s3 buckets  

->  2021-09-30 16:36:44 allie-bucket-test
```
`demo-encrypt` : the file to upload  
s3:// `allie-bucket-test` : bucket name
```
$ aws s3 cp demo-encrypt s3://allie-bucket-test  
  
->  upload: ./demo-encrypt to s3://allie-bucket-test/demo-encrypt
```
![11](https://github.com/alliehayashi/Markdown_Pictures/raw/master/en-decrypt/11-upload-to-s3.png)



# Deploy Flask Web via AWS
這邊預設已建立好EC2 Instance, 並已下載key

環境：Ubuntu 16.04

---

## cd到儲存key的路徑
範例是儲存在 `downloads`
```
$ cd downloads
```
## ssh到EC2
EC2 > Instance > Connect 
```markdown
$ ssh -i "`your_key`"ubuntu@`your_ip`
```
>Output:
The authenticity of host '`your_ip` (`your_ip`)' can't be established.
ECDSA key fingerprint is SHA256:sjPgJy3Ho4UDejjVGfTh9/jYOUVmf25OOptJdCI0994.
Are you sure you want to continue connecting (yes/no)? 
  `yes`

>Output: Warning: Permanently added '`your_ip`' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-1128-aws x86_64)... (略)
---
## Update Packages
接下來就會進到`ubuntu@your_ip:~$`的command line
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
```
## Install pip
```
$ sudo apt-get install -y python3-pip
```
查看是否安裝成功 `$ pip3 --version` 
>Output:
pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)
---
## Install venv
預設環境是python3.5
```
$ sudo apt-get install python3.5-venv
```
## Create venv 
這邊使用`Flask_Web`作為名稱，可替換成想要的名字
```
$ python3 -m venv Flask_Web
$ cd Flask_Web                ＃cd到剛剛創建的venv底下
$ source bin/activate         ＃啟用venv
```
啟用成功後command line就會變成：`(Flask_Web) ubuntu@ip-10-0-0-237:~/Flask_Web$ `

---
## Install Flask
```
$ sudo apt-get install python3-flask
```
---
## 使用vim創建檔案
```
$ sudo vim app.py
```
打開vim後使用`i` 開始編輯; `esc`結束編輯; `:x`退出vim

程式碼如下:
```Python
from flask import Flask
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)
```
## 執行
```
$ sudo python3 app.py
```
>Output: sudo: unable to resolve host ip-10-0-0-237
Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
60.248.185.20 - - [04/Nov/2021 07:41:07] "GET / HTTP/1.1" 200 -
60.248.185.20 - - [04/Nov/2021 07:41:08] "GET /favicon.ico HTTP/1.1" 404 -
60.248.185.20 - - [04/Nov/2021 07:42:43] "GET / HTTP/1.1" 200 -
## 大功告成！連線到你的ip去看看有沒有成功吧 (´ ｡✪ω✪｡｀) 
Credits: Ada Chen
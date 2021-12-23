# Deploy Flask Web via AWS
  
Requirements:
- Ubuntu 16.04 (AWS EC2 Instance)
- [EC2](https://github.com/alliehayashi/Tutorials/blob/master/0-create-aws-ec2.md)  
  
Using：
- macOS Mojave 10.14.6
- zsh 5.8 (x86_64-apple-darwin18.7.0)

---

## 1. 連接到EC2
### cd到儲存key的路徑，範例是儲存在 `downloads`
```
$ cd downloads
```
### ssh到EC2
EC2 > Instance > Connect 
```markdown
$ ssh -i "<your_key>"ubuntu@<your_ip>
```  
---
## 2. Update Packages
接下來就會進到 `ubuntu@<your_ip>:~$` 的command line
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
```
![update](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/1-update.png)

![upgrade](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/2-upgrade.png)
---
## 3. Install pip
```
$ sudo apt-get install -y python3-pip
```
查看是否安裝成功 
```
$ pip3 --version

->  pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)
``` 

---
## 4. 建置虛擬環境(Venv)
### Install venv (預設環境是 python3.5)
```
$ sudo apt-get install python3.5-venv
```

![venv](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/3-install-venv.png)
### Create venv 
這邊使用 `Flask_Web` 作為名稱，可替換成想要的名字
```
$ python3 -m venv Flask_Web
$ cd Flask_Web                ＃cd到剛剛創建的venv底下
$ source bin/activate         ＃啟用venv
```
啟用成功後command line就會變成：`(Flask_Web) ubuntu@ip-10-0-0-237:~/Flask_Web$ `
![activate](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/4-activate.png)
---
## 5. Install Flask
```
$ sudo apt-get install python3-flask
```
---
## 6. 執行
### 使用vim創建檔案
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
### run app.py


```
$ sudo python3 app.py
```
![run](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/5-running.png)

---
## 大功告成！連線到你的ip去看看有沒有成功吧 (´｡✪ω✪｡｀) 
![hello](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/6-hello.png)
## 其他指令
```
Ctrl+C    # 終止running

$ exit    # 中斷EC2 connection
```

![exit](https://github.com/alliehayashi/Markdown_Pictures/raw/master/ubuntu-flask/7-exit.png)  
*Credits: [Ada Chen](https://github.com/Ada-Chen2531)*
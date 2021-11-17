# Setup & Create Virtualenv on Mac 
Create `python3.7.7` venv with `pyenv`, `pyenv-virtualenv` and `virtualenv` on Mac with `python3.8.2`  
  
Using:
* pip3 (pip 21.3.1)
* Homebrew 3.2.14-9-g292c1fd
* macOS Mojave 10.14.6
* zsh 5.8 (x86_64-apple-darwin18.7.0)
* virtualenv 20.10.0p
* pyenv 2.0.1
  
>ref: [【Python教學】使用 pyenv 和 virtualenv 打造 Python 環境](https://www.maxlist.xyz/2020/04/01/python-pyenv-virtualenv/)
---
## 1. Setup
### 安裝 pyenv, pyenv-virtualenv, virtualenv
```bash
$ brew install pyenv
$ brew install pyenv-virtualenv
$ pip3 install virtualenv
```
### 設定pyenv環境變數
```bash
$ vim ~/.zshrc
```
打開 vim，加入以下設定：  
  
`export PATH=“$HOME/.pyenv/bin:$PATH”`  
`eval “$(pyenv init -)”`

---
## 2. Install Python Version
```
$ pyenv install -l    #查看可安裝的版本
```
### Install the python version you want
- 這邊以`3.7.7`作為範例，版本可自行替換
```bash
$ pyenv install -v 3.7.7
```
### Version you've installed
```bash
$ pyenv versions  

->  * system (set by /Usersallielin/.pyenv/version)  
->  3.7.7
```
---
## 3. Create your Virtualenv 🎉
### 首先 cd 到想要創建的file底下
- `3.7.7` 是想建立的python版本，最後的`test`則是 venv 名稱，可自行更換
```
$ virtualenv -p ~/.pyenv/versions/3.7.7/bin/python <test>   
```
### 使用環境預設的python版本，則是：
```
$ virtualenv <test>
```

### 啟動環境
- 成功建立後可以看到資料夾裡多出了一個 `test` 資料夾。
- 記得要把 `test` 改成上一步中新建的 venv 名稱
```bash
$ source <test>/bin/activate
```
```
$ . bin/activate    #另一種啟用指令
```
### 查看版本
- cd 到 `test` 資料夾裡
```bash
$ python3 --version    #版本  
  
->  Python 3.7.7
```
```bash
$ which python3    #位置  
  
->  /Users/allielin/Desktop/aws/test/bin/python3
```
```bash
$ pip3 list    #已安裝套件  

Package    Version
---------- -------
pip        21.3.1
setuptools 58.3.0
wheel      0.37.0
```
---
## 4. 其他操作
### 關閉venv
```bash
$ deactivate
```
### 刪除venv
- 一樣將`test`替換成你的 venv
```bash
$ rm -rf <test>
```
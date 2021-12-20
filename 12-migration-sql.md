# SQL setup & Migration (Mac)


---
## 1. Setups 
>ref: [How to install MySQL on macOS](https://flaviocopes.com/mysql-how-to-install/)
### # Install 
```
brew install mysql
```
### # Start Running
```
brew services start mysql
```
### # Secure Settings (password)
```
mysql_secure_installation
```

### # Stop Running
```
brew services stop mysql
```

---
## 2. Edits
### # code 
`<project-path>/camelot-backend/tools/camelot-env.sh`  
![6]
```
source <project-path>/camelot-backend/tools/camelot-env.sh
```
### # Activate Venv
```
source <project-path>/camelot-backend/venv/bin/activate
```
在 deploy 的 `install-python-venv` 時建立的，需要的套件都在裡面了   
![3]

---
## 3.  MySQL settings
### # Login to MySQL
```
mysql -u root -p
```
![7]

### # Create DB
```
CREATE DATABASE camelot4zyxel;
```
沒有新建 camelot4zyxel DB 的話會出現以下錯誤:  
![5]

---
## 4. Generate Migration
```
cd <project-path>/camelot-backend/src/persistent/migrations
```
```
alembic -x env=dev -x region=us-east-1 upgrade head
```
![4]

---
## 5. GUI
這邊使用的是 `MySQLWorkbranch`  
### # Login
![2]
打開後就可以看到匯入的資料庫
![8]



[2]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/2-gui.png
[3]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/3-venv.png
[4]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/4-run.png
[5]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/5-error.png
[6]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/6-code-after.png
[7]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/7-login-sql.png
[8]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/8-tables.png
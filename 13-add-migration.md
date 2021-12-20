# Add Migration
### Prerequisite: 
- [SQL setup & Migration (Mac)](https://github.com/alliehayashi/Tutorials/blob/master/12-migration-sql.md)  
- ❗️不要連上 VPN❗️
>Document: camelot-backend/src/persistent/migrations/ReadMe.md
---
## Setups
```
source <project-path>/camelot-backend/tools/camelot-env.sh
```
make sure python path is at the correct root
### #加入column
- 修改 camelot-backend/src/persistent/ `models.py`
- 這裡修改的範例是 Class `User`，加了一個叫做 `test` 的 column
![10]
```
alembic revision -m "migration_version_file_name"
```
- 根據所做更動去命名 migration_version_file_name 
- 此步驟會產生一個名為 `<version_hash>`_`<migration_version_file_name>`.py 的檔案
![9]
- 到 `camelot-backend/src/persistent/migrations/alembic/versions` 找到上一步產出的 .py檔  
![11]  
- 撰寫程式 (這裡寫的是簡單的 upgrade/downgrade的 alembic 語法)
![12]

---
## upgrade 
```
alembic -x env=dev -x region=us-east-1 upgrade head
``` 
![15]
從 MySQLWOrkBench 可以看到，資料表增加了一個 test 的 column
![13]

---
## downgrade 
回到 camelot-backend/src/persistent/ `models.py` 把新建的欄位刪掉
![17]
```
alembic -x env=dev -x region=us-east-1 downgrade -1
``` 
![16]
可以看到 test 的欄位被消失了
![14]



[9]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/09-add.png
[10]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/10-add-test.png
[11]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/11-find-version.png
[12]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/12-upgrade-downgrade.png
[13]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/13-added.png
[14]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/14-dropped.png
[15]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/15-upgrade.png
[16]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/16-downgrade.png
[17]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/migration/17-delete-column.png

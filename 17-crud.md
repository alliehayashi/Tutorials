# 常用的 git 指令

# CRUD practice
接續 migration，練習資料庫CRUD  
>目標：創建一個 table `test`，進行資料的增刪改查

---
## 1. 環境設置
### ＃連上 vpn
>[vpn settings](https://54.238.224.107/projects/zyxel-camelot/wiki/Deploy_Camelot_Frontend_(Mac)#4-VPN-%E9%80%A3%E7%B7%9A)

🔺 下一步做完拿檔 data key 就關掉 vpn，才不會更動到遠端料庫
### ＃export 環境變數
```
source <project-path>/camelot-backend/tools/camelot-env.sh
```
![3]  
🔺 如果切換到別的 work file 後沒再 source 的話，會發生讀取不到的問題

---
## 2. Migration - Add Table

>[add migration 參考](https://github.com/alliehayashi/Tutorials/blob/master/13-add-migration.md#add-migration)
```
cd <project_path>/camelot-backend/src/persistent/migrations
```
- models.py 儲存了 db 的結構
- add Class `test` in models.py, note that primary key is required
### `camelot-backend/src/persistent/models.py`
```python
class test(Base):
    __tablename__ = 'test'
    id = Column(String(255), primary_key=True)
    value = Column(String(255))
```
- 建立新 version
```bash
alembic revision -m "test_crud" #依照執行動作更換 "____"內容 
```
![2]
`camelot-backend/src/persistent/migrations/alembic/versions/`裡面可以找到 `862f5dcfa30d_test_crud.py` 這個檔案進行更改
```python
from alembic import op
import sqlalchemy as sa

# revision identifiers, used by Alembic.
revision = '862f5dcfa30d'    #這個版本
down_revision = '6d4a5fba9c76'    #下一個版本
branch_labels = None
depends_on = None

#加上去的 code
def upgrade():
    op.create_table('test',
        sa.Column('id', sa.String(length=16), nullable=False, primary_key=True),
        sa.Column('value', sa.String(length=16), nullable=False),
    )
    print('table created')

def downgrade():
    op.drop_table('test')
    print('table dropped')
```

- 改完之後就可以 upgrade 了
```
alembic -x env=dev -x region=us-east-1 upgrade head
```
![1] 
- 可以去資料庫找剛剛新建的 table `test`
![4]

---
## 3. CRUD
- 負責與資料庫溝通的是 `dal` 開頭的 python 檔案，路徑是 `camelot-backend/src/data_access`
- 這邊因為練習完會刪掉，所以直接在 company 底下新建一個 `dal_test.py` 做練習
- @db_session 是 decorator 的用法，可以參考[這裏](https://medium.com/citycoddee/python%E9%80%B2%E9%9A%8E%E6%8A%80%E5%B7%A7-3-%E7%A5%9E%E5%A5%87%E5%8F%88%E7%BE%8E%E5%A5%BD%E7%9A%84-decorator-%E5%97%B7%E5%97%9A-6559edc87bc0) 
- [Python定义的函数（或调用）中参数*args 和**kwargs的用法](https://blog.csdn.net/u010852680/article/details/77848570)

### `camelot-backend/src/persistent/models.py`
```python
def db_session(func):
    @wraps(func)
    def warp(*args, **kwargs):
        # new session
        session = Session()  # (this is now a scoped session)
        try:
            ret = func(*args, **kwargs)
            session.commit()
        except (Exception,):
            session.rollback()
            LOGGER.exception("database rollback")
            raise
        finally:
            Session.remove()  # NOTE: *remove* rather than *close* here
        return ret

    return warp
```
- 其中，這裡就是會執行使用 @db_session decorator 的指令，並且在跑完 function 後的下一行直接協助 commit 
```python
try:
            ret = func(*args, **kwargs)    #run function
            session.commit()    #commit 結果
```

### `camelot-backend/src/data_access/company/dal_test.py`

```python
from sqlalchemy import delete, insert, update
from src.persistent.models import Session, db_session, test

class crud:
    @db_session
    def create(session):    
        # query conditions
        insert_data = session.query(test)
        insert_data = test(id = 1, value = 123) # insert
        session.add(insert_data)

    @db_session
    def update(session):
        # update conditions
        update = session.query(test).filter(test.id == 1).update({"value":"456"})
    
        
    @db_session
    def delete(session):
        # delete conditions
        delete = session.query(test).filter(test.id == 1).delete()

    @db_session
    def read(session):
        
        # read 
        # query 出來的型態是 object --> <class 'src.persistent.models.test'>
        # 所以是無法直接 print 出來的
        for data in session.query(test):
            print(data.id, data.value)
        
# [run]        
try:     
    session = Session()
    
    crud.create(session)
    # crud.update(session)
    # crud.delete(session)
    # crud.read(session)  
    
    print('successed')
    
except Exception as e:
    print(e)   
```
---
## 4. Drop Table
```
alembic -x env=dev -x region=us-east-1 downgrade -1
```
成功的從原本的 `862f5dcfa30d` 降回前一版了
![5]

[1]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/crud/01-table-created.png
[2]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/crud/02-create-version.png
[3]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/crud/03-source-env.png
[4]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/crud/04-test-table.png
[5]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/crud/05-drop-table.png
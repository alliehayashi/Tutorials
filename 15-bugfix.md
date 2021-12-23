# Bugfix Proccess
### ▶️ 以 [bugFix #35354](https://redmine.marketplace.zyxel.com/issues/35354) 為例，解說 修完 bug 後執行unit test 的流程
  
### Prerequisite:  
- ### 已安裝所需套件及環境 [$ make build_env](https://github.com/alliehayashi/zyxel_notes/blob/master/03-deploy-camelot-backend.md#4-build-host-environment)
- ### 啟用 venv

---
## 1. 撰寫 Unit Test
這次修正的是要讓 MZC 回傳的 error 40001 判斷為 succcess
### `biz_device_request.py` 新增 `judge_mzc_error_code` function
```python
# [BugFix] [License transfer] #35354 liscense transfer will fail when the *machine framework version* is lower than *license version*
# https://redmine.marketplace.zyxel.com/issues/35354
# judge_mzc_error_code: error 40001 --> successful
# convert_error_to_success_code: convert error 40001 to 0
def judge_mzc_error_code(code: int) -> bool:
    if code == mzc_code.need_to_upgrade_device_firmware.value: 
        return False
    
    if code == mzc_code.mzc_general_success.value:
        return False 
    return True
```    
- 改完 code 後要先做 unit test
在 `camelot-backend/tests/unit` 底下新增以 **`test`** 開頭的檔案
- 舉例來說，這次修改了 `asset` 的 `judge_mzc_error_code`，所以就在 `asset` 底下新增一個名為 `test_judge_mzc_error_code` 的測試檔

![1]  
### `test_judge_mzc_error_code` test case
- import business 層更動的 code
- 依照預期得到的結果撰寫 cases 做測試
- assert 放入 function 吃進去後會得到的正確答案。以`judge_mzc_error_code(mzc_code.mzc_general_success.value == False` 為範例，judge_mzc_error_code 收到 mzc_code.mzc_general_success.value (0: success)，應該要回傳 False ，因為它不是 error code
- function 都必須以 `test_` 做命名，否則會測試會找不到檔案
```python
import pytest
from src.business.asset.biz_device_request import judge_mzc_error_code
from src.utility.myzyxel.mzc_helper_code import MzcGeneralCode as mzc_code

# test if the return value is **error code**

# success --> not error code
def test_is_fail_zero():
    assert judge_mzc_error_code(mzc_code.mzc_general_success.value) == False 
    
# 40001 -->  [bugfix]#35354 not error code 
def test_is_fail_40001():
    assert judge_mzc_error_code(mzc_code.need_to_upgrade_device_firmware.value) == False

# 2003 -->  error code 
def test_is_fail_2003():
    assert judge_mzc_error_code(mzc_code.mzc_account_not_found.value) == True
    
```
---
## 2. 執行測試
```
$ cd <project-path>/camelot-backend/tests/
$ export PYTHONPATH="/<project-path>/camelot-backend/"
```
測試使用 pytest 進行，需要的套件在 venv 裡面都有
```
$ pytest -v tests/unit/asset/test_biz_judge_mzc_error_code.py
```
![2]  

可以做反向測試看看會不會報錯
- 這裡把 原本會回傳 False 的 mzc_code.need_to_upgrade_device_firmware.value 放進回傳 True 的 function 實驗
```python
# 2003 -->  error code 
def test_is_fail_2003():
    assert judge_mzc_error_code(mzc_code.need_to_upgrade_device_firmware.value) == True
```
再跑一次 `$ pytest -v tests/unit/asset/test_biz_judge_mzc_error_code.py` 的結果：
![3]  
-可以看到回傳的結果因為是 False，但我們給的值是 True，所以測試失敗了

---
## 3. deploy to dev site
測試完後可以 deploy 過去 dev site，這邊因為修改的是 `asset`，所以就只部署 asset 的部分
```
make asset
```
![4]
> [deploy 指令參考](https://github.com/alliehayashi/zyxel_notes/blob/master/03-deploy-camelot-backend.md#7-deploy)


[1]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/bugfix/01-name-after.png
[2]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/bugfix/02-test-success.png
[3]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/bugfix/03-test-failed.png
[4]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/bugfix/04-make-asset.png


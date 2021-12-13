# Git Pull Request
>[Working With Github](https://github.com/zyxel-dc/camelot-backend/wiki/Working-With-Github)
## 1. Fork Project
所有的開發都必須在 fork 出來的 repo 上進行
![1](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/01-fork.png)
![2](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/02-forked-from.png)

---
## 2. Clone 
把剛剛 fork 出來的 project clone 到 local
```
$ git clone git@github.com:<usrname>/camelot-backend.git
```
---
## 3. 保持同步
透過加入 upstream，可以隨時保持與原本的 repo 同步
### ＃新增 upstream
```
$ git remote add upstream git@github.com:zyxel-dc/camelot-backend.git
```
### ＃檢視 remote repo
```
$ git remote -vvv
```
![3](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/03-remote-vvv.png)
### ＃刪除 upstream
```
$ git remote rm upstream
```

---
## 4. 設定 Commit template
在 config 裡設定 commit 訊息，方便團隊統一格式
```
$ git config --global commit.template ./.git-commit-template
```
`.git-commit-template` 內容
```
[ENHANCEMENT][]
[BUGFIX][]
[WORKAROUND][]
[SPEC-CHANGE][]
[DOCS][]

Description:
	
Root Cause:
	N/A
Solution:
	N/A
Issue ID:
	N/A
Reviewer:
    
Note:
	N/A
```

當需要 commit的時候就會跳出 vim 編輯器帶入此 template
```
$ git commit
```
![7](https://github.com/alliehayashi/Markdown_Pictures/blob/master/pics/git-pr/07-commit-template.png)

---
## 5. Pull Request
首先先 push 到自己的 repo
![6](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/06-which-branch.png)

```
$ git push origin develop    #git push <repo> <branch>
```
再到原始的 repo 去，找到 `Pulll reqests`，建立新的 pull request
![4](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/04-pull%20request.png)
- 黃色是剛剛commit的內容
- Reiviewers 填入所有團隊成員
- Assignees 填入要 approve 的人
![5](https://github.com/alliehayashi/Markdown_Pictures/raw/master/pics/git-pr/05-create-pull-request.png)
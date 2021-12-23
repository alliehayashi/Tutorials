# 常用的 git 指令

## 移除 staged (add)
```
git reset <file>
```
---
## 更新 upstream remote
```
git pull --rebase upstream develop
```

---
## 合併兩筆commit
```
git log   
```
![1]  
- 複製想要合併的 commit 的前一筆 commit id，按 `q` 退出
```
git rebase -i <id>
```

![6]  

- 把第二筆的 pick 改成 s  
   
![2]    

- 接下來會跳出要合併的兩筆 commit 資料  
    
![3]  
- 把不要的 commit message 刪掉，留下需要的  
  
![5]

- 下 `git log` 查，可以發現兩筆資料被合併了
   
![4]

---





 

[1]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/01-git-log.png
[2]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/02-piuck-s.png
[3]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/03-2-commit-messages.png
[4]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/04-rebased.png
[5]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/05-commit-msg.png
[6]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/git-commands/06-before-pick.png

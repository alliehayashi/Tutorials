# 系統問題整理
Issue|Error
---------|---------|
[#1](https://github.com/alliehayashi/Tutorials/blob/master/issues.md#1-base64-doesn't-has-**-d**-command)|base64 doesn't has **-d** command
[#2](https://github.com/alliehayashi/Tutorials/blob/master/issues.md#2-xcrun-error-invalid-active-developer-path)| xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools)|

---
## 1. base64 doesn't has **-d** command
情境：執行 /camelot-backend/tools/`camelot-data-key-plaintext.sh` 的 bash -d 會出錯。`bash64 --help` 發現只有 `-D`& `--decode` 指令，推斷是系統太舊

### Solution: Upgrade MacOS [Mojave (10.14.6) --> Catalina (10.15.7)]

![1]  
note：程式已改為 `bash64 --decode` (PR#741)

---
## 2. xcrun: error: invalid active developer path 
情境：從 Mojave(10.14.6) 升到 Catalina(10.15.7) 之後，command line 執行 Makefile 指令時報錯
```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```
### Solution: 下載 xcode cli
```
xcode-select --install
```
>參考: [Git is not working after macOS Update](https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa)

---


[1]:https://github.com/alliehayashi/Markdown_Pictures/raw/master/issues/01-bash64.png







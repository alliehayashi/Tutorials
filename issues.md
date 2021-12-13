# 系統問題整理
Issue|Error
---------|---------|
[#1](https://github.com/alliehayashi/Tutorials/blob/master/issues.md#1-xcrun-error-invalid-active-developer-path)| xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools)| y|
---
## 1. xcrun: error: invalid active developer path 
情境：從 Mojave(10.14.6) 升到 Catalina(10.15.7) 之後，command line 執行 Makefile 指令時報錯
```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```
Solution: 下載 xcode cli
```
xcode-select --install
```
>參考: [Git is not working after macOS Update](https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa)

---











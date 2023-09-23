# Ch2 Git基礎指令

查看提交歷史  

## git log：查看commit紀錄
```bash
|操作|
$ git log

commit b676f744aa968723020a32dcc57e112256f2f30e (HEAD -> master)
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:24:17 2023 +0800

    新增介紹頁面

commit bcb0bc41788e9697acec5259eed90498418cf596
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:22:36 2023 +0800

    載入CSS樣式表
```

## git reflog：查看git引用紀錄
git reflog 是一個 Git 指令，用來查看 Git 儲存庫中的引用紀錄（Reference Log）。      
引用紀錄是一個記錄分支（branch）、標籤（tag）和 HEAD 等引用的歷史記錄。     
```
$ git reflog

f60f42f (HEAD -> master) HEAD@{0}: reset: moving to f60f42f
d3a26af HEAD@{1}: commit: 修改歡迎頁面(welcome.html)的header
6e6e886 HEAD@{2}: commit: 新增readme
49e755c HEAD@{3}: reset: moving to 49e755c
f60f42f (HEAD -> master) HEAD@{4}: reset: moving to f60f42f
49e755c HEAD@{5}: commit: welcome.html 檔案名稱更正
5b5324a HEAD@{6}: commit: 修改介紹頁面(intro.html)的header
f60f42f (HEAD -> master) HEAD@{7}: commit: 新增介紹頁面 intro.html
bcb0bc4 HEAD@{8}: reset: moving to HEAD^
840034b HEAD@{9}: commit: 載入CSS樣式表
bcb0bc4 HEAD@{10}: reset: moving to HEAD^
7b5708d HEAD@{11}: commit (amend): 新增介紹頁面 intro.html
b676f74 HEAD@{12}: commit: 新增介紹頁面
bcb0bc4 HEAD@{13}: commit (initial): 載入CSS樣式表
```

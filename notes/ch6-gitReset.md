# Ch3 Git reset 移動目前分支的 HEAD 指標，從而影響該分支的提交歷史。

## 撤销最後一個提交 `$ git reset HEAD^`
```bash
|狀態|
$ git log
PS C:\Users\...\03.commit> git log
commit 7b5708d67bbb4bb7fff98b30595c35c3be36502f (HEAD -> master)
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:24:17 2023 +0800
    新增介紹頁面 intro.html
commit bcb0bc41788e9697acec5259eed90498418cf596
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:22:36 2023 +0800
    載入CSS樣式表
```
```bash
|操作|
$ git reset HEAD^
```
```bash
|結果|
$ git log
commit bcb0bc41788e9697acec5259eed90498418cf596 (HEAD -> master)
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:22:36 2023 +0800
    載入CSS樣式表
$ git status
On branch master
Untracked files:
(use "git add <file>..." to include in what will be committed)
        README.md
        intro.html
```
## 將分支指標移到特定提交 `$git reset <commit-hash> --<reset-mode>` 



##  Git Reset 模式

```
回復至指定Commit狀態 `$ git reset f60f42f --mixed`
``````

1. `--mixed` 模式 (預設)     
將 HEAD 分支回滾到指定的提交，但不會更改工作目錄中的檔案。        
工作目錄中會多出拆出來的檔案，原本暫存區的資料會被拋棄，        
這意味著你可以選擇是否重新提交這些變更。
2. `--soft` 模式     
將 HEAD 分支回滾到指定的提交，但不會變更工作目錄中的檔案。      
工作目錄中會多出拆出來的檔案，原本暫存區的資料還在。
3. `--hard` 模式     
將 HEAD 分支回滾到指定的提交，並且會重設工作目錄中的文件，      
使其與指定提交的狀態完全相同。        
工作目錄中不會新增資料，原本暫存區的資料會被拋棄，因此慎重使用。

## 舉例
以下方目錄狀態退回 f60f42f 為例：       
1. 輸入`git reset f60f42f`可以回復至當時`f60f42f`提交狀態
2. 再次輸入`git reset 49e755c`後又可以回復到原本的提交狀態

```Bash
|狀態|
$ git log
commit 49e755cc5a9c0a86b6c55af3a907e16a594959dd (HEAD -> master)
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 18:21:16 2023 +0800
    welcome.html 檔案名稱更正
commit 5b5324a174d32c0e77b15b5698a4ff8a479cedcc
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 18:20:24 2023 +0800
    修改介紹頁面(intro.html)的header
commit f60f42f861cf770d060fef71e077c58e3e12262f
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 18:19:05 2023 +0800
    新增介紹頁面 intro.html
commit bcb0bc41788e9697acec5259eed90498418cf596
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:22:36 2023 +0800
    載入CSS樣式表
```
```Bash
|操作|
$ git reset f60f42f --mixed
Unstaged changes after reset:
M       intro.html
D       welcom.html
```
```Bash
|結果|
$ git log
commit f60f42f861cf770d060fef71e077c58e3e12262f (HEAD -> master)
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 18:19:05 2023 +0800
    新增介紹頁面 intro.html
commit bcb0bc41788e9697acec5259eed90498418cf596
Author: wwwchendev <wwwchendev@gmail.com>
Date:   Thu Sep 21 17:22:36 2023 +0800
    載入CSS樣式表
```



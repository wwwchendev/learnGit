# Ch2 Git基礎指令

## git init：git數據庫初始化
```bash 
$ git init
Initialized empty Git repository in C:/Users/../02.init/.git/
```
## git add：暫存變更 (移至 暫存區(Staging Area))
```bash 
$ git add welcome.html  將指定檔案加入暫存區
$ git add --all         將所有檔案加入暫存區
$ git add .             將所有檔案加入暫存區
```
## git commit：提交變更並附上Commit訊息 (移至 儲存庫(Repository))
```bash 
$ git commit -m ""      提交變更並附上Commit訊息
$ git commit -m --allow-empty "空的" 沒有任何更改也可以產出新Commit
```
### git commit --amend：修改最後一次的Commit紀錄    
    ```bash
    |操作|
    $ git commit --amend -m "新增介紹頁面 intro.html"

    [master 7b5708d] 新增介紹頁面 intro.html
    Date: Thu Sep 21 17:24:17 2023 +0800
    1 file changed, 11 insertions(+)
    create mode 100644 intro.html
    ```

    ```bash
    |結果|
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

## git status：查詢狀態
```bash
$ git status
On branch master
No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   welcome.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   welcome.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
```
- 檔案的狀態
    1. Untracked files ( 未追蹤的檔案 )       
        工作目錄中 尚未被 Git 紀錄在版本控制中，可以使用`$git add`加入暫存。
    2. Changes to be committed ( 暫存區中準備提交的變更 )     
        暫存區 等待被提交，可以使用`$git commit`加入暫存。
    3. Changes not staged for commit ( 已修改但尚未加入暫存區的檔案 )     
        數據庫commit 或 暫存區add 檔案有了新的變更。



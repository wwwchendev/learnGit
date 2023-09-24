# Rebase -i 互動模式 
## 工作目錄環境
依序新增了這些檔案並提交：      
1. init    
1. 建置html環境    
1. 修改title為首頁 
1. 內容新增:歡迎來到我的網頁           
1. 新增圖片    

- 用 `$git log` 查看commit紀錄
    ```
    commit ac420d14101f213c9bd7a982096e05c1519e04c0 (HEAD -> master)
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:14:00 2023 +0800

        新增圖片

    commit 8de3410b7b9ac03aa623f0fbfe4825570e82128d
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:13:01 2023 +0800

        內容新增:歡迎來到我的網頁

    commit 24559de13daf7b51a9588e4d09daed8f07e805f7
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:12:34 2023 +0800

        修改title為首頁

    commit d0c829f3f9199585a207e60bc5146c75eada0931
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:12:07 2023 +0800

        建置html環境

    commit d4dcc9e116bb94b8f2c8b0da4601db6a347e2291
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 09:24:46 2023 +0800

        init
    ```
## 正文開始 

#### Git rebase 管理提交歷史

輸入`$git rebase -i <commit-hash>`指令後會出現`git-rebase-todo文件`，這可以讓我們指定從當前分支特定提交紀錄至最新一次提交為範圍進行提交紀錄重新排列與修改。

```bash
$git rebase -i d4dcc9e
# $git rebase -i <commit-hash>
# 說明：以 d4dcc9e 開始到當前分支最新提交作為範圍內進行rebase
```
```bash
# 會跳出git-rebase-todo文件如下
pick d0c829f 建置html環境
pick 24559de 修改title為首頁
pick 8de3410 內容新增:歡迎來到我的網頁
pick ac420d1 新增圖片
```
#### 操作1. 修改歷史的Commit內容        
```bash
# git-rebase-todo
pick d0c829f 建置html環境
$reword 24559de 修改title為首頁
#想在稍後修改這個Commit訊息，所以將Commit前面的操作從pick為reword(不含$符號)
pick 8de3410 內容新增:歡迎來到我的網頁
pick ac420d1 新增圖片
```
- 會跳出COMMIT_EDITMSG文件如下，在這裡編輯新的提交信息後存關閉。
    ```bash
    在網頁head部分修改title為首頁
    # Please enter the commit message for your changes.Lines starting
    # with '#' will be ignored, and an empty message abortsthe commit.
    #
    # Date:      Sat Sep 23 13:12:34 2023 +0800
    #
    ~
    ```
    完成以後Commit信息以及Commit-Hash都會更新，     
    終端機 會回覆"Successfully rebased and updated refsheads/master."
#### 操作2. 把多個commit合併成一個commit            
```bash   
# git-rebase-todo     
pick d0c829f 建置html環境
$squash 245e210 在網頁head部分修改title為首頁
# squash操作會將目標Commit`245e210` 合併至前一個Commit`d0c829f`        
pick 11c5ede 內容新增:歡迎來到我的網頁
pick a58f7be 新增圖片
``` 
- 會跳出COMMIT_EDITMSG文件如下：   
    ```bash   
    # This is a combination of 2 commits.
    # This is the 1st commit message:
    建置html環境
    # This is the commit message #2:
    在網頁head部分修改title為首頁
    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.        
    ```  
    內容全部清除改成新的提交信息後，保存關閉就可以了
    ```bash      
    建置html環境並修改title為首頁       
    ```  
    完成以後Commit信息以及Commit-Hash都會更新，     
    終端機 會回覆"Successfully rebased and updated refs/heads/master."

    我們可以用`$git checkout`到合併後的提交上確認。

#### 操作3. 把一個commit拆成多個commit      
```bash        
$edit 9b459dc 建置html環境並修改title為首頁
# 將操作設置為 edit 時，Git 會在達到該操作時停止 rebase，
# 並讓我們進行編輯、修改提交或進行其他操作。
pick 637f349 內容新增:歡迎來到我的網頁
pick 570dfc4 新增圖片
``` 
git-rebase-todo 保存關閉後，Git 會在達到該操作時停止 rebase，
此時終端機出現下面訊息：
```bash        
Stopped at 9b459dc...  建置html環境並修改title為首頁
You can amend the commit now, with
git commit --amend
Once you are satisfied with your changes, run
git rebase --continue
``` 
1. 用`$git reset HEAD^`拆掉 9b459dc
    ```bash
    git reset HEAD^
    Unstaged changes after reset:
    M       index.html
    ```
2. 重新提交 建置html環境、修改title為首頁 這兩個Commit就可以了     
    ```bash
    #⚠️這裡VSCode會跳出Error
    error: 
    You have uncommitted changes in your working tree. Please, commit them
    first and then run 'git rebase --continue' again.
    ```
    所以請直接在終端機輸入`$git add`+`$git commit`指令
    ```bash
    $git add index.html
    $git commit -m "建置html環境"
    $git add index.html
    $git commit -m "修改title為首頁"
    ```
3. 使用`$git rebase --continue`繼續rebase
    ```bash
    $git rebase --continue
    Successfully rebased and updated detached HEAD.        
    ```
4. 完成後使用`$git log`指令查看提交歷史
    ```bash
    $git log
    commit a45de2edf67a07a9f93216aee02749c147ee8076 (HEAD -> master)
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:14:00 2023 +0800
        新增圖片
    commit bec5b2df81ead7089b597f4910460295f63b9c54
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:13:01 2023 +0800
        內容新增:歡迎來到我的網頁
    commit c4bc00657103007358f2c94fad3a5d48c046f329
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:10:04 2023 +0800
        修改title為首頁
    commit f2ed8ee3aed626c793a7a823ef0ff0febe1c3460
    Author: wwwchendev <wwwchendev@g
    mail.com>
    Date:   Sat Sep 23 15:09:42 2023 +0800
        建置html環境
    commit d4dcc9e116bb94b8f2c8b0da4601db6a347e2291
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 09:24:46 2023 +0800
        init
        
    ```
#### 操作4. 想要在commit之間增加新的commit  
1. 在目標commit前 操作設置為edit
    ```bash     
    pick f2ed8ee 建置html環境
    $edit c4bc006 修改title為首頁
    #假設想要在`c4bc006`和`bec5b2d`之間新增commit
    #在`c4bc006`前設置操作為 edit
    pick bec5b2d 內容新增:歡迎來到我的網頁
    pick a45de2e 新增圖片
    ```
2. rebase模式暫停到該Commit的時候，添加Commit
    ```bash
    #這裡使用添加空的commit模擬增加Commit的狀況
    $git commit --allow-empty -m "添加空的commit-1"
    $git commit --allow-empty -m "添加空的commit-2"
    ```
3. 使用`git rebase --continue`繼續進行rebase
    ```bash
    $git rebase --continue
    Successfully rebased and updated refs/heads/master.
    ```
4. 使用`git log`查看提交歷史
    ```
    $ git log
    commit acb182368ea319c14496559799288eb09eac6865 (HEAD -> master)
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:14:00 2023 +0800
        新增圖片
    commit d78bc8fd6c4acdd940855ca3cab6165a12c831a9
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:13:01 2023 +0800
        內容新增:歡迎來到我的網頁
    commit b2fc8a4e7797fdbf503f9990fb569a3bb06e41b3
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:19:20 2023 +0800
        添加空的commit-2
    commit 0e19d4324fb5a7502a9650fbe469cc50f86f4db1
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:18:57 2023 +0800
        添加空的commit-1
    commit c4bc00657103007358f2c94fad3a5d48c046f329
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:10:04 2023 +0800
        修改title為首頁
    commit f2ed8ee3aed626c793a7a823ef0ff0febe1c3460
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:09:42 2023 +0800
        建置html環境
    commit d4dcc9e116bb94b8f2c8b0da4601db6a347e2291
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 09:24:46 2023 +0800
        init
    ```
#### 操作5. 調整commit順序              
```bash
#進入`rebase -i`互動模式
pick f2ed8ee 建置html環境
pick c4bc006 修改title為首頁
pick 0e19d43 添加空的commit-1 # empty
pick b2fc8a4 添加空的commit-2 # empty
pick d78bc8f 內容新增:歡迎來到我的網頁
pick acb1823 新增圖片
```
- 此時，我們想要把`d78bc8f`移動到`c4bc006`後面，只需手動調整順序
    ```bash
    pick f2ed8ee 建置html環境
    pick c4bc006 修改title為首頁
    pick d78bc8f 內容新增:歡迎來到我的網頁
    pick 0e19d43 添加空的commit-1 # empty
    pick b2fc8a4 添加空的commit-2 # empty
    pick acb1823 新增圖片
    ```
    保存關閉後終端機就會出現`Successfully rebased and updated refs/heads/master.`
 - 用 `$git log` 查看提交歷史。
    ```        
    commit 6964664e6368aa2df53de412d8c9c2a906917835 (HEAD -> master)
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:14:00 2023 +0800
        新增圖片
    commit 41a0bd756e46c83147634ae8763e86bcb6dd02cc
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:19:20 2023 +0800
        添加空的commit-2
    commit fb07261daf6d47f91b20025a8f93ed67f0e1ebe8
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:18:57 2023 +0800
        添加空的commit-1
    commit 9fc9d6a71a7fb5b3e4b8275a5d06aa3ff81623db
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:13:01 2023 +0800
        內容新增:歡迎來到我的網頁
    commit c4bc00657103007358f2c94fad3a5d48c046f329
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:10:04 2023 +0800
        修改title為首頁
    commit f2ed8ee3aed626c793a7a823ef0ff0febe1c3460
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:09:42 2023 +0800
        建置html環境
    commit d4dcc9e116bb94b8f2c8b0da4601db6a347e2291
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 09:24:46 2023 +0800
        init
    ``````
#### 操作6. 刪除commit    
```bash
#進入`rebase -i`互動模式
pick f2ed8ee 建置html環境
pick c4bc006 修改title為首頁
pick 9fc9d6a 內容新增:歡迎來到我的網頁
$drop fb07261 添加空的commit-1 # empty
$drop 41a0bd7 添加空的commit-2 # empty
# 將commit前的 操作設置為drop，rebase完成後將刪除該提交。
pick 6964664 新增圖片
```
保存關閉後終端機就會出現`Successfully rebased and updated refs/heads/master.`
- 用`$git log`查看提交歷史。  
    ```bash
    commit 14e9a90cca54d4c4b3a09fb3a5a5dbb7885c1891 (HEAD -> master)
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:14:00 2023 +0800
        新增圖片
    commit 9fc9d6a71a7fb5b3e4b8275a5d06aa3ff81623db
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 13:13:01 2023 +0800
        內容新增:歡迎來到我的網頁
    commit c4bc00657103007358f2c94fad3a5d48c046f329
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:10:04 2023 +0800
        修改title為首頁
    commit f2ed8ee3aed626c793a7a823ef0ff0febe1c3460
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 15:09:42 2023 +0800
        建置html環境
    commit d4dcc9e116bb94b8f2c8b0da4601db6a347e2291
    Author: wwwchendev <wwwchendev@gmail.com>
    Date:   Sat Sep 23 09:24:46 2023 +0800
        init
    ```   
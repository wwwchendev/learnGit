# 學習Git

## 1. 安裝設定
1. [安裝git與基本設定](./notes/ch1-InstallandConfig.md)
    - `$ git --version` 查詢 git 安裝版本
    - `$ git config --list --local\global` 視目前設定內容
    - `$ git config --global alias.縮寫 原指令` 設定指令縮寫
1. [文本編輯器](./notes/ch1-editor.md)
    - Vim 編輯器(默認)
    - 以 VS Code 作為 Git 編輯器

## 2. 初始化與基本指令
- [git基本指令](./notes/ch2-basicCommand.md)
    - `$ git init` git 數據庫初始化
    - `$ git add` 暫存變更 (移至 暫存區(Staging Area))
    - `$ git commit -m ""` 提交變更並附上 Commit 訊息 (移至 儲存庫(Repository))
      - [如何寫可讀性高的Commit](./notes/ch2-conventionalCommits.md)
    - `$ git commit --amend`：修改最後一次的Commit紀錄 
    - `$ git status` 查詢 git 追蹤狀態
    - [查看提交歷史](./notes/ch2-log.md)
        - `$ git log` 查看目前分支的提交歷史
        - `$ git reflog` 查看引用（包括分支和 HEAD）的歷史記錄
- 認識 HEAD 指標        
    HEAD 是用來指示目前工作目錄中顯示的是當前分支哪一個提交狀態的標誌，     
    默認情況下，HEAD會隨著分支切換指向當前分支的最新提交。      
- .gitignore        
    .gitignore 文件是用來指示 Git 哪些文件或目錄應該被忽略，不應該被版本控制的。
    ```
    $ .gitignore
    
    # 忽略 node_modules 目錄
    node_modules/

    # 忽略 .env 文件
    .env
    ```

## 3. Git branch 分支
```javascript
$ git branch -a                  查看分支列表(包含遠端分支)
$ git branch <branch-name>       創建分支
$ git branch -m <branch-originName> <branch-newName> 重新命名分支
$ git branch -d <branch-name>    刪除分支
$ git switch <branch-name>       切換分支

$ git merge <otherBranch-name>   合併分支
    // 假設 A 分支要和 B 分支合併
    1. 切換到 A 分支 `$ git switch A` 
    2. 合併 B 分支 `$ git merge B` 
```
- [常見的Git-Flow流程 - 分支合併以及合併衝突](./notes/ch3-merge.md)
### 誤刪分支怎麼辦
```javascript
專案中存在 A 和 B 兩個分支
如果分支 A 上有三個提交 a1、a2、a3，當前 HEAD 指向最新的提交 a3，
如果分支 B 上有兩個提交 b1、b2，當前 HEAD 指向最新的提交 b2，

操作不慎將 `$ git branch -d B`給誤刪，回復步驟：
1. `$git reflog` 查找 b2 的Hash值
2. 建立一個新的分支以指向提交 b2，
   使用 `$ git branch <branch-customName> <commit-latestHash>` 就可以恢復原狀態。
```


## 4. Git tag 標籤
在 Git 存儲庫中添加和管理標籤以標記重要的提交，版本號或發布點。
```javascript
$ git tag <tag-name> <commit-hash>                  輕量標籤(lightweight tag)
$ git tag <tag-name> <commit-hash> -a -m "Tag msg"  有附註的標籤(annotated tag)
$ git tag -d <tag-name>                             刪除標籤
```
```javascript
//舉例
$ git tag v2.0.0 as4a1d4 -a -m "Release version 2.0.0"
    - v2.0.0 是標籤的名稱，用於表示版本號。
    - as4a1d4 是要標記的特定提交的 SHA-1 哈希。
    - -a 表示創建一個有附註的標籤。
    - -m "Release version 2.0.0" 是標籤的附註，這裡提供了一個版本發布的說明。
```
標籤不像分支一樣，它們不會自動推送到遠端存儲庫，        
如果你希望在遠端存儲庫（如 GitHub、GitLab 或 Bitbucket）上分享你的標籤，        
你需要將它們推送（push）到遠端存儲庫。      
```javascript
$ git push origin <tag-name>                        將標籤推送到遠端數據庫
```

## 5. Git checkout 檢出
- 檢出(checkout)      
    - 檢出指的是從版本庫中獲取或查看特定的文件或提交狀態，並將其應用到工作目錄中，以便你可以查看、編輯或測試這些文件或 提交。
    - 不會影響原分支。
    ```javascript
    如果分支 A 上有三個提交 a1 > a2 > a3，當前 HEAD 指向最新的提交 a3，
    使用`$git checkout a2` 檢出a2，以下是會發生的情況：
    
    1. HEAD 移動：
    HEAD 將移動到分支 A 上的提交 a2。 將進入斷頭狀態(detached HEAD)，
    因為此時 HEAD 不再指向分支的最新提交 a3，而是指向了 a2。
    💡斷頭狀態（detached HEAD）
        意思是 HEAD 不再指向分支的最新提交，而是指向了某個具體的提交。
        這通常是用來檢視或測試某個特定提交的專案狀態，
        但需要小心在這種狀態下建立新的提交，因為它們不會成為分支的一部分。

    2. 工作目錄狀態：
    你的工作目錄將會被更新為提交 a2 的狀態，這表示你會看到 a2 時的專案狀態。

    3. 新提交：
    如果你此時建立新的提交，例如 a4，它將建立在分離 HEAD（a2）的基礎上。
    這意味著 a4 將指向 a2，但並不會成為分支 A 的一部分。
    可以認為這是一個 a4 臨時提交，不屬於任何分支。

    4. 返回最新提交 a3：
    當查看完 a2 狀態後，可使用`$ git checkout A`返回到分支 A 的最新提交 a3。
    此時 HEAD 將再次指向分支 A，並且你會回到正常的分支狀態。
    ```

1. [斷頭狀態 特性與用途](./notes/ch5.detachedHEAD.md)
    - 利用斷頭狀態進行 簡單的功能實驗
    - 基於某個Commit 創建新分支進行持續性的開發



## 6. Git reset 重置
- 重置(reset)      
    - 移動目前分支的 HEAD 指標，從而影響了該分支的提交歷史。
    ```javascript
    如果分支 A 上有三個提交 a1 > a2 > a3，當前 HEAD 指向最新的提交 a3，
    使用`$git reset a2` 將專案重置到a2提交的狀態，以下是會發生的情況：
    
    1. HEAD 移動：
    HEAD 將指向到分支 A 上的提交 a2。 

    2. 工作目錄狀態：
    你的工作目錄將會被更新為提交 a2 的狀態，這表示你會看到 a2 時的專案狀態。

    3. 新提交：
    如果你在這個狀態下建立新的提交 a4，a4 將會建立在 a2 的基礎上。 a4 會成為分支 A 的一部分，而分支歷史將變成 a1 > a2 > a4。

    4. a3丟失：
    由於你沒有在建立新提交 a4 之前儲存分支 a3 的引用，a3 將會遺失。
    它不再指向任何提交，因此在分支歷史中將不再出現。
    ```
1. [Git reset 指令](./notes/ch6-gitReset.md)
    - `$ git reset HEAD^` 撤销最後一個提交
    - `$ git reset <commit-hash>` 將分支指標移到特定的提交
    - reset 模式介紹


## 7. Git revert 撤銷提交

```javascript
如果分支 A 上有三個提交 a1 > a2 > a3，當前 HEAD 指向最新的提交 a3，
使用`$git revert HEAD --no-edit` 撤銷提交，以下是會發生的情況：

1. 分支歷史將變成 a1 > a2 > a3 > a4。
   GIT 將創建新的提交 a4用來撤銷 a3 的更改，
   也就是 a4 將包含 a3 的相反更改，以將代碼恢復到 a2 的狀態。
2. 工作目錄狀態：HEAD 指向 a4 (但工作目錄和暫存區的狀態與 a2 是一樣的)。

補充：
1. 使用`$git revert HEAD --no-edit`就不需要編輯提交信息，
   默認提交信息為 REVERT "<previous-commit-msg>"，這也是比較常用的作法。
2. 如果想要自定義Revert產生新Commit的提交信息，
   使用`$git revert HEAD`後會打開文本編輯器，編輯完成後保存退出即可。
```
- 使用時機      
    - git revert 則不會修改分支歷史。它通過創建一個新的逆向提交，以取消之前的提交，但這個操作不會改變分支的歷史，因此比較適合在協作環境中使用，因為它不會強制其他人改變他們的本地歷史。

    - 使用 git revert 時，即使你撤銷了一個錯誤的提交，其他人仍然可以保持他們的工作進度，不會受到太大干擾。這使得 git revert 成為一個更安全的選擇，當你需要回滾提交並且不希望對團隊成員產生太多干擾時。

## 8. Git rebase 重新定義當前分支的參考基準

1. 以 `指定分支` 重新定義當前分支的參考基準 
    ```bash
    $git rebase <other-branch>
    ```

2. `$git rebase -i`互動模式  
輸入`$git rebase -i <commit-hash>`指令後會出現`git-rebase-todo文件`，這可以讓我們指定從當前分支特定提交紀錄至最新一次提交為範圍進行提交紀錄重新排列與修改。
    - 操作1. 修改歷史的Commit內容
    - 操作2. 把多個commit合併成一個commit
    - 操作3. 把一個commit拆成多個commit
    - 操作4. 想要在commit之間增加新的commit
    - 操作5. 調整commit順序 
    - 操作6. 刪除commit         

    可以參考我寫的 [git rebase互動模式 實作筆記](./notes/ch8-rebase.md) 用一樣的專案環境練習這些rebase操作。

3. 如何取消rebase
    ```bash
    $git reset ORIG_HEAD --hard

    說明：
    `ORIG_HEAD` 是一個特殊的引用，它指向你最近的操作之前的 HEAD 的位置。
    通常執行像合併（merge）或重設（reset）等改變分支歷史的操作時，
    Git 會自動保存原始的 HEAD 位置到 ORIG_HEAD。
    ```





## 9. Git filterbranch 過濾Git數據庫中的提交歷史

`$git filter-branch`指令 使用過濾操作對數據庫每個提交應用自定義的處理，包括：        
1. 刪除或修改提交中的特定文件。
2. 重命名提交中的文件或目錄。
3. 修改提交訊息。
4. 合併或拆分提交。
5. 重新排列提交的順序。

### ❗移除數據庫中的敏感信息或不需要的文件

1. 在所有提交歷史中刪除特定檔案 可復原
    ```bash
    $ git filter-branch --tree-filter "rm -f .env"
    #說明：遍歷存儲庫的每個提交，並在每個提交中執行指定的 shell 命令，具體來說，是刪除每個交中的名為 ".env" 的文件。
    # --tree-filter
    # filter-branch 選項，指定每個提交中都要運行的命令，並將工作目錄檔案視為正確樹狀結構。
    # "rm -f .env"
    # 這是一個 shell 命令，會在每個提交中運行。它的功能是刪除存儲庫中的名為 ".env" 的文。
    ```
    - 復原 filter-branch
        ```bash
        $git reset refs/original/refs/heads/master --hard
        ```
2. 在所有提交歷史中刪除特定檔案，且不可被復原
    ```bash
    $ git filter-branch -f --tree-filter "rm -f .env"
    $ rm .git/refs/original/refs/heads/master
    $ git reflog expire --all --expire=now
    $ git fsck --unreachable
    $ git gc --prune=now
    $ git push -f
    ```


## 10. 使用 遠端數據庫 (以github為例)

前往現有git工作目錄專案 (如果沒有的話要先開新目錄並git初始化)

### 新增遠端數據庫
```bash
# git remote add <remoteRepository-name> <remoteRepository-url> 
$git remote add origin https://github.com/<your_github_id>/<github_repository-url>
```

### 將 本地專案 推送至 遠端數據庫 

根據要推送的分支修改&lt;localRepository-name>
```bash
$git push -u origin main   
# git push -u <remoteRepository-name> <localRepository-name>
#說明：將本地分支"main"的變更推送到遠端倉庫"origin"
```   
- 如何刪除遠端分支？        
假設遠端"origin "有一個分支"newfeature"，刪除步驟如下：
    ```bash
    #1.切換到本地端分支
    $ git switch main

    #2.使用 git push 指定--delete選項來刪除遠端分支
    $ git push origin --delete newfeature
    ```

### 將 遠端數據庫 更新拉取至 本地端

```bash
$git fetch origin           拉取遠端變更
#$git fetch <remoteRepository-name>
#說明：從遠端"origin"中獲取最新的提交和分支，只有一個遠端倉庫可省略<remote-name>

$git pull origin main       拉取遠端變更並合併 ($git fetch + $git merge)
#$git pull <remoteRepository-name> <remoteBranch-name>
#說明:將遠端數據庫 origin 上的 main 分支最新變更拉取（下載）到本地分支，並自動合併這些變更到當前分支。
```

### 從遠端複製數據庫到本地端
```bash
$git clone <remoteRepository-url> [<local-directory>] 
#<repository-url> 是遠端 Git 存儲庫的 URL，可以是 HTTPS 或 SSH 協議。
#<local-directory>（可選）
#是克隆存儲庫後在本地創建的目錄名稱。如果不指定此參數將使用原名稱作為目錄名。
```

### Github－ 提出 Issue
GitHub 的 Issue 機制是一種用於追蹤、討論和解決專案中的任務、問題、建議和改進的功能，任何人都可以在 GitHub 上的儲存庫頁面上提出 Issue：
1. 造訪數據庫頁面 點擊 "Issues"頁籤，點擊"New Issue"填寫問題資訊 。      
2.  可以詳細描述問題的性質、復現步驟、期望行為等資訊，點擊"Submit new issue"將問題提交給儲存庫的維護者和其他社群成員。
3.  一旦問題被提交，其他人就可以看到它並就與專案相關的問題進行溝通和討論，包括儲存庫的維護者。 


### Github－ Pull Request流程

Pull Request（PR）機制是 GitHub 上開源專案的重要組成部分，      
它使開發者能夠為開源專案貢獻新功能、修復錯誤和改進。

1. Fork 專案：      
    開發者首先會 Fork（分叉）目標專案的程式碼庫，這將在他們的 GitHub 帳戶下建立一個專案的副本。
2. 建立分支與開發：在Fork中建立一個新分支用於開發某個特定功能或修復，完成後提交。
3. 發起 Pull Request：      
    一旦開發者認為他們的變更已經準備好被合併到目標專案中，他們就會發起一個 Pull Request。 這意味著他們請求目標專案的維護者或團隊來審查他們的變更並考慮將其合併。
4. 審查與討論：         
    Pull Request 被發起後，其他開發者、維護者或團隊成員可以在其中進行審查、討論和提供回饋。 這通常涉及到查看程式碼變更、提出問題、建議改進或測試變更。
5. 合併與自動關閉分支：       
    一旦審查人員確認變更是安全且適當的，Pull Request 可以合併到目標專案的主分支。    
    一旦 Pull Request 被合併，通常會刪除與 PR 關聯的分支，有助於保持程式碼庫的乾淨和整潔。

## 11.Git-flow \ GitHub flow

### Git-flow
如果看完 [Git-Flow 基本教學 - 沈弘哲YT](https://www.youtube.com/watch?v=zXlta66thZY) 這部影片對 Git-flow 有基礎的了解，     
可以參考我在 [在專案中應用Git Flow進行開發，分支合併與解決合併衝突](./notes/ch3-merge.md) 文章中模擬的案例。

### GitHub-flow
六角學院的老師們GitHub flow模擬影片
- [35 GitHub flow 線上三人實際演練 - 六角學院YT](https://www.youtube.com/watch?v=C_u6RLXixLY)

## 12.情境題

### ❗暫存進度        
在分支 missionA 開發過程中 突然需要去修復 missionB分支      
```bash
$git add --all
$git commit -m "做到一半"
$git switch missionB
//..修復missionB

$git switch missionA
$git reset HEAD^
```

### ❗不小心把檔案刪掉了  

在 Git 中，提交是一個非常重要的概念，只有提交後的更改才會被永久記錄下來。       
如果某個文件在尚未提交的情況下被誤刪除，那麼復原時只能回復到最後一次提交的內容。      
Git 記錄的是每次提交的快照，因此未提交的更改不會被保存到歷史中。   
```bash
1. git專案當中有一個尚未被追蹤的README.md
2. README.md 裡面內容是"hello world" 將它$git add 到暫存區
3. $git commit -m "docs(README.md): 新增README文件" 提交README.md 
4. 編輯README.md "hello world --day01" 並$git add 到暫存區
5. 在專案中刪除README.md檔案
```

- 檔案尚未被提交：在工作目錄\暫存區被誤刪
    ```bash
    $git checkout <path/to/file>    檢回特定被刪除的檔案
    $git checkout .                 檢回所有被刪除的檔案
    
    #說明：在正常模式下，當你執行 git checkout 並指定一個已經存在於當前分支的文件時，Git 會從最新提交中恢復該文件的內容，並將其放入你的工作目錄中。
    ```
- 檔案已提交被誤刪
    ```bash
    git reset HEAD^
    ```
- 檔案已推送至遠端被誤刪
    - 💡和團隊一起開發        
    如果檔案已經推送到遠端存儲庫，則不應輕易修改存儲庫的歷史。  
    相反，可以創建一個新的提交，將被刪除的檔案恢復到適當的狀態，    
    然後將新提交推送到遠端存儲庫。
    - 個人開發限定，如果和他人協作的時候不要這樣用
        ```bash
        git reset HEAD^
        git push -f
        ```

### ❗斷頭狀態+基於特定提交進行試驗和測試
1.  __斷頭狀態(detached HEAD)__     
    當使用 檢出指令 `$git checkout <commit-hash>` 查看 特定commit時，        
    會產生 斷頭(detached HEAD)狀態：   
    ```
    💡斷頭狀態 (detached HEAD)      
        意思是 HEAD 不再指向分支的最新提交，而是指向了某個具體的提交。      
        這通常是用來檢視或測試某個特定提交的專案狀態，      
        但需要小心在這種狀態下建立新的提交，因為它們不會成為分支的一部分。      
    ```
    這是暫時性的狀態，只是想查看某個提交而不打算在該提交上進行新的開發時，使用`$git checkout <branch-name>`將 HEAD 移動回該分支的最新提交，就會回到正常的分支狀態。

- __斷頭狀態的特性與用途__      
    斷頭狀態建立新的提交，它將指向分支上的父提交，但本身不會成為原始分支的一部分。      
    這使得斷頭狀態非常適合用於基於某個特定提交進行試驗和測試，      
    因為新的提交既基於父提交，又不會影響原始分支的提交歷史。 

2. __檢出某個Commit後進行開發__     
如果只是臨時查看提交，可在斷頭狀態下建立新提交。        
但若要在該提交上持續開發，建議創建新分支以更好管理工作和提交歷史，避免混亂。      

- 根據需求選擇不同方法：       
    - 方法一：在斷頭狀態下繼續進行新的提交
        ```bash
        # 新功能不符預期       
        放棄新提交，回到原分支 `$git checkout <branch-name>`。
        ```
        ```bash
        # 新功能成果很理想 想合併於原分支
        1. 先使用 `$git checkout <branch-name>` 返回原分支的最新提交。 
        2. 然後使用 `$git merge <commit-hash>` 
            將斷頭狀態下建立的新提交合併到原始分支中，這會將新功能引入到原始分支中。
        3. 最後可以刪除不再需要的分支（如果有的話），
            例如在斷頭狀態下進行實驗的分支以保持分支結構的整潔。
        ```       
    - 方法二：在斷頭狀態下，建立一個新分支並在該分支上進行開發
        ```bash
        1. 用 `$git checkout <commit-hash>` 檢出某個特定Commit，進入斷頭狀態。        
            在這種狀態下，你的 HEAD 不再指向任何分支，而是直接指向該特定的 Commit。     
        2. 用 `$git branch <new-branch-name>` 創建一個新的分支。        
            這個分支將被創建，並且指向當前Commit（斷頭狀態的 Commit）。      
        3. 用 `$git switch <new-branch-name>` 切換到新創建分支。         
            這樣你的 HEAD 將指向這個新的分支，而工作目錄將包含該 Commit 的內容。

        補充：第二和第三步驟可以合併為 `$git switch -b <new-branch-name>`
        ```
        


## 參考資源
- [五倍紅寶石．高見龍 - 為你自己學 Git](https://www.tenlong.com.tw/products/9789864342662) 實體書
- [高見龍 - Git 面試題](https://www.youtube.com/watch?v=YxLqRxF_moY&list=PLBd8JGCAcUAFe5uy3UzokJ-oIVQr1obl4)
- [六角學院 - Git、GitHub 教學](https://www.youtube.com/watch?v=PNEM7CH3ZAg&list=PLYrA-SsMvTPOZeB6DHvB0ewl3miMf-2tj)
- [Git - Documentation](https://git-scm.com/doc)        

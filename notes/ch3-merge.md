## 常見的 Git Flow 工作流程



### MERGE 合併分支
1. 從 master分支分出develop
    ```bash
    #1. 位於 分支master 新增 index.html 檔案並建置HTML環境
    $git add index.html
    $git commit -m "專案 新增 index.html 檔案並建置HTML環境"

    #2. index.html 新增 title
    $git add index.html
    $git commit -m "index.html 新增 title-美味早午餐"

    #3. 建立 分支 develop
    $git branch develop
    ```

2. 設計部 開始在 `feature/design`上開發
    ```bash
    #1. 建立 分支 feature/design、切換到 該分支
    $git branch feature/design 
    $git switch feature/design 

    #2. 在 index.html 新增 body區塊內容 Banner圖片
    $git add index.html
    $git commit -m "index.html 新增 Banner圖片(中式早餐)"
    ```

3. 專案負責人 確認設計沒問題後把`feature/design`合併到`develop`，合併後刪除分支 `feature/design`
    ```bash
    #1. 把 `feature/design` 合併到 `develop`
    $git switch develop
    $git merge feature/design

    #2. 刪除分支 `feature/design`
    $ git branch -d feature/design
    ```

### MERGE Conflict 合併衝突

4. 專案負責人 請企劃部 規劃首頁內容
    ```bash
    #1. 基於`develop` 建立 分支 `feature/content`、切換到 該分支
    $git branch feature/content 
    $git switch feature/content
    ```
5. 企劃部的 A員 和 B員 決定分工合作
    ```bash
    #1. 基於`feature/content` 建立分支 `feature/content/A`、`feature/content/B`
    $git branch feature/content/A
    $git branch feature/content/B
    ```
    - A員
        ```bash
        #1. A員Clone專案後 切換到 分支feature/content/A
        $git clone <repo-URL>
        $git switch feature/content/A

        #2. index.html 新增 body區塊內容 SLOGAN
        $git add index.html
        $git commit -m "index.html 新增 Slogan-活力從早餐開始"

        #3. index.html 新增 body區塊內容 菜單品項:漢堡/吐司
        $git add index.html
        $git commit -m "index.html 新增 菜單品項 漢堡/吐司"
        ```
    - B員
        ```bash
        #1. B員Clone專案後 切換到 分支feature/content/B
        $git clone <repo-URL>
        $git switch feature/content/B

        #2. index.html 新增 body區塊內容 SLOGAN
        $git add index.html
        $git commit -m "index.html 新增 Slogan-美味，簡單，健康"

        #3. index.html 新增 body區塊內容 品質宣言
        $git add index.html
        $git commit -m "index.html 新增 品質宣言"
            #我們的品質宣言：嚴選食材，精心製作，始終保持美味、簡單和健康的承諾。
            #我們追求最高標準，以為您提供最美味的早午餐，同時確保食品的簡單純淨和營養豐富。
            #我們深信，美味早午餐可以簡單做到，並且總是有益於您的健康。
        ```
6. 合併新開發的分支到`feature/content`      
    ```bash
    #1.切換到`feature/content`
    $git switch feature/content

    #2. 把 `feature/content/A` 合併到 `feature/content`，
        合併後刪除分支`feature/content/A`
    $git merge feature/content/A
    $git branch -d feature/content/A

    #3. 把 `feature/content/B` 合併到 `feature/content`        
    $git merge feature/content/B
    
    #4. 此時會產生合併衝突。
    error: could not apply <commit-hash>... <commit-msg>
    hint: Resolve all conflicts manually, mark them as resolved with
    hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
    hint: You can instead skip this commit: run "git rebase --skip".
    hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
    ```

7. 解決合併衝突  
    - 這時 index.html 文件將包含合併衝突的標記
        ```
        <<<<<<< HEAD
        index.html 新增 Slogan-活力從早餐開始
        =======
        index.html 新增 Slogan-美味，簡單，健康
        >>>>>>> <commit-hash>... <commit-msg>
        ```
    1. 手動編輯 index.html 文件，刪除衝突標記，並選擇要保留的內容或根據需求進行修改後保存。
    2. 使用 git add 命令 `$git add index.html` 將已解決的文件標記為已解決
    3. 繼續合併操作，使用 `$git rebase --continue` 命令。
    4. 如果還有其他的合併衝突，重複以上步驟，直到所有的衝突都被解決。

    這樣 `feature/content/A` 和 `feature/content/B` 就被合併在 `feature/content` 內。

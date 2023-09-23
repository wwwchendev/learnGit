# Ch1 安裝設定
## 安裝軟體與服務註冊
- 版控程式 [Git](https://git-scm.com/) 
- 圖形化介面 [SourceTree](https://www.sourcetreeapp.com/)
- 遠端數據庫 [GitHub](https://github.com/)
- ⚠️在VSCode上使用Git

- 查詢git安裝版本
    ```terminal 
    $ git --version
    git version 2.42.0.windows.2
    ```

## Git config 設定
- 檢視目前設定內容 (local範圍)
    ```terminal 
    $ git config --local --list 

    core.repositoryformatversion=0  指定 Git儲存庫的格式版本
    core.filemode=false             控制 Git 是否應該追蹤文件的讀寫權限和可執行屬性
    core.bare=false                 指示儲存庫是否為非裸儲存庫
    core.logallrefupdates=true      控制是否要記錄所有引用的更新
    core.symlinks=false             控制是否應該處理符號連結
    core.ignorecase=true            控制是否區分文件名的大小寫
    ```
- 檢視目前設定內容 (global範圍) 
    - 觀看所有config設定："C:\Users\User\.gitconfig"
    ```terminal 
    $ git config --global --list

    filter.lfs.process=git-lfs filter-process   
    指定 Git Large File Storage（LFS）的過濾處理程序。
    filter.lfs.required=true                指示 Git 是否要求 LFS 過濾器必須存在。
    filter.lfs.clean=git-lfs clean -- %f    定義在提交之前對文件進行的清理操作。
    filter.lfs.smudge=git-lfs smudge -- %f  定義在檢出文件時的處理操作。
    user.name=username                      設定用戶的名稱
    user.email=user@gmail.com               設定用戶的電子郵件地址
    core.editor=code --wait                 指定 Git 使用的文本編輯器
    diff.tool=default-difftool              指定 Git 使用的差異比較工具
    difftool.default-difftool.cmd=code--wait --diff $LOCAL $REMOTE            
    定義差異比較工具的命令
    merge.tool=code                         指定 Git 使用的合併工具

    difftool.sourcetree.cmd=''          定義 SourceTree 整合中的差異比較和合併工具命令
    mergetool.sourcetree.cmd='' 
    mergetool.sourcetree.trustexitcode=true 控制 SourceTree 是否信任合併工具的退出代碼
    ```

- alias 指令縮寫
    ```terminal 
    $ git config --global alias.co checkout
    $ git config --global alias.br branch
    $ git config --global alias.st status
    ```
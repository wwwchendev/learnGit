# Ch1 安裝設定
## 文本編輯器 
### Vim編輯器
```terminal
1.命令模式 Normal 
    i 進入編輯模式
    :w 存檔
    :q 離開
    :wq 存檔離開

2.編輯模式 Insert
    ESC 離開
```

### VS Code 作為 Git 編輯器

通過以下指令將core.editor 設置為VScode，Git將會在需要編輯Commit訊息或解決合併衝突時自動開啟 Visual Studio Code，並在你完成編輯後繼續執行 Git 操作。 
```terminal
git config --global core.editor "code --wait"
```
說明
1. `code` 從終端啟動 VS Code。
2. `--wait`         
    VS Code選項:設置為編輯器在檔案關閉之前等待，        
    這使Git能夠在我們編輯提交資訊並關閉編輯器後，繼續進行相應的操作。       



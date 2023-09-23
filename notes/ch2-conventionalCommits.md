# Ch2 Git基礎指令

## 撰寫可讀性高的 Commit
```bash
<類型 type>[可選的作用範圍 scope]: <描述 description>
# feat(calculator.js): 添加 \`calculateTotal()\` 函數，用於計算總金額。
# feat(cart.js)!: 完成金流串接邏輯。

[可選的正文 body]
#這個提交加入了一個新的 `calculateTotal()` 函數到 `calculator.js` 檔案中，用於計算購物車中商品的總金額。 完成了 `cart.js` 檔案中的金流串接邏輯，現在可以正常處理支付操作。

[可選的頁腳 footer]
# 相關 issue: #123,#456
```
- [慣例式提交 Conventional Commits 1.0.0 - github](https://github.com/conventional-commits/conventionalcommits.org/blob/master/content/v1.0.0/index.zh-hant.md)

## 各個Commit類型與情境示例

1. wip：表示發起的工作（Work in Progress），尚未完成。
    ```
    feat(api): send an email to the customer when a product is shipped
    ```
2. feat：新增功能（Feature）的變更，通常是新增了一個新功能、模組或檔案。
    ```
    feat(calculator.js): 添加 \`calculateTotal()\` 函數，用於計算總金額。
    ```
3. fix：修復錯誤（Bug Fix）的變更，用於修復程式碼中的錯誤或問題。
    ```
    fix: 解決日期格式錯誤的問題。
    ```
4. docs：文件變更，包括更新 README、文件或註釋等。      
無論是新增 README 文件、README 新增 第二小節、README 勘誤 第二小節這些情況都是使用docs。
    ```bash
    docs: 新增 README 文件

    新增了一個新的 README 文件，提供了專案的基本文件和說明。
    ```
5. style：代表程式碼風格變更，如空格、縮排、命名等，不影響程式碼功能。
    ```
    style(eslint): 更新程式碼縮排風格

    由於 ESLint 的規則要求，對整個程式碼庫進行了縮排風格的調整。

    這次提交主要包括了以下更改：
    - 將所有縮排從 4 個空格改為 2 個空格
    - 修正了一些 ESLint 報告的縮排錯誤
    - 更新了 `.eslintrc` 設定檔以反映新的縮排規則

    這些變更旨在提高程式碼的一致性和可讀性，以便更好地符合專案的程式碼風格標準。

    # 關聯任務: #789
    ```

6. test：測試相關的變更，例如新增或修改測試範例。
    ```
    test: 為使用者登入模組添加單元測試。
    ```
7. chore：雜項事務變更，通常是目前工具、配置等方面的變更，不影響功能。
    ```
    chore: 更新 webpack 配置，以提升編譯效能。
    ```
8. build：目前工具變更，如更新套件依賴、設定變更等(gulp.npm...)。
    ```
    build: npm升級到最新版本。
    ```
9. revert：撤銷（Revert）之前的變更。
    ```
    revert(user.html): 還原 "feat: 添加使用者個人資料圖片上傳功能"

    這還原了commit abcd123。
    ```
10. refactor：代表程式碼重構，指的是在不改變功能的前提下改進程式碼結構或效能。
    ```
    refactor(utils.js): 重構計算稅費的邏輯

    重構了 `calculateTax()` 函數，以提高程式碼的可讀性和可維護性。 沒有改變其功能或輸出。
    ```
11. perf：效能最佳化（Performance）的變更，通常是針對程式碼的效能進行最佳化。
    ```
    perf(api): 最佳化資料庫查詢效率

    透過改進資料庫查詢語句，減少了查詢回應時間，提高了 API 的效能。
    ```
12. ci：持續整合（持續整合）相關的變更，如配置 CI/CD 流程。
    ```
    ci: 更新 CI/CD 配置以自動部署

    更新了持續整合和持續交付（CI/CD）流程的配置文件，以實現自動部署到生產環境。 現在，每次推送到主分支後，將自動部署。
    ```
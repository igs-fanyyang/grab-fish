# 摸魚遊戲廳：公開網站版

不用登入任何帳號就能玩，Shiba外送 的排行榜也能上傳。

- **網站**：GitHub Pages，免費。
- **排行榜**：Firebase Firestore，免費額度就夠用。每個瀏覽器會自動拿到一個匿名身分，一個身分一筆成績。

## 資料夾內容

| 檔案 | 用途 |
|---|---|
| `index.html` | 遊戲廳大廳，並負責排行榜和 Firebase 之間的讀寫 |
| `games/` | 三個遊戲（SkiFree、Chip's Challenge、Shiba外送） |
| `firebase-config.js` | **要你填**：Firebase 網頁設定 |
| `firestore.rules` | 排行榜的安全規則，貼到 Firebase 主控台 |

## 一、建立 Firebase 排行榜（約 10 分鐘）

1. 到 https://console.firebase.google.com 用你的 Google 帳號登入，按「新增專案」，名稱例如 `fish-arcade`。Google Analytics 可以關掉。
2. 左側「建構 → Authentication」→「開始使用」→「Sign-in method」→ 啟用 **匿名（Anonymous）**。
3. 左側「建構 → Firestore Database」→「建立資料庫」，位置選 `asia-east1`（台灣），先選「正式版模式」。
4. Firestore 頁面上方「規則」分頁：把 `firestore.rules` 的內容整段貼上，取代原本的規則，按「發布」。
5. 專案總覽旁的齒輪 →「專案設定」→「一般」→ 最下方「你的應用程式」→ 按 `</>`（網頁）圖示新增應用程式，名稱隨意，**不用**勾 Firebase Hosting。
6. 畫面會出現一段 `const firebaseConfig = { … }`。把大括號整段複製，貼到 `firebase-config.js`，取代 `null`：
   ```js
   window.FIREBASE_CONFIG = { apiKey: "…", authDomain: "…", projectId: "…", storageBucket: "…", messagingSenderId: "…", appId: "…" };
   ```
   這組設定本來就是放在網頁裡給瀏覽器用的，可以公開。真正保護排行榜的是第 4 步的規則。

## 二、放上 GitHub Pages（約 5 分鐘）

1. 到 https://github.com 登入（沒有帳號就先註冊），右上「+」→「New repository」。名稱例如 `fish-arcade`，設成 **Public**，建立。
2. 在新的 repository 頁面按「uploading an existing file」，把這個資料夾裡的所有檔案拖進去：`index.html`、`firebase-config.js`、`firestore.rules`、`README.md`，以及整個 `games` 資料夾。拖完按「Commit changes」。
3. 到「Settings → Pages」，Source 選「Deploy from a branch」，Branch 選 `main`、資料夾選 `/ (root)`，按 Save。
4. 等 1～2 分鐘，頁面上方會出現網址，例如 `https://你的帳號.github.io/fish-arcade/`。這就是可以分享給任何人的連結。
5. 回到 Firebase：「Authentication → 設定 → 已授權的網域」，新增 `你的帳號.github.io`，匿名登入才會在這個網址生效。

## 三、確認

- 打開網址，進 Shiba外送 → 按「排行榜」：
  - 顯示「還沒有人上榜…」：連線成功。
  - 顯示「排行榜暫時連不上」：請檢查 `firebase-config.js` 和已授權的網域。
- 玩一局，結束時應出現「新紀錄！已更新…排行榜」。

## 注意

- **排行榜分開**：這個網站的排行榜和 OpHub、Claude 獨立遊戲廳的排行榜是三份，互不相通。
- **匿名身分**：清除瀏覽器資料或換裝置，會被當成新玩家。
- **成績可偽造**：規則限制了欄位和數值範圍，但沒有登入驗證的公開排行榜無法完全防作弊。
- **更新遊戲**：遊戲在 OpNote 的 `games/` 改完後，把新檔案重新上傳到 GitHub repository 的 `games/`，覆蓋舊檔即可。

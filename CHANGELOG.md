# 漂流瓶 v2 更新說明

## 🔴 P0 — 必修

### 1. routines 狀態提升
- `routines` state 從 `RoutineTab` 移到 `DriftBottleApp`
- 透過 props 傳遞 `routines / setRoutines / syncRoutines`
- 新增/修改/重置都會觸發 Firestore 同步
- 跨裝置可同步、刷新不消失

### 2. Service Worker (sw.js)
- cache 版本提升到 `driftbottle-v2`
- 統一使用 cdnjs（與 index.html 一致）
- 完整 cache list：React/ReactDOM/Babel + 所有圖片 + 字型
- Firebase API 永遠走網路（即時資料）
- 離線時導頁 fallback 到 index.html

### 3. 語音輸入
- 完整錯誤碼處理：`not-allowed` / `no-speech` / `audio-capture` / `network` / `aborted`
- 錯誤訊息以 UI 顯示（不再無聲失敗）
- 錄音中斷時保留已辨識文字

### 4. Firebase 安全與隱私
- `firestore.rules` — 只能讀寫自己的 `users/{uid}` 文件
- 新增「清空雲端資料」按鈕（只清資料、保留帳號）
- 新增「刪除帳號」按鈕（移除 Auth 帳號 + 標記 Firestore 文件）
- 重新登入需求會以訊息提示

## 🟠 P1 — 實用性

### 5. 待辦升級
- 漂流瓶可選填 **截止時間**（datetime-local）
- 三個 filter：**全部 / 今天 / 已完成**
- 過期未完成顯示紅色警示
- 細節面板顯示截止時間

### 6. 自動重置
- daily routines：跨日自動清空 checked
- weekly routines：跨週（週日為界）自動清空
- 用 `lastResetKey` 識別

### 7. 新手引導 + 空狀態
- 首次開啟顯示 onboarding overlay（記在 localStorage）
- 各 filter 有對應空狀態文字

## 🟡 P2 — 體驗優化

### 8. 放鬆區
- 加入主音量滑桿（0–100%）
- 持久作用於整個 AudioContext

### 9. 減少動畫模式
- 設定面板 toggle，記在 localStorage
- 開啟後所有 animation/transition 縮短到接近 0

### 10. 同步狀態
- 同步中：頭像旁邊綠點閃爍
- 同步成功：靜止綠點
- 同步失敗：紅色珊瑚色點

## 📦 交付檔案
- `index.html` — 主程式
- `sw.js` — Service Worker v2
- `firestore.rules` — 在 Firebase Console 貼上：
  Console → Firestore Database → Rules → 貼上 → 發布

## ⚠️ 部署提醒
1. 把 `firestore.rules` 內容貼到 Firebase Console 並發布
2. 如果之前用過舊版 PWA，更新後 sw.js 會自動清舊 cache
3. iPhone 加到主畫面後若要更新，需要刪除主畫面 icon 重加（PWA 限制）

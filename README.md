# ![萊姆輸入法](LimeStudio/app/src/main/res/drawable-hdpi/logo.png) 萊姆輸入法

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

LIME 是一套以繁體中文輸入為核心的開源輸入法專案，採用 GPL 授權並持續開源維護，目前支援 Android 與 iOS 兩大平台。

本次 2026 版本為相隔多年後的長週期更新，重點包含 Android 新版相容性、架構重整與整體穩定性提升。

## 個人化修改（leochen0622 fork）

本 fork 以官方 [lime-ime/limeime](https://github.com/lime-ime/limeime) 的 `master`（`0063630a`）為基礎，加上以下個人化修改（`a11e65c9`–`ed65dc8c`，共 6 個 commit）。除特別註明外，修改僅限 Android 版。

### 按鍵滑動輸入符號（`a11e65c9`）

在「LIME+數字列鍵盤」與「數字列英文鍵盤」上，部分按鍵可以朝某個方向滑動來輸入符號，配置與美式鍵盤的 Shift 符號相同。鍵帽邊緣以粗體小字標示可滑出的符號，滑動時按鍵預覽會顯示即將輸出的符號。

- **啟用中文鍵盤**：設定 App →「輸入法」→ 選擇輸入法 →「軟鍵盤配置」→「鍵盤佈局」→「LIME+數字列鍵盤」。拼音與自建輸入法預設就是這個鍵盤。
- **啟用英文鍵盤**：「喜好設定」→「數字列英文鍵盤」（預設開啟）。
- **操作**：在按鍵上朝標示方向滑動超過按鍵寬度（或高度）的 40% 後放開。沒有標示的方向維持原本「滑到相鄰按鍵」的行為。按下 Shift 後的鍵盤沒有滑動符號。

| 按鍵 | 下滑 | 左滑 | 右滑 | 上滑 |
| --- | --- | --- | --- | --- |
| `1` | `!` | | `~` | |
| `2` `3` `4` `5` `6` `7` `8` `9` `0` | `@` `#` `$` `%` `^` `&` `*` `(` `)` | | | |
| `q` | | | `` ` `` | |
| `o` | `-` | `_` | | |
| `p` | `=` | `+` | | |
| `j` | `[` | `{` | | |
| `k` | `]` | `}` | | |
| `l` | `\` | `\|` | | |
| `b` | `;` | `:` | | |
| `n` | `'` | `"` | | |
| `m` | `/` | `?` | | |
| `,` | | | | `<` |
| `.` | | | | `>` |

`j`、`k`、`n` 下滑的 `[`、`]`、`'` 會當成一般按鍵送出，所以在無蝦米等以這些符號為字根的輸入法中仍可用來組字（例如滑出 `[` 再按空白鍵得到「「」）。其餘滑動符號一律直接輸出。

### 數字列鍵盤版面調整（`020ef44e`）

適用於「LIME+數字列鍵盤」、「數字列英文鍵盤」與三頁符號鍵盤：

- 第三列的 Shift 鍵（符號鍵盤為換頁鍵）由 15% 寬縮為 10%，最後一個字母與刪除鍵之間留 5% 間隔。
- 最下列移除「隱藏鍵盤」鍵，空間併入空白鍵（30% → 45%；英文鍵盤的網址與 Email 列為 15% → 30%）。
- 模式鍵移到最下列左側：字母鍵盤依序為「123」、語言切換（「EN」或「中」），符號鍵盤為「EN」、「中」。
- 原本長按隱藏鍵開啟的 LIME 選單，改為在鍵盤上快速往上滑開啟；隱藏鍵盤則在鍵盤上往下滑，或使用系統返回手勢。

### 中文模式下原樣送出輸入碼（`5f02be64`、`2ca4548f`、`ed65dc8c`）

組字中（已打入字根、候選字列出現時），以下按鍵會送出輸入碼本身，而不是反白的候選字。以無蝦米為例：

| 操作 | 結果 | 範例 |
| --- | --- | --- |
| 按 Enter | 送出輸入碼本身 | `bcs` + Enter → `bcs`；`[` + Enter → `[` |
| 按數字鍵（該數字不是輸入法字根時） | 送出輸入碼，再接上數字 | `bcs6` → `bcs6` |
| 按空白鍵（與官方版相同） | 送出反白的候選字 | `bcs` + 空白 + `6` → `陳6`；`[` + 空白 → `「` |

- **數字鍵**：適用於所有不以數字為字根的輸入法，例如無蝦米、倉頡、行列30。注音、大易、行列10 等以數字為字根的輸入法不受影響；行列30 以 `w`、`hg` 開頭的數字符號輸入也照舊。使用實體鍵盤時，數字選字仍然優先。iOS 版也同步了這條規則，但尚未在 iOS 上建置與測試。
- **大寫字母**：以 Shift 或 Caps Lock 打出的大寫字母直接輸出，不再被轉成小寫當作字根（例如 Shift + `a` → `A`）。碼表本身定義了大寫字根時除外。
- 沒有在組字，或在英文模式時，Enter 與數字鍵的行為與官方版相同。

### 不顯示建議字的欄位也顯示組字碼（`3c8395c0`）

許多搜尋框與聊天輸入框會宣告為「不顯示建議字」的欄位。官方版在這類欄位中不會把輸入碼（帶底線）放進文字框，因此無法邊打字根邊篩選搜尋結果。本 fork 改為所有欄位都顯示組字碼，只有強制使用英文的密碼與 Email 欄位例外。此項不需任何設定。

### 安裝

本 fork 不發布 APK，也不在 Google Play 上架，需自行建置並以自己的金鑰簽署（參考 [APK / AAB Release & Sideload Workflow](docs/APK_RELEASE_WORKFLOW.md) 的 Workflow B）。自行建置的套件名稱為 `org.limeime`，與 Google Play 版相同但簽署金鑰不同，無法直接覆蓋更新。裝置上若已安裝 Google Play 版，請先在 LIME 內備份資料，解除安裝後再安裝本 fork 的 APK，然後還原。

## 官方網站與使用手冊

一般使用者請從官方網站開始：

- **官方網站**：<https://lime-ime.github.io/limeime>
- **使用手冊**：<https://lime-ime.github.io/limeime/pages>

第一次[安裝啟用](https://lime-ime.github.io/limeime/pages/quick-start.html)、[換機備份還原](https://lime-ime.github.io/limeime/pages/database-management.html)、[鍵盤輸入](https://lime-ime.github.io/limeime/pages/keyboard-input.html)、[調整喜好設定](https://lime-ime.github.io/limeime/pages/preferences.html)、[按鍵震動](https://lime-ime.github.io/limeime/pages/preferences.html)、[語音輸入](https://lime-ime.github.io/limeime/pages/troubleshooting.html)或[疑難排解](https://lime-ime.github.io/limeime/pages/troubleshooting.html)，請先閱讀使用手冊。

## 隱私與安全性

LIME 不收集、不分享任何使用者資訊，也不要求帳號登入或個人資料存取。所有輸入內容與學習資料僅保存在裝置本機。

App 僅在功能需要時宣告少數權限（如下載碼表、按鍵震動、內建語音輸入等），各權限的實際用途與平台差異，請見手冊的[隱私說明](https://lime-ime.github.io/limeime/pages/privacy.html)。

## 下載

### 最新正式版（GitHub Release）

- 版本：v6.1.38
- APK：[LIMEHD2026-6.1.38.apk](https://github.com/lime-ime/limeime/releases/download/v6.1.38/LIMEHD2026-6.1.38.apk)

## 版本與相容性

- 套件名稱：net.toload.main.hd2026
- 顯示名稱：萊姆輸入法A
- 目標 SDK：37
- 最低支援 SDK：25（Android 7.1；自 v6.1.32 起）
- GitHub Release APK 與 Google Play 版本使用不同套件名稱與簽署金鑰，可以同時安裝並共存，但不能互相直接更新或升級
- Google Play 封閉測試使用者請從 Google Play 更新。若要在 GitHub APK 與 Google Play 版本之間切換使用，請先備份輸入法資料

### 為何自 v6.1.32 起最低支援 Android 7.1（API 25）

LIME 的輸入法碼表由 GitHub 雲端下載；GitHub 會將下載導向使用 Let's Encrypt 憑證鏈的 `raw.githubusercontent.com`。Android 7.0（API 24）以前的系統 CA 信任庫不包含該憑證鏈所需的 ISRG Root X1，因此無法建立受信任的 TLS 連線，碼表下載與安裝會失敗。

系統 CA 信任庫屬於 Android 作業系統，App 無法安全地替舊系統更新。LIME 不採用略過憑證驗證或內建自訂 CA 的繞過方式，因此從 v6.1.32 起，Android 版本的最低支援層級設定為 API 25。API 21–24 裝置無法從 Google Play 安裝 v6.1.32 之後的版本，也無法側載新版 APK；仍可繼續使用 v6.1.31 或更早的相容版本。

## 開發

本專案包含 Android 與 iOS 兩套平台原始碼：

- **Android（`LimeStudio/`，Java / Kotlin）**：以 Android Studio 開啟 `LimeStudio/` 專案根目錄，待 Gradle Sync 完成後，選擇 `app` 設定檔即可建置與執行。建議使用近期版本的 Android Studio 與相符的 Android SDK / Build Tools。
- **iOS（`LimeIME-iOS/`，Swift）**：以 Xcode 開啟 `LimeIME-iOS/LimeIME.xcodeproj`，選好目標 scheme 與簽署團隊後即可建置與執行。鍵盤擴充需在實機或模擬器上啟用後測試。

## 主要技術規格文件

開發前建議先閱讀以下核心架構與格式規格文件（位於 `docs/`）：

### 架構

- [LimeIME 架構總覽](docs/LIMEIME_ARCHITECTURE.md) — 整體架構與模組關係
- [UI 架構（MVC）](docs/UI_ARCHITECTURE.md) — 介面層的 MVC 設計
- [IMService 規格](docs/IM_SERVICE.md) — 輸入法服務核心規格

### 資料與格式

- [`limedb` 規格](docs/LIMEDB_SPEC.md) — LimeDB 資料庫格式
- [CIN / LIME 檔案格式規格](docs/CIN_LIME_SPEC.md) — 輸入法碼表文字格式

### 鍵盤佈局

- [Android / iPhone 鍵盤佈局](docs/ANDROID_IPHONE_KEYBOARD.md) — 跨平台鍵盤佈局與特殊鍵行為
- [iPad 鍵盤佈局](docs/IPAD_KEYBOARD.md) — iPad 專屬鍵盤佈局與候選列

### 設定

- [LIME Settings 規格](docs/LIME_SETTINGS.md) — 設定 App 與喜好設定規格
- [喜好設定對照表](docs/PREFS_TABLE.md) — iOS 與 Android 各喜好設定項目對照

## 問題回報

若您遇到使用問題，請優先透過 GitHub Issue 回報，並盡量提供以下資訊：

- 裝置品牌與型號（例如：Samsung Galaxy S23 / iPhone 15）
- 系統版本（例如：Android 14 / API 34，或 iOS 17）
- App 版本（例如：v6.1.34）
- 安裝來源（GitHub Release 或歷史版本 APK）
- 問題發生步驟與預期結果
- 實際結果與錯誤訊息（若有）

資訊越完整，越能加速問題重現與修正。

## 開發團隊

- Jeremy Wu
- Julian Chen
- Art Hung

## 聯絡方式

- 萊姆小編 [limeimetw@gmail.com](mailto:limeimetw@gmail.com)

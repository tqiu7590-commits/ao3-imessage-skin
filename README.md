# iMessage Work Skin for AO3

> An AO3 work skin that renders an iMessage-style chat inside your fic — with a new-message notification banner and tap-to-reveal messages. Pure CSS + HTML, no JavaScript, written against AO3's work-skin rules and tested on AO3.

在 AO3 作品正文裡插入一個模擬 iPhone iMessage 的聊天界面。讀者先看到一條新消息通知，點開後進入聊天，**每點一下出現下一條消息**。只用 AO3 work skin 允許的 CSS 和 HTML，不需要 JavaScript，已在 AO3 上實際保存並正常使用。

## 效果

- 🔔 **新消息通知橫幅**：點一下橫幅收起，手機聊天界面出現
- 👆 **點一下出一條**：「•••」輸入中氣泡就是點擊按鈕，點開後消息淡入彈出
- ⏱ **連發效果**：同一步裡放多條消息，會間隔 0.5 秒逐條冒出
- 💬 **自動氣泡尾巴**：每一方連續消息的最後一條才有尾巴，新消息到來時尾巴自動下移
- 📏 **自動分組間距**：同一人連發間距緊湊，換人說話間距拉開
- 🛡 **防誤觸**：點到已出現的舊消息不會把對話收起來
- 🕐 時間分隔、「已讀」小字、「輕觸繼續」提示
- 📱 手機寬度下正常顯示，不會橫向溢出

## 文件

| 文件 | 說明 |
| --- | --- |
| `imessage-skin.paste.css` | Work Skin，整段貼進 AO3 |
| `imessage-body.paste.html` | 正文範本，照它的結構寫你自己的對話 |

## 使用方法

### 1. 建立 Work Skin

1. AO3 → 右上角頭像 → **Dashboard** → **Skins** → **My Work Skins** → **Create Work Skin**
2. 起個名字，把 `imessage-skin.paste.css` 的全部內容貼進 **CSS** 欄位，保存
3. **保存後讀回核對**：重新打開這個皮膚，確認規則都還在（數一下 `{` 的數量，應該和文件裡一樣）

> AO3 會自動在每條規則前面加上 `#workskin`，不用自己寫。
>
> 皮膚裡刻意沒有註釋：有人回報過，AO3 保存帶註釋的皮膚時會無聲地丟掉一批規則。自己修改時也不要加 `/* 註釋 */`。

### 2. 在作品裡啟用

發文或編輯作品時，在 **Associations → Select Work Skin** 選擇剛才建立的皮膚。

### 3. 貼正文

正文編輯器切到 **HTML** 模式，照 `imessage-body.paste.html` 的結構寫你的對話。建議先存成草稿，確認顯示正常再發佈。

## HTML 結構

```html
<details class="notify">                          <!-- 通知橫幅（可選） -->
<summary><span class="banner"><span class="app">iMESSAGE · 现在</span><span class="from">Yanis</span><span class="preview">您就瞧好吧！</span></span></summary>
<div class="phone">
<div class="head"><span class="avatar">Y</span><span class="name">Yanis ›</span></div>
<div class="chat">

<details class="step" open>                       <!-- 第 0 步：一打開就顯示，帶 open，沒有輸入中氣泡 -->
<summary><span class="time">3:21</span><span class="msg out">自己的消息</span><span class="msg in">對方的消息</span></summary>

<details class="step">                            <!-- 第 1 步：讀者點一下 -->
<summary><span class="typing in"></span><span class="msg in">第 1 步的消息</span><span class="hint">轻触继续</span></summary>

<details class="step">                            <!-- 第 2 步：套在第 1 步裡面 -->
<summary><span class="typing out"></span><span class="msg out">第 2 步的消息</span><span class="read">已读 15:26</span></summary>
</details>

</details>
</details>
</div>
</div>
</details>
```

（上面的 `<!-- -->` 只是說明，貼進 AO3 時不要帶註釋。）

**規則：**

- **所有消息都寫在 `<summary>` 裡面。** AO3 保存正文時會把其他地方並排的 `<span>` 包進 `<p>`、把換行變成 `<br>`，但不會改動 `<summary>` 的內容
- 每個 `<details class="step">` 是一次點擊
- `<summary>` 裡的順序：**輸入中氣泡（必須第一個）** → 可選的 `time` → 這一步的消息 → 可選的 `read` / `hint`
- 下一步寫在這一步的 `</summary>` 後面、`</details>` 前面，一層套一層；有幾個 step，最後就要有幾個 `</details>`
- 一打開就要顯示的消息，放在第一個 `<details class="step" open>` 裡，不放輸入中氣泡
- 不想要通知橫幅的話，去掉外層的 `<details class="notify">`、它的 `<summary>` 和對應的 `</details>`，只留 `<div class="phone">`

### Class 速查

| Class | 用途 |
| --- | --- |
| `msg in` | 對方的消息（灰色，靠左） |
| `msg out` | 自己的消息（藍色，靠右） |
| `time` | 居中的時間分隔小字；放在步驟開頭時，會把前後消息分成兩組 |
| `read` | 「已讀」「已送達」小字，放在自己消息後面 |
| `typing in` / `typing out` | 「•••」輸入中氣泡，讀者點它出下一步 |
| `hint` | 「輕觸繼續」提示，放在第一步 `<summary>` 的最後 |

氣泡尾巴和消息間距都是自動判斷的，不用手動加 class。

## 自訂

AO3 的 work skin **不支持 CSS 變量**（`--xxx` 和 `var()`），所以顏色和尺寸都直接寫在規則裡。要換主題，在 `imessage-skin.paste.css` 裡全文搜索替換：

| 值 | 用途 |
| --- | --- |
| `#0a84ff` | 自己的氣泡（藍色） |
| `#e9e9eb` | 對方的氣泡（灰色）、對方的輸入中氣泡 |
| `#ffffff` | 螢幕底色、自己的文字；**氣泡尾巴的遮罩也用它**，換底色時要一起換 |
| `#000000` | 對方的文字、聯絡人名字 |
| `#f6f6f6` | 頂部聯絡人欄 |
| `#d1d1d6` | 手機邊框、分隔線 |
| `#8e8e93` | 時間、已讀、提示等灰色小字 |
| `14px` / `0.4px` | 消息字號 / 字間距（在 `.phone` 裡） |
| `3px` / `12px` | 同一人連發的間距 / 換人說話的間距 |

## AO3 的限制與實現方式

AO3 work skin 的規則來自 [otwarchive 源碼](https://github.com/otwcode/otwarchive)：通用的 CSS 白名單（`config/config.yml`、`lib/css_cleaner.rb`），加上 work skin 專門的限制（`app/models/work_skin.rb`）。

| 限制 | 對策 |
| --- | --- |
| work skin 不能用自定義屬性（`--xxx`）和 `var()` | 顏色、尺寸直接寫死；尾巴和間距用選擇器分別設定 |
| 不能用 JavaScript、`<input>` | 用 AO3 允許的 `<details>` / `<summary>` 做點擊展開 |
| 不能用 `@keyframes` / `animation` | 用 `transition` 配合 `details[open]` 狀態切換做淡入彈出 |
| 不能用 `pointer-events` | 已展開的步驟蓋一層透明的 `::after` 罩子防誤觸，下一步用 `z-index` 浮在罩子上 |
| 不能用 `calc()`、`cubic-bezier()` | 只用固定數值和 `ease` 等關鍵字 |
| AO3 的 CSS 解析器按逗號拆分選擇器 | `:has()` 括號裡不放逗號 |
| 保存正文時會插入 `<p>` / `<br>`（[`lib/paragraph_maker.rb`](https://github.com/otwcode/otwarchive/blob/master/lib/paragraph_maker.rb)） | 消息都寫在 `<summary>` 裡（AO3 不改動它的內容）；`.phone` 內的 `<br>` 隱藏、`<p>` 清掉邊距 |
| 有人回報：保存帶註釋的皮膚時會無聲丟規則 | 皮膚不帶註釋，保存後讀回核對 |

**已知限制：**

- 新出現的「•••」氣泡沒有進場動畫（沒有 keyframes 時做不到）
- 輸入中氣泡的放大效果只在滑鼠懸停時出現，手機上是靜止的
- 氣泡尾巴和分組間距依賴 CSS `:has()`，需要 Safari 15.4+ / Chrome 105+ / Firefox 121+；更舊的瀏覽器上所有消息都會顯示尾巴、間距一樣，但不影響閱讀

## 參考

- [otwcode/otwarchive](https://github.com/otwcode/otwarchive) — AO3 源碼，CSS / HTML 白名單和 work skin 限制
- [How to Make an AO3 Work Skin That Doesn't Break](https://www.wordfokus.com/how-to-make-ao3-work-skin/) — 註釋導致丟規則的回報、讀回核對的方法
- [AO3 Workskins 合集](https://archiveofourown.org/collections/Workskins)

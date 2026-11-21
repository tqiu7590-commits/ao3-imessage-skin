# iMessage Work Skin for AO3

> An AO3 work skin that renders an iMessage-style chat inside your fic — with a new-message notification banner and tap-to-reveal messages. Pure CSS + HTML, no JavaScript, written against AO3's work-skin rules and tested on AO3.

在 AO3 作品正文里插入一个模拟 iPhone iMessage 的聊天界面。读者先看到一条新消息通知，点开后进入聊天，**每点一下出现下一条消息**。

## 效果

-  **新消息通知横幅**：点一下横幅收起，手机聊天界面出现
-  **点一下出一条**：「•••」输入中气泡就是点击按钮，点开后消息淡入弹出
-  **连发效果**：同一步里放多条消息，会间隔 0.5 秒逐条冒出
-  **自动分组间距**：同一人连发间距紧凑，换人说话间距拉开


## 文件

| 文件 | 說明 |
| --- | --- |
| `imessage-skin.paste.css` | Work Skin，整段贴进 AO3，或者直接在AO3workskin选择iMessagelikeworkskin， 我已经发布|
| `imessage-body.paste.html` | 正文范本 |

## 使用方法

### 1. 建立 Work Skin

1. AO3 → 右上角头像 → **Dashboard** → **Skins** → **My Work Skins** → **Create Work Skin**
2. 起个名字，把 `imessage-skin.paste.css` 的全部内容贴进 **CSS** 栏位，保存
或者直接在作品页面选择iMessagelikeworkskin

### 2. 在作品里启用（如果选择自己建立workskin）

发文或编辑作品时，在 **Associations → Select Work Skin** 选择刚才建立的皮肤。

### 3. 贴正文

正文编辑器切到 **HTML** 模式，照 `imessage-body.paste.html` 的结构把对话贴进去。

## HTML 结构
```
</details>
</details>
</div>
</div>
</details>
```

（上面的 `<!-- -->` 只是说明，贴进 AO3 时不要带注释。）

**规则：**

- **所有消息都写在 `<summary>` 里面。** AO3 保存正文时会把其他地方并排的 `<span>` 包进 `<p>`、把换行变成 `<br>`，但不会改动 `<summary>` 的内容
- 每个 `<details class="step">` 是一次点击
- `<summary>` 里的顺序：**输入中气泡（必须第一个）** → 可选的 `time` → 这一步的消息 → 可选的 `read` / `hint`
- 下一步写在这一步的 `</summary>` 后面、`</details>` 前面，一层套一层；有几个 step，最后就要有几个 `</details>`
- 一打开就要显示的消息，放在第一个 `<details class="step" open>` 里，不放输入中气泡
- 不想要通知横幅的话，去掉外层的 `<details class="notify">`、它的 `<summary>` 和对应的 `</details>`，只留 `<div class="phone">`

### Class 速查

| Class | 用途 |
| --- | --- |
| `msg in` | 对方的消息（灰色，靠左） |
| `msg out` | 自己的消息（蓝色，靠右） |
| `time` | 居中的时间分隔小字；放在步骤开头时，会把前后消息分成两组 |
| `read` | 「已读」「已送达」小字，放在自己消息后面 |
| `typing in` / `typing out` | 「•••」输入中气泡，读者点它出下一步 |
| `hint` | 「轻触继续」提示，放在第一步 `<summary>` 的最后 |

气泡尾巴和消息间距都是自动判断的，不用手动加 class。


| 值 | 用途 |
| --- | --- |
| `#0a84ff` | 自己的气泡（蓝色） |
| `#e9e9eb` | 对方的气泡（灰色）、对方的输入中气泡 |
| `#ffffff` | 荧幕底色、自己的文字；**气泡尾巴的遮罩也用它**，换底色时要一起换 |
| `#000000` | 对方的文字、联络人名字 |
| `#f6f6f6` | 顶部联络人栏 |
| `#d1d1d6` | 手机边框、分隔线 |
| `#8e8e93` | 时间、已读、提示等灰色小字 |
| `14px` / `0.4px` | 消息字号 / 字间距（在 `.phone` 里） |
| `3px` / `12px` | 同一人连发的间距 / 换人说话的间距 |


## 参考

- [otwcode/otwarchive](https://github.com/otwcode/otwarchive) — AO3 源码，CSS / HTML 白名单和 work skin 限制
- [AO3 Workskins 合集](https://archiveofourown.org/collections/Workskins)

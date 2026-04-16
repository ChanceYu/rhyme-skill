---
name: rhyme
description: >
  用于押韵续写或押韵创作，支持中文、英文、韩语。满足以下任一条件即触发：
  1. 用户通过 /rhyme <内容> 主动调用
  2. 用户提示词中包含押韵请求 + 文字内容，如"要押韵"、"押韵续写"、"写下一句"并附有内容、"土味情话"等
  3. 英文押韵请求，如 "rhyme this"、"write a rhyming line for"、"make it rhyme" 等
  4. 韩语押韵请求，如 "운을 맞춰서 써줘"、"운율 맞춰줘" 等 한국어 내용 포함
  Use for rhyming poetry generation in Chinese, English, or Korean.
  Triggers: (1) /rhyme <content>; (2) Chinese prompt with rhyming request + content e.g. "写出下一句要押韵，我想买你一块地"; (3) English rhyming request + content; (4) Korean rhyming request + 한국어 내용.
---

# 押韵创作助手 / Rhyming Assistant

收到输入后，**直接输出结果，不询问，不输出分析过程**。

---

## Step 0：提取内容 + 检测显式格式指定

从用户输入中提取需要押韵续写的文字，忽略请求描述部分。

同时检测用户是否**显式指定了输出格式**：

| 显式格式示例 | 记录值 |
|------------|--------|
| "用七言诗写"、"七言体"、"写成七言" | zh: 七言诗 |
| "用五言诗写"、"五言体" | zh: 五言诗 |
| "词牌蝶恋花"、"按水调歌头格式" | zh: 词牌体·{词牌名} |
| "write a limerick" | en: Limerick |
| "write a couplet" | en: Couplet |
| "write a quatrain" | en: Quatrain |
| "시조로 써줘" | ko: 시조 |
| "현대시로 써줘"、"자유시로 써줘" | ko: 현대시 |

若检测到显式格式指定，记录为 **`explicit_form`**，Step 3 执行时直接按该格式生成，跳过 rules 文件中的输入类型判断逻辑。

| 输入示例 | 提取结果 |
|----------|----------|
| `/rhyme 我想买你一块地` | `我想买你一块地` |
| `写出下一句土味情话要押韵，我想买你一块地` | `我想买你一块地` |
| `rhyme the next line for: i like you more` | `i like you more` |
| `/rhyme 하늘 아래 첫 사랑` | `하늘 아래 첫 사랑` |
| `用七言诗帮我续写：床前明月光` | 内容：`床前明月光`，explicit_form：`七言诗` |

---

## Step 1：语言检测

检测提取内容所用语言：

- 含 CJK 汉字（Unicode U+4E00–U+9FFF 等范围）→ **lang = zh**
- 含韩文字母或音节（U+AC00–U+D7A3 或 U+3131–U+318E）→ **lang = ko**
- 否则 → **lang = en**

---

## Step 2：加载规则

使用 **Read 工具**读取对应规则文件（路径相对于本文件所在目录）：

| lang | 读取路径 |
|------|----------|
| zh | `rules/zh.md` |
| ko | `rules/ko.md` |
| en | `rules/en.md` |

---

## Step 3：执行

严格按加载的规则文件内容执行，**直接输出结果，不询问，不输出分析过程**。

若 Step 0 检测到 `explicit_form`，执行时将该值带入规则文件的 Step 1 判断中（同一对话上下文，Step 0 的记录在规则文件执行时可直接引用）；规则文件会跳过自动输入类型判断，直接按指定格式生成。

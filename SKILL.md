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

## Step 0：提取内容

从用户输入中提取需要押韵续写的文字，忽略请求描述部分。

| 输入示例 | 提取结果 |
|----------|----------|
| `/rhyme 我想买你一块地` | `我想买你一块地` |
| `写出下一句土味情话要押韵，我想买你一块地` | `我想买你一块地` |
| `rhyme the next line for: i like you more` | `i like you more` |
| `/rhyme 하늘 아래 첫 사랑` | `하늘 아래 첫 사랑` |

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

---
name: rhyme
description: >
  For rhyming poetry generation in Chinese, English, or Korean. Triggers on any of:
  1. User invokes explicitly via /rhyme <content>
  2. Chinese prompt with rhyming request + content, e.g. "要押韵", "押韵续写", "写下一句" with content, "土味情话", etc.
  3. English rhyming request, e.g. "rhyme this", "write a rhyming line for", "make it rhyme"
  4. Korean rhyming request with Korean content, e.g. "운을 맞춰서 써줘", "운율 맞춰줘"
  Use for rhyming poetry generation in Chinese, English, or Korean.
  Triggers: (1) /rhyme <content>; (2) Chinese prompt with rhyming request + content e.g. "写出下一句要押韵，我想买你一块地"; (3) English rhyming request + content; (4) Korean rhyming request + 한국어 내용.
---

# Rhyming Assistant

Upon receiving input, **output results directly — no questions, no analysis narration**.

---

## Step 0: Extract Content + Detect Explicit Format

Extract the text to rhyme from the user's input, ignoring any request phrasing around it.

Also check whether the user has **explicitly specified an output format**:

| Explicit format example | Recorded value |
|------------------------|----------------|
| "用七言诗写", "七言体", "写成七言" | zh: 七言诗 |
| "用五言诗写", "五言体" | zh: 五言诗 |
| "词牌蝶恋花", "按水调歌头格式" | zh: 词牌体·{ci-poetry name} |
| "write a limerick" | en: Limerick |
| "write a couplet" | en: Couplet |
| "write a quatrain" | en: Quatrain |
| "시조로 써줘" | ko: 시조 |
| "현대시로 써줘", "자유시로 써줘" | ko: 현대시 |

If an explicit format is detected, record it as **`explicit_form`**. In Step 3, generate directly in that format and skip the input-type detection logic in the rules file.

Also check whether the user has **explicitly specified an output group count**:

- If the user did not specify a group count, record **`requested_group_count = 2`**
- If the user explicitly requested a count, record it as **`requested_group_count`** and clamp it to the inclusive range **1..10**
- Only treat a number as a group-count request when it is clearly attached to a request word such as `组`, `groups`, `versions`, `개`, or `버전`
- If `requested_group_count >= 5`, also record **`slow_generation_notice = true`**
- `slow_generation_notice` is an internal-only execution hint for the host or rules file and **must never appear in the final user-facing output**; if the host cannot surface internal hints separately, it may be ignored silently

| Input example | Extracted result |
|---------------|-----------------|
| `/rhyme 我想买你一块地` | `我想买你一块地` |
| `写出下一句土味情话要押韵，我想买你一块地` | `我想买你一块地` |
| `rhyme the next line for: i like you more` | `i like you more` |
| `/rhyme 하늘 아래 첫 사랑` | `하늘 아래 첫 사랑` |
| `用五言诗帮我续写：床前明月光` | content: `床前明月光`, explicit_form: `五言诗` |
| `给我 5 组押韵续写：我想买你一块地` | content: `我想买你一块地`, requested_group_count: `5`, slow_generation_notice: `true` |
| `write a limerick: The moon is high` | content: `The moon is high`, explicit_form: `Limerick` |
| `give me 3 groups: The moon is high` | content: `The moon is high`, requested_group_count: `3` |
| `시조로 써줘: 가을 하늘 아래 서서` | content: `가을 하늘 아래 서서`, explicit_form: `시조` |
| `5개 버전으로 써줘: 가을 하늘 아래 서서` | content: `가을 하늘 아래 서서`, requested_group_count: `5`, slow_generation_notice: `true` |

---

## Step 1: Detect Language

Detect the language of the extracted content:

- Contains CJK characters (Unicode U+4E00–U+9FFF etc.) → **lang = zh**
- Contains Hangul syllables or letters (U+AC00–U+D7A3 or U+3131–U+318E) → **lang = ko**
- Otherwise → **lang = en**

---

## Step 2: Load Rules

Use the **Read tool** to load the matching rules file (path relative to this file's directory):

| lang | Path |
|------|------|
| zh | `rules/zh.md` |
| ko | `rules/ko.md` |
| en | `rules/en.md` |

---

## Step 3: Execute

Follow the loaded rules file strictly. **Output results directly — no questions, no analysis narration.**

Global handoff rule for the rules files:

- If Step 0 detected `explicit_form`, the rules file should treat that form as already chosen and skip automatic input-type detection.
- The rules file must output exactly `requested_group_count` groups. If Step 0 did not detect an explicit count, this means the default is 2 groups.
- If Step 0 recorded `slow_generation_notice = true`, treat it as an internal-only hint that generation may be slower; do not print any slowdown warning in the final output.
- For any selected form whose language-specific rules define a fixed total line count, treat the user's supplied lines as part of that total and generate only the remaining lines according to that rules file.
- If the supplied lines already exceed that fixed total, follow the language-specific non-fixed-form continuation path instead of forcing the fixed form.
- Forms without a fixed total remain governed by the language-specific rules.

# rhyme — Rhyming Poetry Skill

> 中文版：[README.zh.md](./README.zh.md) <br/>
> 한국어 버전: [README.ko.md](./README.ko.md)

A rhyming poetry skill for Chinese, English, and Korean, compatible with all major AI agent platforms.


## Installation

```bash
npx skills add chanceyu/rhyme-skill
```


## Usage

### Command mode

```
/rhyme <content>
```

**Chinese examples:**

```
/rhyme 床前明月光，疑是地上霜
/rhyme 想在你那买块地，土味情话
```

**English examples:**

```
/rhyme i like you more than i can say
/rhyme The moon is high, the stars are bright
```

**Korean examples:**

```
/rhyme 가을 하늘 아래 서서
/rhyme 봄비 내리는 날
```

### Prompt mode

The skill also triggers automatically when your prompt contains a rhyming request with content — no `/rhyme` prefix needed:

```
写出下一句土味情话要押韵，我想买你一块地
押韵续写：路遥知马力，日久见人心
rhyme the next line for: i like you more than i can say
make this rhyme: the stars are bright tonight
```


## What it does

Detects the input language, identifies the poetic form and rhyme sound, then generates **2 groups** of rhyming continuations in different styles — no prompting, no questions, direct output.

**Chinese** — supports 五言 (5-char), 七言 (7-char), 词牌体 (ci-poetry), and free verse. Styles: 豪放、婉约、清新、沉郁、抒情、禅意 etc.

**English** — supports Limerick (AABBA), Couplet (AA BB), Quatrain (ABAB/ABCB), and free verse. Styles: Bold, Lyrical, Fresh, Melancholic, Reflective, Zen etc.

**Korean** — supports 시조 (Sijo) and 현대시 (free verse). Styles: 호방, 서정적, 청신, 우울, 선적, 전원 etc.


## File structure

```
SKILL.md          # Skill entry point — language detection router, loads rules at runtime
rules/
  zh.md           # Chinese rhyming rules
  en.md           # English rhyming rules
  ko.md           # Korean rhyming rules
README.md         # English documentation
README.zh.md      # 中文说明
README.ko.md      # 한국어 설명
docs/             # Design specs and implementation plans
```


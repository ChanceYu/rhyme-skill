# rhyme — 押韵创作 Skill

> English version: [README.md](./README.md) <br/>
> 한국어 버전: [README.ko.md](./README.ko.md)

支持中文、英文、韩语的押韵创作技能，适用于各类 AI Agent 平台。


## 安装

```bash
npx skills add chanceyu/rhyme-skill
```


## 用法

### 命令模式

```
/rhyme <内容>
```

**中文示例：**
```
/rhyme 床前明月光，疑是地上霜
/rhyme 想在你那买块地，土味情话
```

**英文示例：**
```
/rhyme i like you more than i can say
/rhyme The moon is high, the stars are bright
```

**韩语示例：**

```
/rhyme 가을 하늘 아래 서서
/rhyme 봄비 내리는 날
```

### 提示词模式

当提示词中包含押韵请求 + 内容时，技能会自动触发，无需 `/rhyme` 前缀：

```
写出下一句土味情话要押韵，我想买你一块地
押韵续写：路遥知马力，日久见人心
rhyme the next line for: i like you more than i can say
make this rhyme: the stars are bright tonight
```


## 功能说明

自动识别输入语言、判断诗体、提取韵脚，直接输出 **2 组**不同风格的押韵续写，不询问、不解释。

**中文** — 支持五言、七言、词牌体、自由体。风格：豪放、婉约、清新、沉郁、抒情、禅意 等

**英文** — 支持 Limerick（AABBA）、Couplet（AA BB）、Quatrain（ABAB/ABCB）、自由诗。风格：Bold、Lyrical、Fresh、Melancholic、Reflective、Zen 等

**韩语** — 支持시조（時調）和현대시（自由诗）。风格：호방、서정적、청신、우울、선적、전원 等


## 文件结构

```
SKILL.md          # 技能入口 — 语言检测路由，运行时动态加载规则文件
rules/
  zh.md           # 中文押韵规则
  en.md           # 英文押韵规则
  ko.md           # 韩语押韵规则
README.md         # English documentation
README.zh.md      # 中文说明
README.ko.md      # 한국어 설명
docs/             # 设计文档与实现计划
```

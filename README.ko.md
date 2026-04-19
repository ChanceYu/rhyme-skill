# Rhyme Skill

[中文](./README.zh.md) | [English](./README.md)

중국어, 영어, 한국어 시, 가사, 문장의 운율을 지원하는 AI 기반 라임 어시스턴트 스킬.

## 주요 기능

- ✅ 명령 모드(`/rhyme <내용>`)와 자연어 프롬프트 모드 두 가지 방식으로 실행할 수 있습니다
- ✅ 중국어, 영어, 한국어 입력을 자동으로 감지하고 알맞은 규칙 세트로 라우팅합니다
- ✅ 사용자가 시 형식을 명시하면 이를 우선 적용하고, 없을 때만 자동으로 형식을 판별합니다
- ✅ 기본적으로 2개 그룹의 압운 이어쓰기를 생성하며, 요청하면 최대 10개 그룹까지 늘릴 수 있습니다
- ✅ 오언시, 칠언시, 사(词牌体), 리머릭, 대구 형식, 4행시, 시조 같은 언어별 형식을 지원합니다
- ✅ 별도의 확인이나 추가 질문 없이 결과를 바로 출력합니다

## 설치

```bash
npx skills add chanceyu/rhyme-skill
```

## 사용법

### 명령 모드

```
/rhyme <내용>
```

**중국어 예시:**
```
/rhyme 床前明月光，疑是地上霜
/rhyme 想在你那买块地，土味情话
```

**영어 예시:**
```
/rhyme i like you more than i can say
/rhyme The moon is high, the stars are bright
```

**한국어 예시:**
```
/rhyme 가을 하늘 아래 서서
/rhyme 봄비 내리는 날
```

### 프롬프트 모드

프롬프트에 압운 요청과 내용이 함께 들어 있으면 `/rhyme` 접두사 없이도 자동으로 스킬이 발동합니다.

```
写出下一句土味情话要押韵，我想买你一块地
押韵续写：路遥知马力，日久见人心
rhyme the next line for: i like you more than i can say
make this rhyme: the stars are bright tonight
운을 맞춰서 이어 써줘: 가을 하늘 아래 서서
운율 맞춰줘: 봄비 내리는 날
```

## 기능 설명

입력 언어를 감지하고, 시 형식과 운을 판단한 뒤, 압운 이어쓰기를 기본적으로 **2개 그룹** 생성합니다. 사용자가 그룹 수를 명시하면 그 수를 따르며, 최대 **10개 그룹**까지 지원합니다. 질문하지 않고, 설명하지 않으며, 바로 결과를 출력합니다.

**중국어** - 오언시, 칠언시, 사(词牌体), 자유체를 지원합니다. 스타일: 豪放, 婉约, 清新, 沉郁, 抒情, 禅意 등

**영어** - Limerick (AABBA), Couplet (AA BB), Quatrain (ABAB/ABCB), 자유시를 지원합니다. 스타일: Bold, Lyrical, Fresh, Melancholic, Reflective, Zen 등

**한국어** - 시조와 현대시(자유시)를 지원합니다. 스타일: 호방, 서정적, 청신, 우울, 선적, 전원 등

## 파일 구조

```
SKILL.md          # 스킬 진입점 - 언어 감지 라우터, 실행 시 규칙 파일을 동적으로 로드
rules/
  zh.md           # 중국어 압운 규칙
  en.md           # 영어 압운 규칙
  ko.md           # 한국어 압운 규칙
README.md         # English documentation
README.zh.md      # 中文说明
README.ko.md      # 한국어 설명
```

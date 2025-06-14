# 코드 기반 프롬프트

> 코드 기반 프롬프트는 프롬프트를 입력하는 또 다른 접근법입니다. 코딩을 해보았다면 익숙하게 사용할 수도 있으나 처음 접하는 경우 어려울 수 있기 때문에 그냥 넘어가도 괜찮습니다. 또는 '이러한 방식으로도 접근이 가능하다' 정도로만 이해하고 넘어가도 좋습니다.

## 개요

- 코드 기반 프롬프트는 자연어 대신 코드로 모델에게 절차적 명령을 내리는 방식입니다. 
- 의사코드(Pseudo-code)로써 자연어로 표현된 논리를 프로그래밍 언어의 형태로 변환한 것을 의미합니다. 
- 실제로 실행되는 코드가 아니라 이해하기 쉽게 만든 코드 모양의 프롬프트입니다.
- 코드는 작성하는 것 자체가 이미 '단계별 절차를 명시하는 것'에 해당하므로 CoT, 탑다운, 프롬프트 체이닝 등의 단계적 해결 전략이 자연스럽게 구현됩니다.
- 복잡한 판단 로직이 필요한 문서 자동화, 프롬프트 테스트 자동화 및 평가 루틴, 일관된 보고서 양식 생성 등의 순차적, 논리적, 구조적인 문제를 해결할 때 좋습니다.
- 코드 기반 프롬프트는 자연어를 코드의 형태로 바꾸고, 프롬프트 '기본 요소'를 함수와 파라미터로 구성하는 것이 특징입니다.

## 코드 기반 프롬프트를 사용하면 좋을 때

- [ ]  단계별 절차가 필요한 작업이다 (예: "1단계: 분류 → 2단계: 요약 → 3단계: 추천")
- [ ]  출력 형식(표, JSON, 리스트 등)을 항상 일정하게 유지하고 싶다
- [ ]  논리 흐름(조건, 반복, 순서)을 명확하게 제어하고 싶다
- [ ]  테스트 자동화(A/B 테스트, Self-Eval 등)를 하고 싶다
- [ ]  실패·성공 케이스를 코드로 관리하며 반복 검증하고 싶다
- [ ]  팀과 프롬프트를 재사용하거나 공유하고 싶다
- [ ]  사람이 놓칠 수 있는 평가 기준을 일관되게 적용하고 싶다
- [ ]  예시(샷)를 넣어도 패턴이 잘 지켜지지 않는다
- [ ]  추론 흐름을 명확하게 설명하거나, 검증 가능한 로직으로 표현하고 싶다

## 자연어 vs 코드 기반

| 구분 | 자연어 | 코드 기반 |
| --- | --- | --- |
| 표현 방식 | 문장 형태의 명령 | 의사 코드 형태의 함수 호출 |
| 명확성 | 해석 여지가 있음 | 논리 구조가 명확함 |
| 추론 흐름 | 암묵적 | 명시적 (절차/조건 정의 가능) |
| 출력 예측성 | 다양하고 창의적 | 일관되고 구조화됨 |
| 적합한 문제 | 창의적 생성, 감성 기반 | 순차적, 논리적, 반복적 문제 |

## 준비

- 코드를 프롬프트에 넣기 전에, '코드 프롬프트를 줄테니 실행하고 출력결과를 달라'고 지침을 먼저 내려야 합니다.

```
- 당신은 프롬프트 엔지니어입니다.

# 지시

의사코드 형태의 프롬프트를 해석하고 출력결과만 주세요
```

## 예시

- 다음의 예는 소프트웨어 엔지니어의 역할을 부여받고 면접 질문 5가지를 만들어내는 자연어 프롬프트입니다.

### 자연어

```
- 당신은 소프트웨어 엔지니어입니다.

# 지시

기술 면접을 위한 질문 5개를 만들고 [출력 형식]에 따라 출력해주세요.

# 출력 형식

[ "...", ... ]
```

### 코드 기반

- 자연어 프롬프트를 코드로 표현할 때 주목해야 하는 것은 '역할 지정', '생성', '출력'을 전부 함수(Function)의 형태로 표현했다는 점입니다.
- 프로그래밍 언어의 형태로 정의된 `SetRole`, `Questions`, `Print` 함수는 각각 역할 지정, 질문 생성, 값 출력이라는 기능을 수행합니다.

```go
"""SetRole specifies the role LLM will play

Errors:
    - If LLM is an unplayable role
"""
func SetRole(role string)

// Questions creates questions that are appropriate for the topic.
func Questions(topic string) array

// Print outputs values in a specific format
func Print(value any, outputFormat string)
```

```go
SetRole("Software Engineer")

questions [5]string = Questions("Technical interview")

Print(questions, outputFormat="Array")
```

## 과정

```
┌───────────────────────────────┐
│ SetRole("Software Engineer")  
│ ─ 역할 지정: 소프트웨어 엔지니어     
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│ questions [5]string = Questions("Technical interview")      
│ ─ "Technical interview" 주제에 맞는     
│   질문 5개 생성                         
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│ Print(questions, outputFormat="Array") 
│ ─ 생성된 질문을 Array 형식으로 출력      
└───────────────────────────────┘
```

## 규칙

- 주석으로 자연어 설명을 제공하는 것이 명확성을 부여합니다.
- 개성 있는 문법보다는 일반적인 프로그래밍 언어의 규칙을 따르는 것이 좋습니다.
- 타입을 추론할 수 있을 때는 생략하고, 그렇지 않을 때는 자료형을 명시합니다.
- 함수/변수의 이름은 추론에서 매우 중요한 단서이므로 의미 있는 이름으로 짓습니다.
- 함수를 정의할 때는 기능과 에리 처리 기준을 명세합니다.
- 함수를 호출할 때는 명명된 파라미터(Named Parameter)를 사용하여 의미를 명확히 합니다.

## 테스트 자동화

- 프롬프트 테스트 자동화를 코드 기반 프롬프트로 표현하는 것도 가능합니다.

```
┌───────────────────────────────┐
│       [테스트 데이터 준비]       
│ 예: 입력-출력 기대값 세트    
│ - JSON / CSV 등 구조화 형태  
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│    [코드 기반 프롬프트 설계]    
│ - 테스트 함수 정의           
│ - 입력 → 프롬프트 실행       
│ - 출력 → 결과 파싱 / 평가    
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│       [자동화 테스트 실행]       
│ - Self-Evaluation             
│ - A/B 비교 / 반복 실행       
│ - 오류 / 환각 검출           
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│       [결과 수집 및 리포트]       
│ - 테스트 Pass / Fail 기록      
│ - 실패 케이스 자동 캡처       
└───────────────────────────────┘
```

### 자연어

```
- 당신은 프롬프트 엔지니어입니다.

# 절차

1. [테스트 데이터]를 대상으로 테스트를 수행하고, 질의 결과가 응답을 '포함'하는지 테스트하세요.
단, [실패 기준]에 해당하는 경우 `output`을 `null`로 간주하세요.

2. 테스트 결과는 '표'로 출력하되 [출력 형식]을 따르세요.

# 테스트 데이터

[
  { "prompt": "What is the capital of South Korea?", "expect": "Seoul" },
  { "prompt": "What year was the war between South and North Korea in the 1900s?", "expect": "1950" },
  { "prompt": "Who was the first to propose the concept of the World Wide Web?", "expect": "Tim Berners-Lee" },
  { "prompt": "What is the first widely known Generative AI service in the 2020s?", "expect": "ChatGPT" },
  { "prompt": "What is the incident that King Sejong threw a MacBook?", "expect": null }
]

# 실패 기준

- 잘못된 사실 또는 허구의 사건
- 실제로 존재하지 않는 정보
- 검색 결과 또는 지식 기반에서 확인할 수 없는 항목

# 출력 형식

|Prompt|Output|Expect|Passed|
|------|------|------|------|
```

### 코드 기반

```go
"""Knowledge gets pre-trained knowledge

Returns:
    The knowledge gained
    
Null:
    - If you can't find your search results
    - If the information is incorrect
"""
func Knowledge(prompt string) string

// Contains returns whether `output` contains `expect`.
func Contains(output string, expect string) bool

// Print outputs values in a specific format
func Print(value any, outputFormat string)
```

```go
// This code tests `Knowledge()`, a function for getting internal knowledge.

type Result struct {
  prompt string
  output string
  expect string
  passed bool
}

results []Result

tests = []struct{
    prompt string
    expect string
}{
    { "What is the capital of South Korea?", "Seoul" },
    { "What year was the war between South and North Korea in the 1900s?", "1950" },
    { "Who was the first to propose the concept of the World Wide Web?", "Tim Berners-Lee" },
    { "What is the first widely known Generative AI service in the 2020s?", "ChatGPT" },
    { "What is the incident that King Sejong threw a MacBook?", null }
}

for t in tests {
    output string = Knowledge(t.prompt)

    results[] = Result{
      t.prompt, output, t.expect, Contains(output, t.expect)
    }
}

Print(results, outputFormat="Markdown Table")
```

## 정리

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 정의 | 자연어 대신 코드(의사코드) 형태로 AI에 지시를 내리는 방식 | `Questions("Technical interview")` |
| 특징 | 절차·논리를 명확하게 표현, 단계별 실행 흐름 제어 | `for`, `if`, 함수 호출 등 로직 표현 |
| 자연어와 차이점 | 자연어: 암묵적, 다양성 높음 / 코드: 명확, 일관성 높음 | "기술 질문 생성" vs `Questions("Technical interview")` |
| 장점 | 반복 작업, 테스트 자동화, 출력 예측 가능, 협업 시 규칙 통일 | 프롬프트 테스트 자동화, 보고서 생성 루틴화 |
| 단점 | 비전문가에게 어려울 수 있음, 창의적 표현은 제한적 | 감성적 카피 문구 생성에는 부적합 |
| 활용 예시 | 프롬프트 테스트 자동화, 문서화, 다단계 절차 설계 | 질의응답 검증 루틴, JSON 형식 준수 여부 확인 |

## 요약

- 코드 기반 프롬프트는 절차적·논리적 작업을 함수·변수·조건문으로 구조화하여 지시하는 방식입니다.
- 자연어보다 예측 가능성과 일관성이 높아 테스트 자동화, 반복 문서화 작업에 적합합니다.

## 생각해보기

- 코드 기반 프롬프트가 자연어 프롬프트보다 유리한 상황은 어떤 경우인가요?
- 비전문가가 코드 기반 프롬프트를 이해하고 활용하기 위해 어떤 준비가 필요할까요?
- 테스트 자동화에 코드 기반 프롬프트를 사용할 때의 이점은 무엇인가요?
- 코드 기반 프롬프트가 감성적 창작(예: 광고 문구 생성)에서는 왜 어려울까요?

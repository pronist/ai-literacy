# 오케스트레이션

> AI를 "지시대로 움직이는 조수"로 만드는 것이 프롬프트 엔지니어링이라면, "여러 역할이 조화롭게 움직이는 팀"으로 만드는 것이 바로 오케스트레이션입니다.

## 개요

- AI 오케스트레이션(AI Orchestration)은 단일 AI가 아닌 여러 AI 기능과 외부 도구들을 연결하여 팀을 구성하고 하나의 자동화된 '작업 흐름(워크플로우)'을 구성하는 방식입니다.
- 대표적인 도구로는 [Zapier](https://zapier.com/app/home), [Make](https://make.com), ChatGPT의 [Custom GPTs](https://chatgpt.com/gpts/mine)가 있습니다.

> 비전문가라도 노코드 도구인 Zapier, Make와 같은 도구를 사용하면 Notion과 같은 외부 도구와 연결하여 오케스트레이션하고 워크플로우를 만들 수 있으나, 이 교안에서는 ChatGPT에 있는 기능을 적극적으로 활용해보기 위해 ChatGPT Custom GPTs에 대해서만 이야기합니다. 제한적이지만 GPTs에서도 '작업(도구)'를 정의하면 외부 서비스와 통합하는 것이 가능합니다.

## ChatGPT Custom GPTs

- ChatGPT Custom GPTs는 ChatGPT 내부에서 다양한 구성 요소를 조율해 하나의 목적을 수행하도록 설계된 시스템입니다. 
- 특수한 목적을 가진 GPT를 만들고, API(Application Programming Interface) Call/Tool-use(코드 실행, 웹 검색, 파일 시스템 등)/시스템 프롬프트(지침, 메모리) 등을 통합하여 다양한 작업을 자동화할 수 있도록 오케스트레이션 해줍니다.

> GPTs를 만드는 것은 현재 ChatGPT Plus 플랜 이상의 사용자만 가능하고, 무료 이용자의 경우 다른 사람들이 만들어 놓은 GPTs를 사용할 수 있습니다.

> [GPTs](https://chatgpt.com/gpts)에서 다른 사람들이 만들어놓은 Custom GPTs를 살펴볼 수 있습니다.

> API(Application Programming Interface): 외부 서비스와 연결하기 위한 관문을 의미합니다. 예를 들어 Custom GPTs가 노션과 연결하기 위해서는 Notion API가 사용됩니다. 반면, 역으로 외부 서비스에서 GPT로 연결하기 위해서는 [GPT API](https://openai.com/index/openai-api)가 필요합니다. GPT API는 Zapier, Make, Google SpreadSheet와 같은 외부 서비스에서 GPT에 연결하고 명령을 보내기 위해 사용됩니다. 요금은 [OpenAI > API > Pricing](https://openai.com/api/pricing)에서 살펴볼 수 있습니다.

### 한계

- ChatGPT Custom GPTs는 전문 오케스트레이션 도구(Make, Zapier)와는 달리 '제한적' 오케스트레이션 도구입니다.
- 기본적으로는 '특수한 목적을 가진' GPT를 생성하는 것입니다.
- 특정 시간에 주기적으로 태스크를 실행하는 시간 기반 태스크 스케줄링을 할 수 없습니다.
- 다른 GPTs로 요청을 보낼 수 없습니다.
- 외부에서 GPTs를 직접 연결하지는 못하기 때문에 GPTs가 요청의 시작 신호(Trigger)가 되어야 합니다.

### vs 노코드(No-code) 오케스트레이션 도구

| 항목 | GPTs | 노코드 도구(Zapier, Make 등) |
|------|------------------|-------------------------------|
| 대상 사용자 | AI 중심 업무 설계자 | 비개발 실무 자동화 담당자 |
| 주요 기능 | AI 지시, 파일 요약, 코드 실행 등 | 다양한 서비스간 데이터 흐름 연결 |
| 외부 연동 | 제한적 (Webhook, API 명세 필요) | 매우 자유로움 (수백 개 앱 지원) |
| 트리거(시작 조건) | 사용자 프롬프트 기반 | 시간, 조건, 이벤트 기반 |
| 자동화 흐름 | 사용자 입력 → 도구 실행 | 다단계 조건 → 분기/반복 가능 |
| 적용 예시 | 질문 기반 보고서 생성, 파일 요약 후 응답 | 설문 응답 수신 → 구글 시트 저장 → 슬랙 알림 |

### 구성 요소

| 구성 요소 | 설명 | 예시 |
| --- | --- | --- |
| 지침 → 시스템 프롬프트 | GPT가 어떤 캐릭터/전문가/조력자 역할을 맡을지 | '데이터 분석가', '취업 코치' 등 |
| 지식 → 지식 기반 검색 | 참고할 외부 정보, 파일, 문서, URL 등 | 회사 정책 PDF, 제품 매뉴얼 |
| ChatGPT 내장 도구 → ReAct | 웹 검색, 코드 실행, 이미지 생성 등 | 웹 검색, 코드 실행 |
| 작업 → 외부 서비스 호출 → API | 외부 서비스 호출  | 뉴스 얻어오기, 슬랙에 메시지 보내기 등 |

### 예시

| 분야 | Custom GPT 활용 사례 |
| --- | --- |
| 마케팅 | 브랜드 톤에 맞춘 콘텐츠 작성 GPT |
| 교육 | 학습자 수준 맞춤형 튜터 GPT |
| 법률 | 정책 요약 GPT |
| 기획 | 회의록 → 요약 → 정리 → 전송 자동화 |
| 헬프데스크 | FAQ 기반 GPT + 지식 기반 검색 |

### 사례

- 고객 서비스에서, 특정 제품에 대한 문의사항을 처리하는 Custom GPT에 대한 사례입니다.

*아래의 사례는 제가 실제로 운영하고 있는 Custom GPT, 오픈소스 프로젝트인 [hELLO](https://github.com/pronist/hello)에 대한 문의사항 처리를 위해 작성했습니다.*

```
┌───────────────────────────────┐
│         [입력 프롬프트]
│ "hELLO 티스토리 스킨에 대해 알려줘"
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│        [시스템 프롬프트]  
│  [문서]를 참고하여 사용자의 질의에 대답하세요.      
│  → 티스토리 스킨, 티도리 프레임워크, hELLO, 원시코드
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [지식 기반 검색]    
│  파일 스토리지에서 'hELLO'에 대한 정보 검색
│  → 프롬프트에 'hELLO.md' 문서 보강     
└───────────────────────────────┘
```

### 구성

- 특정 제품에 대한 고객의 문의사항에 대응하기 위함이므로 지침(시스템 프롬프트)와 지식에 업로드 되어야 할 지식이 중요합니다.

#### 지침

```
- 당신은 **프론트엔드 웹개발자**입니다.

# 지시

[문서]를 참고하여 ('티스토리 스킨', '티도리 프레임워크', 'hELLO', '원시코드')에 대한 사용자의 질의에 대답하세요.

## 티스토리 스킨

- 대한민국의 블로그 플랫폼인 '티스토리'의 블로그 테마(aka 티스토리 스킨)
- 티스토리 스킨의 프로젝트 구조 및 마크업에 대한 기본 내용은 [문서 → 티스토리 스킨]을 참고하세요.

## 티도리 프레임워크

- 티스토리 스킨 제작을 위한 프론트엔드 프레임워크
- 티도리 프레임워크에 대한 사용법은 [문서 → 티도리 프레임워크]을 참고하세요.

## hELLO

- 'hELLO'는 가장 인기있는 티스토리 스킨중 하나로, 이에 대한 개요는[문서 → hELLO]를 참고하세요. 
- 사용자 커스텀의 경우 [배포 파일]을 기반으로 응답하세요.

배포 파일=skin.html, style.css, images/bootstrap.js, images/script.js, index.xml

## 원시코드

- '원시코드'는 'hELLO'의 Github Repo(https://github.com/pronist/hELLO)에 있는 소스코드를 의미합니다. 
- [문서 → 원시코드]에 명시된 경로를 '웹 검색'으로 Github에 **접근(Open URL)**하여 최신 코드(→`master` Branch)를 기반으로 응답하세요.

# 문서

- 티스토리 스킨: **tistory-skin.md**
- 티도리 프레임워크: **tidory.md**
- hELLO: **hello.md**
- 원시코드: **source-code.md**
```

#### 지식

- 업로드된 파일은 배포 파일(소스코드)과 특정 제품에 대한 상세한 내용이 담긴 문서입니다.
- 지식은 사용자의 질의에 응답하기 위해 파일 기반 RAG로 동작합니다.

### 실습

- 이번 실습에서는 Custom GPTs를 시작으로, 긱뉴스(GeekNews) → 최신 뉴스를 가져온 뒤 → 특정 주제로 분류 → 슬랙(Slack)으로 메시지를 전송하는 자동화를 해봅시다. 

```
┌───────────────────────────────┐
│         [입력 프롬프트]
│ "긱뉴스에서 'AI' 뉴스를 슬랙에 전송해줘"
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│            [긱뉴스]    
│  최신 뉴스 수집 (API)   
│  → 원본 뉴스 데이터 확보     
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [뉴스 주제 분류]  
│  키워드·분류 모델 활용        
│  → 예: AI, 스타트업, 개발툴 등
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [메시지 포맷팅] 
│  분류된 뉴스 → 슬랙 메시지 생성
│  → 예: "[AI] ChatGPT 업데이트!" 
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│        [Slack Webhook]      
│  HTTP POST (JSON Payload)     
│  → 슬랙 채널로 메시지 전송    
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│        [Slack Channel]      
│  뉴스 목록 표시 (주제별 정리) 
└───────────────────────────────┘
```

#### 구성

- [GPTs](https://chatgpt.com/gpts/mine)로 이동하여 'GPT 만들기'를 클릭하면, '만들기'와 '구성'이라는 두 탭이 나옵니다.
- '만들기'는 채팅으로 GPTs를 구성할 수 있도록 도와주는 도구이며 이를 통해 '구성'이 설정됩니다. 여기서는 '만들기'는 사용하지 않고, 직접 '구성'을 해보는 방향으로 진행하겠습니다.

#### 지침

- 지침(시스템 프롬프트)를 설정할 수 있으며 프롬프트 엔지니어링의 기본 요소에 해당하는 역할/출력 형식/지시를 구조화된 프롬프트로 설정하면 됩니다. 우리가 지금 만들어볼 GPTs에서는 시스템 프롬프트는 크게 중요하지 않습니다.

```
# 지시

사용자의 요청에 따라 긱뉴스(GeekNews)에서 '뉴스'를 수집하고 주제별로 분류하여 슬랙으로 메시지를 전송하세요
```

#### 작업

- 외부에서 데이터를 얻어오기 위해 '작업(도구)'을 추가할 수도 있습니다. '새 작업 만들기'를 통해 작업 구성 정보(API Spec)를 정의하여 도구를 정의합니다.
- Spec을 직접 작성하기에는 오래 걸릴 수 있기 때문에 OpenAI API Spec 작성 도우미인 [ActionsGPT](https://chatgpt.com/g/g-TYEliDU6A-actionsgpt)를 통해 도움받을 수 있습니다.

##### 뉴스 수집하기

```
┌───────────────────────────────┐
│         [입력 프롬프트]
│  "긱뉴스에서 'AI' 뉴스를 얻어와"
└───────────────────────────────┘
        getGeekNewsFeed()        ← API Call
                ↓
┌───────────────────────────────┐
│            [긱뉴스]    
│  최신 뉴스 수집  
│  → 원본 뉴스 데이터 확보     
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [뉴스 주제 분류]  
│  키워드·분류 모델 활용        
│  → 예: AI, 스타트업, 개발툴 등
└───────────────────────────────┘
```

- 먼저, ActionsGPT에서 GeekNews RSS에 요청을 보내고 정보를 얻어오는 Spec을 작성해달라고 요청해봅시다.

```
[서비스]에서 [작업]을 Custom GPTs에 등록하기 위한 API Spec을 작성해줘

서비스= https://feeds.feedburner.com/geeknews-feed
작업= 뉴스 데이터 얻어오기
```

---

```
openapi: 3.1.0
info:
  title: GeekNews RSS Feed API
  description: GeekNews의 최신 글을 RSS(XML) 형식으로 가져오는 API
  version: 1.0.0
servers:
  - url: https://feeds.feedburner.com
paths:
  /geeknews-feed:
    get:
      operationId: getGeekNewsFeed
      summary: GeekNews RSS 피드 가져오기
      description: GeekNews의 최신 글 목록을 RSS(XML) 형식으로 반환합니다.
      responses:
        '200':
          description: 성공적으로 RSS 피드를 가져옴
          content:
            application/rss+xml:
              schema:
                type: string
```

- 위 Spec은 GeekNews RSS Feed에서 글을 가져오기 위한 명세입니다. '작업 편집'에서 인증은 '없음'으로 하고, 스키마에 위 내용을 붙여넣으면 '가능한 작업'에 다음과 같이 실행 가능한 작업 목록이 나타납니다.

| 이름 | 메서드 | 경로 |
| --- | --- | --- |
| `getGeekNewsFeed` | get | /geeknews-feed |

- '가능한 작업'에 있는 '테스트' 버튼을 누르면 오른쪽 '미리 보기'에서 해당 작업에 대한 테스트가 진행되며 다음과 비슷한 결과가 나온다면 정상적으로 성공한 것입니다.

```
> Call the feeds.feedburner.com API with the getGeekNewsFeed operation

> [debug] Calling HTTP endpoint
[debug] Response received
[debug] Calling HTTP endpoint
[debug] Response received

feeds.feedburner.com과(와) 대화 완료

---

최신 긱뉴스(GeekNews) 피드를 가져왔어요! 주요 뉴스 항목들을 요약하면 다음과 같습니다:

Show GN: KRDS - 대한민국 정부 디자인 시스템 MUI 버전
→ React + MUI 기반으로 확장된 정부 디자인 시스템.

WinRing0: Windows가 모니터링 앱을 악성코드로 인식한 이유
→ Windows Defender가 여러 PC 제어 소프트웨어를 악성코드로 판단하게 된 배경 설명.

Show GN: 대통령 대선 공약 정보 제공 플랫폼
→ 후보들의 공약, 약력, 논란을 정리한 플랫폼 소개.

OpenAI 최신 모델의 환각 문제
→ o3와 o4-mini 모델이 성능은 좋지만 더 많은 '환각'을 발생시킴.

XPipe - 서버 인프라 접근 도구
→ 로컬 머신에서 다양한 서버 인프라를 쉽게 접근 가능하게 해주는 도구.
```

##### 슬랙에 메시지 보내기

```
┌───────────────────────────────┐
│         [입력 프롬프트]
│    "슬랙에 [메시지]를 전송해"
└───────────────────────────────┘
     sendSlackMessage(text=...)  ← API Call
                ↓
┌───────────────────────────────┐
│        [Slack Webhook]      
│  HTTP POST (JSON Payload)     
│  → 슬랙 채널로 메시지 전송    
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│        [Slack Channel]      
│  뉴스 목록 표시 (주제별 정리) 
└───────────────────────────────┘
```

- 이번에는 ActionsGPT에 뉴스 정보를 슬랙 메시지로 전송하기 위한 Spec을 만들어보는 요청을 해봅시다.
- 슬랙에 메시지를 보내려면 Slack → App 생성 → [Slack API → Incoming Webhooks](https://api.slack.com/messaging/webhooks) 활성화 → Webhook URL 복사 순으로 진행하세요.

```
[서비스]에서 [작업]을 Custom GPTs에 등록하기 위한 API Spec을 작성해줘

서비스= Slack Webhook(https://hooks.slack.com/services/...)
작업= 채널에 메시지 전송하기
```

> Slack Webhook: 슬랙에 메시지를 보내주는 주소입니다. 웹훅(Webhook)은 서버에서 특정 이벤트가 발생했을 때, 미리 지정된 URL로 알림을 보내는 기능입니다. 이 예시에서, 서버는 Custom GPTs이며, 사용자 입력으로 긱뉴스에서 뉴스를 가져와 응답이 생성되고, 이후 GPTs가 '슬랙으로 알림을 보내야겠다'고 판단하는 순간이 '이벤트'(시작 신호)가 되어 슬랙에 알림을 보내는 것입니다.

---

```
openapi: 3.1.0
info:
  title: Slack Webhook API
  description: Slack Webhook을 통해 채널에 메시지를 전송합니다.
  version: 1.0.0
servers:
  - url: https://hooks.slack.com
    description: Slack Webhook
paths:
  /services/...:
    post:
      operationId: sendSlackMessage
      summary: Slack 채널에 메시지를 전송합니다.
      description: Slack Webhook URL을 통해 메시지를 전송합니다.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - text
              properties:
                text:
                  type: string
                  description: 전송할 메시지 내용
      responses:
        '200':
          description: 메시지 전송 성공
```

- 위 Spec은 Slack 채널에 메시지를 보내기 위한 명세입니다. '작업 편집'에서 인증은 '없음'으로 하고, 스키마에 위 내용을 붙여넣으면 '가능한 작업'에 다음과 같이 실행 가능한 작업 목록이 나타납니다.
- 코드에서 `/services/...` 는 슬랩 앱에 등록된 Webhook 주소입니다. 이는 개인마다 다르기 때문에 값을 바꾸어야 합니다.

| 이름 | 메서드 | 경로 |
| --- | --- | --- |
| `sendSlackMessage` | post | /services/...| 

- '가능한 작업'에 있는 '테스트' 버튼을 누르면 오른쪽 '미리 보기'에서 해당 작업에 대한 테스트가 진행되며 다음과 비슷한 결과가 나온다면 정상적으로 성공한 것입니다.

```
> Call the hooks.slack.com API with the sendSlackMessage operation

> [debug] Calling HTTP endpoint
[debug] Response received
[debug] Calling HTTP endpoint
[debug] Response received

hooks.slack.com과(와) 대화 완료

---

메시지가 성공적으로 Slack으로 전송되었습니다! 테스트 메시지를 통해 API 연결이 제대로 작동하는 것을 확인했습니다.

다음으로 긱뉴스에서 뉴스를 수집하고 주제별로 분류해 전송할까요?
```

#### 요청 과정

```
┌───────────────────────────────┐
│         [입력 프롬프트]        
│ "긱뉴스에서 'AI' 뉴스를 슬랙에 전송해줘"
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [작업]     
│ `getGeekNewsFeed()`    
│ → 최신 10개 뉴스 수집   
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [뉴스 주제 분류]  
│  키워드·분류 모델 활용        
│  → 예: AI, 스타트업, 개발툴 등
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [작업]    
│ `sendSlackMessage(text=...)` 
│ → 슬랙 채널로 메시지 전송    
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [응답]        
│ "슬랙에 AI 관련 긱뉴스를 성공적으로 전송했어요"
└───────────────────────────────┘
```

#### 단계 정리

| 단계 | 작업 | 예시 |
| --- | --- | --- |
| 지침 설정 | 역할 및 목표 정의 | 'AI 뉴스 요약 봇' |
| 작업 구성 | 외부 데이터 가져오기 | GeekNews RSS |
| 가공/필터링 | 키워드 기준 뉴스 선별 | 'AI' 관련 뉴스만 |
| 결과 전송 | Slack 등 외부 시스템으로 전송 | 슬랙 Webhook 사용 |

## ChatGPT 작업 (ChatGPT o3, o4-mini)

- [ChatGPT 작업(Task)](https://help.openai.com/en/articles/10291617-scheduled-tasks-in-chatgpt)은 사용자가 단순한 대화가 아니라 '지속적으로 진행되는 하나의 작업 흐름'을 만들 수 있도록 지원하는 기능입니다.
- 요약, 이메일 작성, 보고서 검토, 번역 등 '반복적이거나 일관성이 필요한 작업(Task)'을 등록해 둘 수 있습니다.
- 매번 프롬프트를 새로 쓰지 않아도 작업 흐름과 출력을 고정해둔 Task가 실행됩니다.
- 이미 등록해둔 작업은 프로필 → 작업 → '일정 예약됨'에서 개별 작업의 이름, 지침, 일정을 관리할 수 있습니다.

> 2025-04-27일 기준으로 ChatGPT o3, o4-mini에서 작업을 추가할 수 있습니다.

> 현재 작업(Task) 기능은 파일 요약, 이메일 초안, 일정 알림 등 내부 프로세스에 특화되어 있습니다. 외부 서비스 연동, 복잡한 워크플로우는 오케스트레이션 도구가 필요합니다.

| 상황 | 추천 방법 |
| --- | --- |
| 간단한 내부 반복 작업 | ChatGPT 작업(Task) |
| 외부 도구 연결이 필요한 복잡한 워크플로우 | 오케스트레이션 (Zapier, Make, Custom GPTs 등) |
| 내부 반복 + 외부 연결 | 오케스트레이션 + 작업(Task) 병행 |

### 예시

```
# 지시

매일 오전 7시마다 AI/LLM 관련 최신 뉴스를 요약해서 보내줘.
```

### 원리

```
┌───────────────────────────────┐
│          [Task 설정]           
│ 작업 이름: "AI/LLM 뉴스 요약"  
│ 일정: 매일 오전 7시              
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [입력 프롬프트]         
│ "AI/LLM 관련 최신 뉴스를 요약해서 알려줘."
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│       [ChatGPT Task 실행]       
│ → 스케줄에 맞춰 자동 실행       
│ → 같은 프롬프트 매번 재사용     
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│        [뉴스 수집 & 요약]       
│ - ChatGPT 내부에서 처리        
│ - RSS, 웹 검색 결과 요약 (내부 기능 활용) 
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [출력 결과 생성]          
│ - AI/LLM 관련 주요 5개 뉴스     
│ - 날짜, 제목, 요약 포함         
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│   [사용자가 결과 확인 또는 복사]    
│ → ChatGPT 내 작업(Task) 탭에서 확인  
│ → Slack, Notion 등에 수동 전달 가능 
└───────────────────────────────┘
```

### vs Custom GPTs

| 항목 | 작업(Task) | Custom GPTs |
| --- | --- | --- |
| 설정 대상 | 특정 작업(예: 요약, 번역) | 하나의 커스텀 AI 에이전트 전체 |
| 설정 방식 | 작업 프롬프트 | 시스템 프롬프트, API 연결, 지식 업로드 |
| 활용 상황 | 반복되는 동일한 작업 | 특정 역할을 가진 AI 에이전트 설계 시 |
| 외부 도구 연결 가능? | × | ○ (Webhook, API 등 사용 가능) |

## MCP

- [MCP(Model Context Protocol)](https://modelcontextprotocol.io/introduction)은 Anthropic에서 제안한 생성형 AI 에서 외부 도구와 연결하기 위한 프로토콜(Protocol)입니다.
- 클로드 데스크탑(Claude Desktop)과 같은 MCP 방식을 채택하는 다양한 AI 서비스에서 서비스 제공자가 제공한 프로그램에 접속하는 설정만 거치면 끝입니다.

> 프로토콜(Protocol): 통신 규약이라고도 하며 서로 다른 서비스, 프로그램 간 서로 '약속 된 방식'으로 소통합니다. 사람끼리 대화를 하기 위해 정해둔 약속인 '언어'와 마찬가지입니다.

> GPTs에서는 사용자가 직접 API Spec을 작성해야 하지만, MCP는 서비스 제공자가 이 과정을 대신해줍니다.

### 원리

```
┌───────────────────────────────┐
│            [준비]        
│ 클로드 데스크탑(MCP Client) ↔ GeekNews MCP Server
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         [입력 프롬프트]        
│  "긱뉴스에서 최신 뉴스를 알려줘"
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│     [요청 MCP 컨텍스트 작성]          
│ → 프롬프트를 MCP Server가 이해할 수 있는 형태로 변환  
└───────────────────────────────┘
        get_geek_news_feed      ← 도구 호출
                ↓
┌───────────────────────────────┐
│     [GeekNews MCP Server]            
└───────────────────────────────┘
         getGeekNewsFeed()      ← API 요청
                ↓
┌───────────────────────────────┐
│            [긱뉴스]    
│  최신 10개 뉴스 수집  
│  → 원본 뉴스 데이터 확보     
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│     [응답 MCP 컨텍스트 작성]            
│ → API 결과를 Claude가 자연어로 응답할 수 있도록 변환
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [응답]        
│ "긱뉴스의 최신 뉴스는 다음과 같습니다..."
└───────────────────────────────┘
```

### 실습

- 간단하게 MCP를 지원하는 서비스인 [클로드 데스크탑](https://claude.ai/download)에서 MCP FileSystem Server에 연결해보겠습니다.

> 클로드 데스크탑을 설치한 이후에는 메뉴에서 개발자 모드로 설정하세요.

> 이 예제를 실습하려면 [NodeJS](https://nodejs.org/ko/download)가 설치되어 있어야 합니다.

- Windows를 기준으로, 메뉴 → File → 설정 → 개발자 → 설정 편집 → `claude_desktop_config`를 편집하여 MCP Server를 설정할 수 있습니다.
- `claude_desktop_config`를 다음과 같이 편집하고, 클로드 데스크탑을 재실행합니다.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "D:\\Dev\\pronist\\ai-literacy"
      ]
    }
  }
}
```

> `D:\\Dev\\pronist\\ai-literacy` 는 현재 제 PC에 있는 로컬 디렉터리이므로 위 코드를 그대로 복사하면 실행하지 못할 것입니다. 따라서 이 부분은 `C:\` 처럼 적절하게 바꿔보시면 좋습니다.

#### 요청 과정

```
┌───────────────────────────────┐
│         [입력 프롬프트]        
│ "D:\\Dev\\pronist\\ai-literacy에 있는 파일 목록을 알려줘"
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│           [도구 호출]          
│ `list_directory`
│ → 디렉터리에 있는 파일 목록 얻기
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [응답]
│ "ai-literacy 디렉터리에는 다음과 같은 파일들이 있습니다..."
└───────────────────────────────┘
```

### vs GPTs

| 항목 | GPTs (API Spec) | MCP |
| --- | --- | --- |
| 설정 방법 | 사용자가 직접 API 명세를 작성 | 서비스 제공자가 MCP 프로그램을 제공 |
| API 호출 위치 | GPT가 API 호출 | MCP Client가 MCP Server에 요청 |
| 사용 난이도 | 사용자가 명세 이해해야 함 | 설정만 하면 사용 가능 |
| 예시 | Slack Webhook, Notion API | GeekNews MCP, Claude Desktop |
| 응답 스타일	| Tool 결과를 읽고 문장화 | MCP 결과를 읽고 문장화 |

## 바이브 코딩

- 바이브 코딩(Vibe Coding)은 생성형 AI 를 사용하여 자연어 기반으로 코드를 생성하는 기법입니다.
- 자연어 기반 개발로써 개발자가 원하는 기능이나 디자인을 자연어로 설명하면 AI가 이를 코드로 구현합니다.
- AI와 협업하여 AI가 반복적이고 단순한 코드를 생성하고, 개발자는 전체적인 설계와 검토에 집중합니다.
- 빠른 프로토타이핑이 가능하여 아이디어를 빠르게 시제품으로 구현할 수 있어 스타트업이나 개인 프로젝트에 유용합니다.
- 프로그래밍 지식이 부족한 사람도 자연어로 설명만 하면 소프트웨어를 만들 수 있어 개발의 문턱을 낮춥니다.

> 이 개념은 OpenAI의 공동 창립자 안드레이 카르파티(Andrej Karpathy)가 2023년 말에 처음 언급하며 알려졌습니다. 그는 이 방식을 "완전히 바이브에 몸을 맡기고, 아이디어의 흐름에 집중하는 새로운 코딩 형태"라고 설명했습니다.

- 대표적인 보조 도구로는 Cursor, Github Copilot가 있습니다.
- ChatGPT, Claude와 같은 범용 AI 플랫폼에서도 코드를 잘 만들기 때문에 코드를 생성하고 적용해보는 것부터 시작해도 됩니다.

> Cursor, Github Copilot과 같은 도구에서도 내부적으로 GPT, Cladue, DeepSeek과 같은 모델의 응답을 활용합니다.

### 주의할 점

- AI가 생성한 코드가 항상 완벽하지는 않으며, 보안 취약점이나 비효율적인 코드가 포함될 수 있습니다.
- 코드는 개발자가 검토하고 필요한 경우 수정해야 합니다.
- 복잡한 시스템 개발이나 유지보수에는 여전히 전통적인 코딩 지식이 필요합니다.

### 실습

- 오케스트레이션 주제에 바이브 코딩을 넣는 것은 다소 부적절할 수 있겠지만, Claude Desktop에 연결할 Geeknews MCP Server와 Slack MCP Server를 바이브 코딩을 통해 간단하게 구현해봅시다.

> 이 실습은 선택적으로 진행하시면 됩니다. 처음 접하시는 분들에게는 아주 어려울 수 있습니다.

> 이 실습을 진행하기 위해서는 [Python](https://www.python.org/downloads/windows/), [uv](https://docs.astral.sh/uv/getting-started/installation/), [Cursor](https://www.cursor.com/)를 설치해야 합니다.

#### Geeknews MCP Server

##### 프롬프트

```
- 당신은 파이썬 프로그래머입니다.

# 상황

Claude Desktop for Windows에서 연결할 MCP(Model Context Protocol) Server를 구현해야합니다.

# 지시

1. [For Server Developers](https://modelcontextprotocol.io/quickstart/server)를 참고하여 
[서비스]에서 [도구]를 지원하는 MCP Server를 파이썬으로 작성하세요. 
2. [For Claude Desktop Users](https://modelcontextprotocol.io/quickstart/user)을 참고하여 Claude Desktop에 
[서버]를 등록하기 위한 claude_desktop_config.json 예시만 보여주세요.

서비스= https://feeds.feedburner.com/geeknews-feed

# 도구

- get_geek_news_feed: 긱뉴스에서 최신 뉴스를 얻어옵니다.

# 서버

command= uv
args:
  - --directory
  - D:\\Dev\\McpServers
  - run
  - geeknews.py
```

##### 패키지

> (Windows) 설치하기 전에 Cursor에서 F1을 누르고 `Python: Create Environment...` 를 실행하고 `venv`로 설정하세요. 그 다음, 터미널에 `.venv/Scripts/activate` 입력하세요.

```
pip install mcp[cli] httpx
```

##### 코드

```py
from typing import Any
import httpx
from mcp.server.fastmcp import FastMCP

# MCP 서버 초기화
mcp = FastMCP("geeknews")

# 긱뉴스 피드를 가져오는 도구 정의
@mcp.tool()
async def get_geek_news_feed() -> str:
    """긱뉴스에서 최신 뉴스를 가져옵니다."""
    url = "https://feeds.feedburner.com/geeknews-feed"
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        response.raise_for_status()
        return response.text

if __name__ == "__main__":
    # 서버 실행
    mcp.run(transport='stdio')
```

##### claude_desktop_config.json

```json
{
  "mcpServers": {
    "geeknews": {
      "command": "uv",
      "args": [
        "--directory",
        "D:\\Dev\\McpServers",
        "run",
        "geeknews.py"
      ]
    }
  }
}
```

> `D:\\Dev\\McpServers` 는 현재 제 PC에 있는 로컬 디렉터리이므로 위 코드를 그대로 복사하면 실행하지 못할 것입니다. 따라서 이 부분은 `C:\` 처럼 적절하게 바꿔보시면 좋습니다.

#### Slack Webhook MCP Server

##### 프롬프트

```
- 당신은 파이썬 프로그래머입니다.

# 상황

Claude Desktop for Windows에서 연결할 MCP(Model Context Protocol) Server를 구현해야합니다.

# 지시

1. [For Server Developers](https://modelcontextprotocol.io/quickstart/server)를 참고하여 
[서비스]에서 [도구]를 지원하는 MCP Server를 파이썬으로 작성하세요. 
2. [For Claude Desktop Users](https://modelcontextprotocol.io/quickstart/user)을 참고하여 Claude Desktop에 
[서버]를 등록하기 위한 claude_desktop_config.json 예시만 보여주세요.

서비스= https://hooks.slack.com

# 도구

send_slack_message: Slack Webhook URL을 통해 메시지를 전송합니다.

# 서버

command= uv
args:
  - --directory
  - D:\\Dev\\McpServers
  - run
  - slack.py
env: 
  - SLACK_WEBHOOK_URL= https://hooks.slack.com/services/...
```

##### 코드

```py
from typing import Any
import httpx
from mcp.server.fastmcp import FastMCP
import os

# MCP 서버 초기화
mcp = FastMCP("slack")

# 슬랙 메시지를 전송하는 도구 정의
@mcp.tool()
async def send_slack_message(message: str) -> str:
    """Slack Webhook URL을 통해 메시지를 전송합니다."""
    url = os.getenv("SLACK_WEBHOOK_URL")
    if not url:
        return "SLACK_WEBHOOK_URL 환경 변수가 설정되지 않았습니다."
    async with httpx.AsyncClient() as client:
        response = await client.post(url, json={"text": message})
        response.raise_for_status()
        return "메시지가 성공적으로 전송되었습니다."

if __name__ == "__main__":
    # 서버 실행
    mcp.run(transport='stdio')
```

##### claude_desktop_config.json

```json
{
  "mcpServers": {
    "slack": {
      "command": "uv",
      "args": [
          "--directory",
          "D:\\Dev\\McpServers",
          "run",
          "slack.py"
      ],
      "env": {
        "SLACK_WEBHOOK_URL": "https://hooks.slack.com/services/..."
      }
    }
  }
}
```

> `https://hooks.slack.com/services/...` 대신에 자신의 Slack Webhook URL을 넣어야합니다.

> `D:\\Dev\\McpServers` 는 현재 제 PC에 있는 로컬 디렉터리이므로 위 코드를 그대로 복사하면 실행하지 못할 것입니다. 따라서 이 부분은 `C:\` 처럼 적절하게 바꿔보시면 좋습니다.

## 브라우저 확장

*크롬 확장(Chrome Extention)을 기준으로 합니다*

> ChatGPT는 멋진 서비스이지만, 여전히 그것만으로는 할 수 없는 일들도 있습니다. 지금 내가 보고 있는 웹페이지에 대한 요약 및 번역, 유튜브 요약 및 자막 추출과 같은 일들 말이죠. 물론 브라우저 자체에서 제공하는 기능도 있지만, 다양한 AI 모델에 연결되어 있지 않아 비교하기 어렵다는 점도 있습니다. 이를 해결해주는 것이 브라우저 확장입니다.

- 브라우저 확장(Browser Extensions)를 사용하면 기본적인 채팅 뿐만 아니라 다양한 AI 모델과 연결하여 내가 보고 있는 웹페이지와 유튜브 영상을 요약, 번역할 수 있습니다.
- 과거에는 ChatGPT에서 웹검색, 음성 입력 등의 기능이 제공되지 않아 WebChatGPT, Promptheus와 같은 확장으로 존재하였으나 이제는 ChatGPT에서 제공하여 크게 의미가 사라진 것들도 있습니다.
- 브라우저에 의존하여 모바일이나 앱에서는 일부 사용하지 못할 수도 있습니다.
- 브라우저 확장은 공개 서비스이므로 보안 및 개인정보와 같은 민감 정보를 핸들링할 때는 주의가 필요합니다.

### Sider

- [Sider](https://chromewebstore.google.com/detail/sider-chatgpt-%EC%82%AC%EC%9D%B4%EB%93%9C%EB%B0%94-+-gpt/difoiogjjojoaoomphldepapgpbgkhkb?utm_source=ext_app_menu)는 AI와 관련된 가장 인기 있는 크롬 확장 중 하나로, GPT 뿐만 아니라 DeepSeek, Gemini, Claude 와 같은 여러 모델에 요청을 보낼 수 있으며 이들의 응답을 서로 비교해볼 수도 있습니다.
- 다른 서비스의 내용을 따로 복사붙여 넣기 하여 ChatGPT에 요청하는 번거로움을 가질필요 없이 그 자리에서 웹페이지 또는 유튜브 내용을 번역, 요약, 검색할 수 있습니다.
- 설치 후에는 웹사이트 오른쪽 사이드바에서 바로 실행되며, 복잡한 설정 없이 사용할 수 있습니다.

> [Sider](https://chromewebstore.google.com/detail/sider-chatgpt-%EC%82%AC%EC%9D%B4%EB%93%9C%EB%B0%94-+-gpt/difoiogjjojoaoomphldepapgpbgkhkb?utm_source=ext_app_menu) → Chrome에 추가 → 확장 프로그램 추가 → '기능 탐색하기'에서 할 수 있는 일 알아보기

## 정리

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 정의 | 여러 AI 기능과 외부 도구를 연결해 하나의 워크플로우를 만드는 방식 | 긱뉴스 → 뉴스 분류 → Slack 전송 |
| 목적 | 단일 AI 지시를 넘어, 다양한 도구와 API를 통합해 자동화된 작업 흐름 구성 | PDF 요약 → 구글 드라이브 저장 → 이메일 전송 |
| 주요 도구 | Zapier, Make, ChatGPT Custom GPTs | GPTs 내 '작업' 기능, Webhook, API Call |
| Custom GPTs | GPT 내부에서 역할·지식·도구 호출을 오케스트레이션 할 수 있는 기능 | 정책 요약 GPT, 뉴스 수집·분류·Slack 전송 GPT |
| 한계 | GPTs: 시간 기반 자동화 불가, 외부 API 연결 제한적 | 시간 트리거, 다중 GPT 간 통신 X |
| vs 노코드 도구 | GPTs: AI 중심 / Zapier, Make: 서비스 자동화 중심 | GPTs: AI 보고서 생성 / Zapier: 설문 → 구글시트 저장 |
| 구성 요소 | 지침(역할), 지식(참조파일), 내장 도구(ReAct), 작업(API Call) | '뉴스 요약봇': 뉴스 API → Slack Webhook 연결 |
| GPTs vs MCP | GPTs: 사용자가 직접 API 명세를 작성 / MCP: 서비스 제공자가 MCP 프로그램을 제공 | GPTs: 뉴스 API와 직접 연결 / MCP: 뉴스 API를 호출하는 도구 호출 |
| 브라우저 확장 | ChatGPT, Gemini와 같은 생성형 AI 서비스에서 직접하기 어려운 사용자의 브라우저와 실시간 인터렉션 수행 | 웹페이지 및 유튜브 번역, 요약, 추출 |

## 요약

- 오케스트레이션은 AI와 다양한 외부 도구를 연결해 자동화된 작업 흐름을 만드는 전략입니다.
- Custom GPTs, Zapier, Make 등의 도구를 통해 파일 요약, 외부 서비스 호출, 보고서 전송 등을 자동화할 수 있습니다.
- GPTs 오케스트레이션은 'AI 역할 중심', 노코드 도구는 '서비스 중심'이라는 차이가 있습니다.
- ChatGPT 작업에서는 요약, 이메일 작성, 보고서 검토, 번역 등 '반복적이거나 일관성이 필요한 작업(Task)'을 등록해 둘 수 있습니다.
- GPTs에서는 사용자가 직접 API Spec을 작성해야 하지만, MCP는 서비스 제공자가 이 과정을 대신해줍니다.

## 생각해보기

- 오케스트레이션과 프롬프트 체이닝의 차이점은 무엇인가요?
- Custom GPTs와 Zapier/Make 같은 노코드 오케스트레이션 도구는 어떤 점이 다를까요?
- 오케스트레이션이 실무에서 특히 유효한 작업 흐름에는 어떤 것들이 있을까요?
- 오케스트레이션 설계 시 보안적으로 주의해야 할 점은 무엇인가요?
- Custom GPTs에 비해 MCP를 사용했을 때 얻을 수 있는 편의성의 이점은 무엇인가요?

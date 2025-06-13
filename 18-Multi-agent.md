# 멀티 에이전트

> AI가 여러 역할로 나뉘어서 협업할 수 있다면? 오케스트레이션이 AI와 도구의 연결을 의미한다면, 멀티 에이전트 시스템(Multi-Agent System)은 AI 내부에서도 역할을 나눠 협업하는 구조입니다.

## 개요

- AI 에이전트(Agent)는 서로 다른 역할을 수행할 수 있는 능력을 가진 독립적인 AI 참여자입니다.
- 각 에이전트가 서로 다른 역할을 맡아서, 서로 정보를 주고 받으며 문제를 해결합니다.
- 멀티 에이전트에서는 전문적으로 튜닝된 다수의 AI 에이전트가 서로 협업하여 복잡한 문제를 나눠서 해결할 수 있습니다.
- 오케스트레이션과 함께 사용하면 도구 연결 + 역할 분담까지 확장할 수 있습니다.

## 예시

```
                  ┌───────────────────────────────┐
                  │         [입력 프롬프트]        
                  │ "긱뉴스의 최신뉴스를 슬랙으로 전송해" 
                  └───────────────────────────────┘
                                  ↓
                        ↓ 멀티 에이전트 시스템 ↓
        ┌──────────────────────────────────────────────────┐
        │                GeekNews Agent Team               │
┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐
│    [RSS Agent]      [News Summarize Agent]     [Slack Agent]    
│     ① 뉴스 수집            ② 뉴스 요약       ③ 슬랙으로 요약된 뉴스 전송
└───────────────────┘ └───────────────────┘ └───────────────────┘
        │                                                  │
        └──────────────────────────────────────────────────┘
```

## 실습

- 에이전트를 구축하기 위한 로우코드(Low-code) 도구인 [Microsoft AutoGen Studio](https://microsoft.github.io/autogen/stable//user-guide/autogenstudio-user-guide/index.html)를 사용해서 멀티 에이전트 시스템 구축을 실습해봅시다.

> 이 실습은 선택적으로 진행하시면 됩니다.

> 이 실습을 진행하기 위해서는 [Python](https://www.python.org/downloads/windows/), [Cursor](https://www.cursor.com/)를 설치해야 합니다.

### 설치

> (Windows) 설치하기 전에 Cursor에서 F1을 누르고 `Python: Create Environment...` 를 실행하고 `venv`로 설정하세요. 그 다음, 터미널에 `.venv/Scripts/activate` 입력하세요.

```
pip install -U autogenstudio
```

```
autogenstudio ui --port 8081
```

- 명령어 실행 이후에는 웹브라우저에서 http://127.0.0.1:8081 주소로 접속하세요.

### 설정

- (Widows) 시스템 설정 → 고급 시스템 설정 → 환경 변수 → 사용자 변수 → 새로 만들기 → `OEPNAI_API_KEY` 등록

### 팀

- 사이드바의 갤러리에서는 팀(Teams), 에이전트(Agents), 도구(Tools), 모델(Models), 종료 조건(Terminations) 등 에이전트 설정에 필요한 구성 요소들을 살펴볼 수 있습니다.
- Add Team을 누르고 GeekNews Team을 생성하세요. Description, Configuration 등 다른 부분은 건드리지 않아도 됩니다.

### 에이전트

- GeekNews Team에 속할 에이전트는 뉴스를 수집할 RSS Agent, 긱뉴스를 요약할 News Summarize Agent, 슬랙으로 메시지를 전송할 Slack Agent입니다.

#### RSS Agent

```
Name: RSS Agent
Description: 긱뉴스에서 RSS를 통해 최신뉴스를 수집합니다.
Configurtion:
  Name: rss_agent
  System Message: 긱뉴스 RSS(https://feeds.feedburner.com/geeknews-feed)에서 뉴스를 수집하세요.
  Tools: ×
```

#### News Summarize Agent

```
Name: News Summarize Agent
Description: News RSS를 요약합니다.
Configurtion:
  Name: news_summarize_agent
  System Message: 다른 에이전트에게서 받은 News RSS를 요약하세요.
  Tools: ×
```

#### Slack Agent

```
Name: Slack Agent
Description: Slack Webhook으로 슬랙에 메시지를 전송합니다.
Configurtion:
  Name: slack_agent
  System Message: Slack Webhook으로 슬랙에 메시지를 전송하세요.
  Tools: send_slack_message
```

##### 환경 변수

- Settings → Environment Variables에서 `SLACK_WEBHOOK_URL`을 설정하세요.

##### send_slack_message

```
Name: send_slack_message
Description: Slack Webhook URL을 통해 슬랙으로 메시지를 전송합니다.
Configution:
  Function Name: send_slack_message
```

```
- 당신은 파이썬 프로그래머입니다.

# 지시

Autogen Studio에서 [도구]를 만들기 위한 파이썬 함수를 작성하세요.

# 도구

send_slack_message: Slack Webhook URL을 통해 메시지를 전송합니다.
(단, Slack Webhook URL은 환경변수로 제공합니다.)
```

```
// Global Imports → Direct Import

os, requests
```

```py
def send_slack_message(message: str) -> dict:
    """
    Slack Webhook URL을 통해 메시지를 전송합니다.

    환경변수 'SLACK_WEBHOOK_URL'에 Webhook URL이 저장되어 있어야 합니다.

    Args:
        message (str): 전송할 메시지 내용

    Returns:
        dict: Slack API의 응답 결과 또는 에러 정보
    """
    webhook_url = os.getenv("SLACK_WEBHOOK_URL")
    if not webhook_url:
        return {"error": "SLACK_WEBHOOK_URL 환경변수가 설정되어 있지 않습니다."}

    payload = {"text": message}

    try:
        response = requests.post(webhook_url, json=payload)
        response.raise_for_status()
        return {"status": "success", "response": response.text}
    except requests.exceptions.RequestException as e:
        return {"status": "error", "message": str(e)}
```

### 팀 빌더

- 사이드바에 있는 팀 빌더(Team Builder)에서 실제로 팀을 구성할 수 있습니다.

1. New Team버튼 하단에 있는 From Gallery에서 기존에 만들어 놓은 GeekNews Team을 복사합니다.
2. 기본적으로 추가되어 있는 에이전트를 제거하고 RSS Agent, New Summarize Agent, Slack Agent를 추가합니다.
3. Tools에서 RSS Agent에 `fetch_webpage`도구를 추가하세요.
4. GeekNews Team 설정에서 Termination Condition → `OrTerminationCondition` 설정에서 `MaxMessageTermination`을 `4`로 설정하세요. (너무 많이 설정하면 메시지가 중단되지 않기 때문에 많은 토큰이 소모되므로 주의가 필요합니다.)
4. 저장 버튼을 누른 뒤, 옆에 있는 Run버튼을 누르고 `긱뉴스에 있는 최신뉴스를 슬랙으로 전송해줘`를 입력한 뒤, 슬랙으로 전송되는지 확인하세요.

### 배포

- AutoGen Studio에서 Deploy로 가서 *Run a Team in Python*에서 보면, 우리가 만든 팀을 파이썬으로 실행할 수 있습니다.
- 먼저 팀 빌더로 가서 `Run` 왼쪽에 보면 팀을 다운받을 수 있는 버튼으로 JSON을 다운 받아옵니다.
- 그 다음, Cusor에서 `agent.py` 파일을 하나 만들고 다음과 같은 프롬프트를 작성합니다.
- 코드가 작성되었다면, 이제 환경변수에 `SLACK_WEBHOOK_URL`을 등록하고 재부팅한 뒤, 터미널에 `python agent.py`를 실행하여 슬랙 채널에 최신 긱뉴스가 전송되었는지 확인합니다.

```
- 당신은 파이썬 프로그래머입니다.

# 지시

AutoGen Studio에서 작성한 에이전트 팀이 [task]를 수행하기 위해 미리 정의된 에이전트 팀 프리셋인 [team_config]를 불러오고 파이썬으로 구동하기 위해 다음의 [예시]를 참고하여 작성해보세요.

task= "긱뉴스에서 최신뉴스를 슬랙으로 전송해"
team_config= ./team-config.json

# 예시

from autogenstudio.teammanager import TeamManager

# Initialize the TeamManager
manager = TeamManager()

# Run a task with a specific team configuration
result = await manager.run(
  task="What is the weather in New York?",
  team_config="team.json"
)
print(result)
```

```py
from autogenstudio.teammanager import TeamManager

async def main():
    # TeamManager 초기화
    manager = TeamManager()

    # 특정 팀 구성으로 작업 실행
    result = await manager.run(
        task="긱뉴스에서 최신뉴스를 슬랙으로 전송해",
        team_config="./team-config.json"
    )
    print(result)

# 메인 함수 실행
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

## 로컬 LLM

- GPT가 아닌 로컬에서 언어모델을 구동하고 AutoGen Studio에서 등록하여 팀을 구성해보겠습니다.
- 로컬에서 LLM을 구동하기 위한 도구로는 대표적으로 [Ollama](https://ollama.com/), [LM Studio](https://lmstudio.ai/)가 있으며 모델 커스텀을 지원하고 명렁 프롬프트에서 실행하는 명령어 기반인 Ollama보다는 비전문가가 사용하기 좋은 LM Studio를 사용합니다.

### 실습

- 먼저, LM Studio에서 LG Research에서 만든 모델인 `EXAONE-3.5-7.8B-Instruct-GGUF`를 다운받고 Developer탭으로 이동하여 서버를 실행합니다.
- 그 다음, AutoGen Studio의 갤러리로 이동하여 모델을 등록해야 하는데, LM Studio에서 구동되는 서버는 Openai-like이기 때문에 OpenAI API와 호환이 됩니다. 따라서 모델이름은 그대로 놔두고, `Base URL`만 `http://127.0.0.1:1234/v1` 로 바꿔줍니다.
- 이후, Team Builder로 이동하여 에이전트에 새로 작성한 모델을 할당하고 태스크 실행을 요청한다음, LM Studio에 다음과 유사한 요청 로그가 기록되는지 확인합니다.

```json
{
  "id": "chatcmpl-3wzwiluduq6ylf4eg85swq",
  "object": "chat.completion",
  "created": 1749379266,
  "model": "exaone-3.5-7.8b-instruct",
  "choices": [
    {
      "index": 0,
      "logprobs": null,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "4"
      }
    }
  ],
  "usage": {
    "prompt_tokens": 32,
    "completion_tokens": 1,
    "total_tokens": 33
  },
  "stats": {},
  "system_fingerprint": "exaone-3.5-7.8b-instruct"
}
```

### EXAONE-3.5-7.8B-Instruct-Q4_K_M.gguf

- `EXAONE-3.5-7.8B-Instruct-GGUF`의 풀네임은 `EXAONE-3.5-7.8B-Instruct-Q4_K_M.gguf` 입니다. `EXAONE`은 모델 브랜드 이름, `3.5`는 버전, `7.8B`는 신경망을 구성하는 파라미터의 규모를 의미하며 78억개의 파라미터를 사용했음을 의미합니다. 

  > 모델의 파라미터 개수가 많다는 것은 더 방대하고 복잡한 신경망으로 구성되어 있다는 뜻으로, 일반적으로 파라미터가 커질수록 모델의 문맥 이해 및 추론 능력이 향상되는 경향이 있습니다. 하지만 막대한 계산비용이 소모되며 로컬에서 실행시 CPU/GPU의 메모리도 그만큼 요구됩니다. 최근에는 모델의 크기를 늘리지 않고 효율적으로 개선하기 위한 방법들도 연구되고 있습니다.  

  > LM Studio에서 모델을 로드하고 `프롬프트 엔지니어링에 대해 설명해줘` 과 같은 질의를 해보고, 이에 대답할 때 크게 증가하는 GPU/CPU 자원 사용량에 주목하세요.

- `instruct`는 [인스트럭션 튜닝](14-Learning.md#%EC%9D%B8%EC%8A%A4%ED%8A%B8%EB%9F%AD%EC%85%98-%ED%8A%9C%EB%8B%9D)을 했다는 뜻입니다. 인스트럭션 튜닝은 사용자의 명령(~해줘)에 더 정확하게 반응하도록 모델을 추가 학습시키는 방식입니다.

#### 양자화

- 양자화(Quantization)는 모델의 파라미터(가중치)를 고정소수점 숫자 또는 더 낮은 정밀도의 숫자로 변환해, 메모리와 연산 비용을 줄이는 방법입니다.
- 원래 모델은 대부분 FP32(32비트 부동소수점) 형식으로 훈련되는데, 양자화는 이를 FP16, INT8, INT4 등 낮은 정밀도로 바꿉니다. 예를 들어, 소수점 다섯 자리까지 저장하던 숫자를 소수점 한두 자리까지만 저장하거나, 아주 작은 정수로 바꾸는 방식입니다.
- 메모리 사용량과 연산 속도를 줄여, 로컬이나 엣지 디바이스에서도 대형 모델을 실행할 수 있게 합니다.

> 4K 고화질 동영상을 SD 화질로 압축하면, 용량은 줄지만 여전히 내용을 볼 수 있습니다. 양자화는 언어 모델을 그런 식으로 압축해서, 덜 정밀하지만 훨씬 가볍게 만드는 기술입니다.

> 너무 많이 압축하면 모델이 바보처럼 행동할 수도 있습니다. 하지만 요즘은 적절히 압축해도 원래 모델과 거의 비슷하게 작동하도록 연구가 잘 되어 있습니다. 그래서 우리가 사용하는 로컬 챗봇이나 모바일 AI 서비스 대부분이 이런 양자화된 모델을 사용합니다.

##### 정밀도

| 형식 | 비트수 | 메모리 절감률 | 정확도 영향 |
| --- | --- | ---- | ---- |
| FP32 | 32bit | 기준치 | 없음 |
| FP16 | 16bit | 절반 | 거의 없음 |
| INT8 | 8bit  | 75% 절감 | 낮음 \~ 중간 |
| INT4 | 4bit  | 87.5% 절감 | 중간 \~ 높음 |

##### 형식 

| 이름 | 설명 | 비트 수 | 특징 |
| --- | --- | --- | --- |
| `Q4_0`, `Q4_K_M` | 4비트 정밀도 | INT4 | 가장 널리 쓰이는 기본 방식 |
| `Q5_K_M` | 5비트 | INT5 | 더 높은 정밀도 |
| `Q8_0` | 8비트 | INT8 | 거의 정확도 손실 없음, 메모리 큼 |
| `F16` | 16비트 부동소수점 | FP16 | 정밀도 유지, 고사양 필요 |

##### _0, _K, _K_M

- `_0`: 가장 단순한 4비트 양자화로, 빠르고 메모리 효율은 좋지만, 정확도 손실이 가장 큽니다. 주로 테스트 또는 가벼운 챗봇 용도로 사용합니다.
- `_K`: 개선된 4비트 양자화입니다. `_0` 보다 느리지만 정확도가 향상됩니다.
- `_K_M`: `_K`에 추가적인 메타정보를 덧붙여 정확도를 더욱 높인 방식입니다. 실행 속도와 정밀도 사이의 균형이 좋아, 로컬 실행에서 가장 널리 쓰입니다.

#### GGUF

- 마지막으로 `GGUF`는 모델의 포맷을 의미하는데, GGUF는 GGML Unified Format의 약자입니다. `GGML`은 GGML은 Georgi Gerganov가 만든 경량화 머신러닝 연산 라이브러리입니다. 모델을 CPU/GPU에서 빠르게 실행할 수 있도록 최적화되어 있으며, 경량화된 머신러닝 모델의 CPU/GPU 실행을 목표로 하는 C 라이브러리입니다. GGUF은 GGML을 기반으로 동작하는 모델 포맷입니다. 
- GGML 파일 형식은 초기에는 매우 효과적이었지만, 모델 메타데이터 추가나 새로운 기능 도입 시 호환성 문제가 발생할 수 있었습니다. GGUF는 이러한 GGML의 한계를 극복하고 더 유연하고 확장 가능한 통일된 포맷을 제공하기 위해 개발되었습니다. 즉, GGUF는 GGML의 후속 버전이자 개선된 포맷이라고 볼 수 있습니다.

  > 비유하자면, GGUF는 `.mp4`와 같은 영상 파일이고, GGML은 그것을 재생하는 미디어 플레이어 같은 역할입니다.

##### GGML vs GGUF

| 항목 | GGML | GGUF |
| --- | --- | --- |
| 정체 | 추론 연산 라이브러리 | 모델 저장 포맷  |
| 기능 | 모델 실행, 연산 최적화 | 모델, 토크나이저, 메타데이터 저장 |
| 관계 | GGML 라이브러리가 GGUF을 로드함 | GGUF는 GGML에 의해 로드됨 |
| 예  | `llama.cpp`, `koboldcpp`, `ggml.c` | `EXAONE-3.5-7.8B-Instruct.gguf` |

## vs 오케스트레이션

- LangChain, AutoGen, CrewAI 등의 도구를 사용하는 멀티 에이전트 구축은 비전문가가 선뜻 시작하기에는 기술적 진입장벽이 있어 도구들만 간략하게 소개합니다.
- AutoGen Studio뿐만 아니라 [Flowise](https://github.com/FlowiseAI/Flowise), [Langflow](https://github.com/langflow-ai/langflow) 또한 노코드(No-code)/로우 코드(Low-Code)도구이므로 사용해보면 좋습니다. 

| 항목 | 오케스트레이션 | 멀티 에이전트 시스템  |
| --- | --- | --- |
| 구성 | AI + 외부 도구 (슬랙, 구글 시트 등) 연결 | 여러 AI 에이전트 간 협업 |
| 중심 | 워크플로우 자동화 | 역할 분담, 협력 기반 문제 해결 |
| 연결 방식 | Zapier, Make, Custom GPTs 등 | 에이전트 간 메시지 전달 (LangChain, AutoGen, CrewAI, Flowise 등) |

## A2A

- [A2A(Agent2Agent)](https://google.github.io/A2A/#/)는 Google이 주도하여 개발한 오픈 프로토콜입니다. 
- 서로 다른 프레임워크에서 개발된 AI 에이전트들이 서로 통신하고 협업할 수 있도록 설계되었습니다.
- 이 프로토콜은 에이전트들이 내부 구조나 구현 방식에 관계없이 상호 작용할 수 있는 공통 언어를 제공합니다.

> 현재 다양한 AI 에이전트들이 존재하지만, 이들은 주로 자체 생태계 내에서만 작동하며, 서로 다른 시스템 간의 협업은 어려운 실정입니다.

> A2A는 현재 실험적인 프로토콜입니다. 따라서 여기에서는 간단하게 소개만하고 넘어갑니다.

## 정리

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 정의 | AI 내부 또는 외부에서 역할을 나눠 여러 에이전트가 협업하는 구조 | 초안 작성 에이전트 → 검토 에이전트 → 요약 에이전트 |
| 에이전트(Agent) | 독립적으로 문제를 해결할 수 있는 AI 참여자 | 생성 에이전트, 피드백 에이전트, 평가 에이전트 |
| 역할 분담 | 하나의 AI가 모든 것을 하지 않고, 전문 역할을 나눔 (메시지 전달, 역할별 분업, 결과물 공유) | 수집 → 요약 → 전송 |
| 구성 도구 | LangChain, AutoGen, CrewAI 등 | LangChain Agents, Microsoft AutoGen |

## 요약

- 멀티 에이전트는 하나의 AI가 아닌, 여러 AI가 역할을 나눠 협력하여 문제를 해결하는 전략입니다.
- 각 에이전트는 생성, 검토, 피드백, 요약 등 독립적인 역할을 수행합니다.
- 오케스트레이션이 도구 연결 중심이라면, 멀티 에이전트는 AI 내부 역할 분업 중심입니다.
- A2A는 서로 다른 프레임워크에서 개발된 AI 에이전트들이 서로 통신하고 협업할 수 있도록 설계된 프로토콜입니다.

## 생각해보기

- 멀티 에이전트와 오케스트레이션은 어떤 점에서 비슷하고, 어떤 점에서 다른가요?
- 생성 에이전트, 검토 에이전트, 피드백 에이전트가 협업할 때 어떤 장점이 있을까요?
- 멀티 에이전트 시스템이 특히 효과적인 업무 사례는 어떤 것이 있을까요?
- 멀티 에이전트 도입 시, 협업 단계에서 발생할 수 있는 문제점과 해결 방법은 무엇인가요?
- 단일 AI와 멀티 에이전트 구성을 선택할 때 고려해야 할 기준은 무엇인가요?

---

[AI 오케스트레이션](12-Orchestration.md) ← 이전 / 다음 → [학습](19-Learning.md)
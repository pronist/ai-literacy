# 멀티모달

- 멀티모달(Multimodal)은 텍스트 뿐만 아니라 여러 종류(파일, 이미지, 사운드 등)의 모달리티(Modality)입력을 받아서 처리할 수 있는 능력입니다. 

> 멀티모달 모델은 '말을 잘하는 AI'가 아니라, '눈과 귀와 손까지 갖춘 AI'로 진화하는 과정입니다.

> 모달리티(Modality): '양식', '양상'이라는 뜻으로, 보통 어떤 형태로 나타나는 현상이나 그것을 받아들이는 방식을 말합니다. AI가 처리할 수 있는 다양한 데이터 종류를 의미합니다.

## 주의할 점

- 입력 형식이 복잡(그래프, 복잡한 표)할수록 오류나 오해 가능성이 높아집니다.
- 멀티모달은 더 많은 자원과 비용이 소요됩니다.
- 아직까지는 정확도나 안전성 측면에서 실험적인 기능도 많습니다.

## 기능

| 기능 | 설명 |
| --- | --- |
| 이미지 분석 | 사진, 그래프, UI 스크린샷 등을 설명하거나 요약 |
| 파일 요약 | PDF, 보고서, 프레젠테이션 등을 자동 요약 |
| 표/코드 이해 | 표, HTML, JSON 등 구조화된 정보 해석 |
| 음성 입력 처리 | 사용자의 말소리를 듣고 답변 생성 |
| 이미지 + 텍스트 혼합 질문 처리 | "이 사진 속 문제가 뭔지 설명해줘"와 같은 복합 요청 가능 |

## 예시

| 활용 분야 | 멀티모달 예시 |
| --- | --- |
| 고객지원 챗봇 | "고객이 보낸 고장 사진 + 설명" 같이 분석 |
| 학습 튜터 | "학생의 손글씨 답안 사진 + 문제 설명" 이해 |
| 문서 요약 | "PDF 보고서 + 표 + 이미지 포함 문서" 요약 |
| 의료 | "X-ray 이미지 + 환자 기록" 동시 참고 |

## 원리

```
┌───────────────────────────────┐
│            [입력]          
│ ┌─────────────┐            
│ │  글자 입력  "이 고양이 어때?" 
│ └─────────────┘             
│ ┌─────────────┐             
│ │  사진 올리기   [cat.jpg]        
│ └─────────────┘             
│ ┌─────────────┐             
│ │  목소리 녹음   [WAV 파일] 
│ └─────────────┘             
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│          [모달리티 연결]     
│ - 글, 사진, 목소리 정보 모두 참고       
│ - 서로 관련 있는 부분 연결          
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│             [응답]             
│ → "귀여운 고양이야! 아메리칸 숏헤어처럼 보여."     
└───────────────────────────────┘
```

## 실습

- ChatGPT [Sora](https://sora.chatgpt.com/explore/images)에서 `그레이스케일, 비오는 날, 새끼 고양이, 창가에 앉아 있음, 정면 자세, 창문 바깥쪽으로 돌아간 고개, 풀샷, 지브리풍` 에 해당하는 고양이 그림 하나를 그려보겠습니다.

```
grayscale, rainy day, kitten, sitting by the window, front view, head turned out of window, full shot, ghibli style
```

- ChatGPT로 이동하여 고양이 그림을 업로드하고, 이에 대해 물어봅시다.

```
이 고양이 어때?
```

## 코드 인터프리터

- ChatGPT 코드 인터프리터(Code Interpreter = Advanced Data Analysis)는 파이썬(Python) 코드를 분석하고 실행해주는 기능입니다.
- 멀티모달을 통해 Excel, CSV와 같은 구조화된 데이터 파일을 분석하도록 지시하면 내부에서 코드가 작성되어 실행됩니다.

> 자연어 프롬프트 뿐만 아니라 날(Raw) 파이썬 코드를 편집하고 직접 실행할 수도 있는데, ChatGPT 채팅 → 캔버스(Canvas)에서는 코드를 직접 편집하고 실행까지 할 수 있습니다.

> 단, 파이썬을 통해 외부 서버로 요청(이메일 전송 등)을 보내는 네트워크 작업은 정책상 제한되어 있습니다.

### 예시

| 할 수 있는 일 | 설명 | 예시 |
| --- | --- | --- |
| 데이터 파일 분석 (CSV, Excel) | 숫자 요약, 그래프 그리기 | "매출 데이터를 분석해줘" |
| 텍스트 파일 처리 | 텍스트 요약, 패턴 찾기 | "로그 파일에서 오류만 추출" |
| 수학 계산, 공식 검증 | 수식 계산, 반복 처리 | "복리 계산을 자동화" |
| 그래프 시각화 | 막대그래프, 꺾은선그래프, 파이차트 등 | "매출 증감 추이를 그래프로" |
| 파일 변환 | CSV → Excel, JSON → CSV 등 | "이 CSV 파일을 엑셀로 변환" |

### 데이터 분석

- [Kaggle](https://www.kaggle.com)로 이동하여 [TESLA Stock Data](https://www.kaggle.com/datasets/varpit94/tesla-stock-data-updated-till-28jun2021)를 다운로드 합니다.
- ChatGPT에서 다운받은 엑셀 파일(TSLA.csv)을 업로드하고 다음과 같이 입력합니다.

```
- 당신은 금융 데이터 분석가입니다.

# 입력

TSLA.csv는 테슬라에 대한 주가 정보입니다.

# 지시

2010년 7월의 주가 정보를 일봉 그래프로 나타내고, 5일 이동평균선을 나타내세요.
```

### 캔버스

- ChatGPT 채팅 → 캔버스를 클릭하고, 다음과 같은 프롬프트를 작성합니다.

```
a+b를 수행하는 python 함수를 작성해줘
```

- Canvas가 나타나면 코드를 다음과 같이 바꾸고 실행을 누르면 결과 값으로 `30`이 출력됩니다.

```py
def add(a, b):
    return a+b

sum = add(10, 20)

print(sum)
```

## 정리

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 정의 | 텍스트뿐 아니라 이미지, 파일, 음성 등 다양한 입력을 처리하는 AI 능력 | 사진 설명, PDF 요약, 음성 입력 |
| 멀티모달 의미 | '양식, 방식' → 데이터 형태(텍스트, 이미지, 음성, 파일 등) | 고장 사진 + 설명 → 분석 결과 제공 |
| 주요 기능 | 이미지 분석, 파일 요약, 표·코드 이해, 음성 처리, 이미지+텍스트 혼합 | X-ray + 환자 기록 동시 분석, 손글씨 사진 + 문제 풀이 |
| 장점 | 복합 정보 이해, 다양한 입력 지원, 설명력 강화 | 그래프 해석, 사진 속 문제 파악 |
| 주의할 점 | 입력 복잡성 증가 → 오류 가능성, 비용 증가, 정확도·안전성 실험적 | 표가 복잡하면 오해 가능, 이미지 해석 오류 |
| 코드 인터프리터 | Python 코드로 데이터 처리 가능, 그래프 작성, 파일 변환 등 | CSV → Excel 변환, 로그 파일 오류 찾기 |

## 요약

- 멀티모달은 텍스트 외에도 이미지, 음성, 파일 등을 함께 이해하는 AI 기술입니다.
- 다양한 입력을 조합해 더 풍부한 답변과 분석을 제공하지만, 복잡성에 따른 오류 가능성도 있습니다.
- 코드 인터프리터를 활용하면 데이터 파일 분석, 변환, 시각화까지 자동화가 가능합니다.

## 생각해보기

- 멀티모달 입력이 가능한 경우와 그렇지 않은 경우 결과에 어떤 차이가 날까요?
- 이미지 + 텍스트를 함께 입력하면 어떤 추가 정보를 얻을 수 있을까요?
- 멀티모달 입력을 사용할 때 발생할 수 있는 위험 요소는 무엇인가요?
- 코드 인터프리터를 통해 어떤 작업을 자동화할 수 있을까요?
- 비전문가가 멀티모달 기능을 활용하기 위해 알아야 할 최소한의 정보는 무엇일까요?

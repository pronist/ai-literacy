# 샘플링

- 트랜스포머 모델이 다음에 올 단어(토큰)를 예측할 때, 가장 가능성 높은 단어를 고정적으로 선택하지 않고, 확률 분포에 따라 무작위로 선택하는 방식입니다. 
- 다양한 문장 생성, 창의적인 표현, 자연스러움 향상을 위해 필수적인 과정이며 출력되는 단어가 매번 다른 이유도 여기에 있습니다.

## 전략

| 이름 | 설명 | 특징 |
| --- | --- | --- |
| Greedy decoding | 확률이 가장 높은 토큰만 선택 | 항상 같은 답변, 창의성 낮음 |
| Top-k 샘플링 | 확률이 높은 상위 *k*개 후보 중 무작위 선택 | 창의성 증가, 통제력 있음 |
| Top-p (Nucleus) 샘플링 | 누적 확률 *p* 이상인 후보군에서 선택 (예: p=0.9) | 더 유연한 선택, 자연스러움 향상 |
| Temperature | 확률 분포의 날카로움 조절 (0~2 사이, 기본값 1) | 낮으면 보수적, 높으면 창의적 (또는 이상함) |
- 대화 챗봇, 요약, 정보검색 → 낮은 Temperature, Top-k, Top-p 제한
- 창작, 마케팅 카피, 소설 생성 → 높은 Temperature, Top-p 넓게

## 예시

| 사용 목적 | 추천 샘플링 전략 | 이유 |
|-----------|-------------------|------|
| 정보 요약 | Greedy, Temp=0.3 | 일관된 응답, 환각 방지 |
| 창작 콘텐츠 | Top-p=0.9, Temp=1.2 | 창의성 증가 |
| 마케팅 카피 | Top-k=5, Temp=1.0 | 적당한 통제 + 다양성 |
| 정답형 질의 | Greedy, Temp=0 | 명확한 답 유도 |

## Temperature

- 숫자가 낮을수록 정답만 고르려 하고, 높을수록 예측 불가능하고 창의적인 단어 선택 가능하게 합니다.
- 높은 Temperature는 창의성을 높이지만 일관성을 떨어뜨릴 수 있으므로 목적에 따라 적절한 값을 선택해야 합니다.

| Temperature | 의미 |
| --- | --- |
| `0.1` | 거의 정해진 정답만 선택함 |
| `0.7` | 대체로 자연스러운 답변 생성 (기본값 추천) |
| `1.5` | 무슨 말 나올지 몰라서 약간 불안함 |
| `2.0` | 환각과 상상의 경계선에서 춤추는 언어모델 |

## 원리

```
┌───────────────────────────────┐
│         [디코더 출력] ← 다음 단어 예측 확률 분포
│ "커피를" (40%)              
│ "빵을" (25%)                
│ "신문을" (15%)              
│ "라면을" (10%)              
│ "전구를" (5%)               
│ "드럼을" (3%)              
│ "샌드백을" (2%)             
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│    샘플링 방식에 따른 선택 로직               
│ [Greedy] → "커피를"
│ [Top-k, k=3] → 랜덤(커피/빵/신문)
│ [Top-p, p=0.9] → 랜덤(커피~라면)
│ [Temp=1.5] → 확률 평탄화 후 랜덤 선택
│ [Temp=0.2] → 상위 단어에 집중 선택
└───────────────────────────────┘
                ↓
┌───────────────────────────────┐
│         실제 출력 예시              
│ → "커피를 마셨다." (Greedy)    
│ → "빵을 먹었다." (Top-k)       
│ → "라면을 끓였다." (Top-p)     
│ → "샌드백을 쳤다." (Temp↑)      
└───────────────────────────────┘
```

## 파라미터

- 샘플링에서 살펴본 `top-k`, `top-p`, `temperature` 와 같이 출력을 제어하기 위한 설정들을 파라미터(Parameter)라고 합니다. 추가적으로 `max_tokens`, `frequency_penalty`, `presence_penalty` 가 있습니다.
- `max_tokens`, `presence_penalty`, `frequency_penalty` 는 샘플링과는 다르게 다음에 '어떤 단어를 말해야 하는 지'가 아니라 '어떻게 말해야 하는 지'에 대한 문제입니다.

> 파라미터는 ChatGPT와 같은 범용 AI 채팅 서비스에서는 세세하게 설정할 수 없으나 OpenAI Playground, Google AI Studio 와 같은 AI 실험 도구에서는 설정할 수 있습니다.

| 이름 | 의미 | 효과 | 예시 | 범위 |
| --- | --- | --- | --- | --- |
| `max_tokens` | 생성할 수 있는 최대 토큰 수를 제한함 | 너무 긴 응답 방지, 리소스 관리, 토큰 초과 방지 | `max_tokens=50` → "한 문장만 대답해!" 강제하는 느낌 | |
| `presence_penalty` | 이미 등장한 주제/단어 사용 억제 | 새로운 아이디어, 창의성 강조 | ○: "봄에는 꽃이 피고, 여름이 오기 전에 산책하기 좋다." <br> ×: "봄은 따뜻하다. 봄에는 꽃이 핀다. 봄에... 또 봄에..." | 0.0 ~ 2.0 |
| `frequency_penalty` | 자주 등장하는 단어의 확률을 깎음 | 같은 단어 반복 줄이기 → 말이 덜 지루해짐 | ○: "고양이는 귀엽고, 작고, 친근한 동물이다." <br> ×: "고양이는 귀엽다. 고양이는 작다. 고양이는... 또 고양이는..." | 0.0 ~ 2.0 |

> `presence_penalty`는 단어의 등장 여부가 기준(단, 그 이후 횟수는 무관)이고, `frequency_penalty` 단어가 등장한 횟수가 기준입니다.

## 정리

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 정의 | 모델이 다음 단어(토큰)를 예측할 때 확률 기반으로 선택하는 방법 | "커피를 마셨다" vs "빵을 먹었다" (Greedy vs Top-k) |
| Greedy decoding | 가장 확률 높은 토큰만 선택 → 항상 같은 답변 | "커피를 마셨다"만 계속 출력 |
| Top-k 샘플링 | 상위 *k*개 후보 중 랜덤 선택 | k=3이면 상위 3개 후보 중 하나 랜덤 선택 |
| Top-p (Nucleus) 샘플링 | 누적 확률 *p* 이상인 후보군에서 선택 | p=0.9이면 상위 90% 확률까지의 후보군에서 랜덤 선택 |
| Temperature | 확률 분포의 날카로움 조정 (0~2, 기본 1) | 낮으면 정답형, 높으면 창의적 (Temp=1.5 → 예측 불가한 결과) |
| 기타 파라미터 | `max_tokens`, `presence_penalty`, `frequency_penalty` | 길이 제한, 반복 방지, 새로운 단어 사용 유도 |

## 요약

- 샘플링 전략은 모델이 다음 단어를 생성할 때 확률 기반으로 선택하는 방법을 제어하는 설정입니다.
- Greedy, Top-k, Top-p, Temperature를 조합해 다양성, 창의성, 안정성을 조절할 수 있습니다.
- 목적에 따라 정답형, 창작형, 요약형 등 다양한 응답 스타일을 설계할 수 있습니다.

## 생각해보기

- Top-k와 Top-p 샘플링은 어떤 상황에서 각각 유리할까요?
- Temperature 값이 높을수록 결과에 어떤 변화가 생기나요?
- 정보 요약과 창작 콘텐츠 작성 시, 각각 어떤 샘플링 전략이 적합할까요?
- presence_penalty와 frequency_penalty는 어떤 역할을 하나요?

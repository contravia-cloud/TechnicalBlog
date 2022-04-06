---
title: "Probabilistic_robotics"
date: 2022-04-06

---

> PPROBABILITIC ROBOTICS, 에이콘, 세바스찬 스런,볼프람 부르가트,디터 폭스 지음, 남궁영환 옮김

# 01 소개
## 1.1 로보틱스 분양의 불확실성
- 로봇의 불확실성에 영향을 주는 요인  
| 로봇 환경 | 본질적으로 예측 불가능함. 고속도로나 개인 주택 같은 호나경은 변화무쌍.|
| 센서 | 센서의 물리적 제약(범위와 해상도), 노이즈 |
| 로봇 액추에이션 | 소음, 마모, 고장 등 |
| 로봇 소프트웨어 | 모델의 오차, 알고리즘 근사화 |
- 이러한 불확실성을 잘 다루기 위해 이 책을 쓰게되었다.  

## 1.2 확률론적 로보틱스
- 확률론적 로보틱스는 **로봇 인식(robot perception)** 과 **액션**의 불확실성을 주로 다룬다.  
- 확률론적 로보틱스의 핵심 아이디어는 **확률 이론의 미적분학**  
- 특정 상황이 가장 좋을 것이라는 최적화의 방법이 아니라 추측할 수 있는 전체 공간의 확률 분포를 기반으로 정보를 표현하는 것임  
  이를 통해 모호함과 빌리프의 정도를 수학적으로 나타낼 수 있으며 제어 선택을 통해 불확실성을 낮출 수 있다.


### 2. uncertainty(불확실성)
#### 2.1 probability
##### 2.1.1 possible world
##### 2.1.2 Axioms in probibility 
  - 0 <= P(w) <= 1
  - ΣP(w) = 1  
  
##### 2.1.3 unconditional probability
  - Unconditional probability is the degree of belief in a proposition in the absence of any other evidence.
    - 비조건부 확률이란 다른 증거가 없을 때 명제(주사위 합이 7이다)에 대한 믿음의 정도(1/6)를 말한다.  
  
#### 2.2 Conditional Probability
  - 주사위 두개를 던져 합이 7이나오는 확률은 비조건부 확률이된다. 이전 사건이 결과에 영향을 주지 않기 때문이다.  
    - 그러나 AI 등에서는 세계가 실제로 작동하는 방식에 대한 초기 지식이 있는 확률이다.
    - 따라서 이것은 조건부 확률이 되는 것이다.
  - conditional probability is degree of belief in a proposition given some evidence that has already been revealed.
    - 조건부 확률은 이미 밝혀진 몇 가지 증거가 주어진 명제에 대한 믿음의 정도이다.
  - P(오늘 비가올까|어제 비가왔다) 이때 조건부 확률을 계산하는 방법은?  
  
#### 2.3 random variable
  - A random variable is a variable in probability theory with a domain of possible values that it can take on.
    - 무작위 변수(random variable)는 확률론에서 가능한 값의 영역을 갖는 변수이다.
    - 예를 들어 날씨가 무작위 변수이고 흐림, 맑음, 비 등이 취할 수 있는 값이 된다.
    - 무작위 변수는 확률 변수라 하고 그 값들이 갖는 확률을 나열하면 확률 분포가 된다.  
  
##### 2.3.1 independence
  - independence is the knowledge that one event occurs does not affedct the probability of the other event.
    - 독립성은 한 사건이 다른 사건의 확률에 영향을 미치지 않는다는 사실이다.  
  
#### 2.4 bayes' rule
  - actual 정보 3가지를 통해 prediction(예견, 추론)을 수행한다.
  - 측정기의 민감도와 특이도, 그리고 사건의 발생 확률을 알면 정확도(측정 시 양성일때 실제 양성일 확률)을 알 수 있다.  



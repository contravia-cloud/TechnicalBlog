---
title: "Probabilistic_robotics"
date: 2022-04-06

---

> PPROBABILITIC ROBOTICS, 에이콘, 세바스찬 스런,볼프람 부르가트,디터 폭스 지음, 남궁영환 옮김

# 01 소개
## 1.1 로보틱스 분양의 불확실성
- 로봇의 불확실성에 영향을 주는 요인  

|로봇 환경 | 본질적으로 예측 불가능함. 고속도로나 개인 주택 같은 호나경은 변화무쌍.|
|센서 | 센서의 물리적 제약(범위와 해상도), 노이즈 |
|로봇 액추에이션 | 소음, 마모, 고장 등 |
|로봇 소프트웨어 | 모델의 오차, 알고리즘 근사화 |  

- 이러한 불확실성을 잘 다루기 위해 이 책을 쓰게되었다.  

## 1.2 확률론적 로보틱스
  - 확률론적 로보틱스는 **로봇 인식(robot perception)** 과 **액션**의 불확실성을 주로 다룬다.  
  - 확률론적 로보틱스의 핵심 아이디어는 **확률 이론의 미적분학**  
  - 특정 상황이 가장 좋을 것이라는 최적화의 방법이 아니라 추측할 수 있는 전체 공간의 확률 분포를 기반으로 정보를 표현하는 것임  
    이를 통해 모호함과 빌리프의 정도를 수학적으로 나타낼 수 있으며 제어 선택을 통해 불확실성을 낮출 수 있다.
  - 로컬화 예(로봇 인식)
<img src="https://contravia-cloud.github.io/TechnicalBlog/assets/img/화면 캡처 2022-04-06 165948.png" width="60%" height="60%" title="확률론적 로보틱스 그림 1.1" alt="확률론적 로보틱스 그림 1.1"/>  
    - 첫번째 : 아무정보(사전정보, 센서가 감지 안되었을 때)가 없을 때
      - bel(X) 값은 모두 동일
    - 두번째 : z일때 - 센서에 문이 감지된 경우
      - P(zㅣx) 거리(x)에 있을 때 z(센서가 문을 감지)일 확률 분포
      - 이값을 bel에 업데이트하여 로봇이 위치할 확률분포가 만들어진다.
    - 세번째 : 로봇이 움직인 경우 bel은 같이 움직인다. 다만 움직임에 따라 확률값이 낮아지게 된다. 
    - 네번째 : 센서가 다시 한번 문을 감지하고 그때의 확률 분포 P(zㅣx)를 업데이트 한다.
    - 다섯번째 : 로봇이 움지이면 확률값이 낮아지게 된다.
    - 로봇 인식 문제는 **스테이트 추정 문제**이다. 로봇 위치에 대한 **사후 추정** 문제이다.
  - 두번째 예(로봇 플래닝 및 제어)
    - 확률론적 플래닝 기법은 **불확실성을 예측**하고 **정보 수집을 계획**할 수 있으며 이러한 계획은 **확률론적 제어 기법**으로 실현할 수 있다.

# 02 재귀 스테이트 추정
## 2.1 개요
  - 확률론적 로보틱스의 핵심은 센서 데이터에서 현재의 스테이트를 어떻게 추정하는지에 있다. 
  - 스테이트 변수를 복원하기 위한 탐색 과정을 스테이트 추정이라고 한다.
  - 확률론적 스테이트 추정 알고리즘은 모든 스테이트상에서 빌리프 확률 분포를 계산한다.  

## 2.2 확률의 기본 개념
  - 로보틱스에서는 확률 P(yㅣx)를 **생성 모델(generative model)** 이라고 하는데,
  - 스테이트 변수 X가 어떻게 센서 측정값 Y에 영향을 끼치는지 추상화된 수준으로 설명해주기 때문이다.  

## 2.3 로봇 환경의 인터랙션
  - 센서를 통해 확보되는 데이터는 노이즈가 섞여 있을 수 있어 바로 '이것이다'라고 감지할 수 없는 경우가 대부분이다.
  - 그래서 로봇은 로봇 환경의 스테이트를 고려하여 내부 빌리프를 유지한다.
  - 또한 로봇은 액추에이터를 통해 로봇 환경에 영향을 줄 수 있고 결과는 예측 불가일 수도 있다.  

### 2.3.1 스테이트 xₜ
  - 환경은 **스테이트**에 따라 그 특성이 정립된다.
  - 스테이트 변수는 다음과 같다.
    - 로봇 포즈(pose)
    - 로봇 조작(manipulation) : 로봇의 액추에이터 관련 환경설정(키네마틱 스테이트)
    - 로봇 속도와 관절 속도(동적 스테이트)
    - 환경 내에 있는 주변 객체들의 위치 및 피쳐
    - 움직이는 객체와 사람의 위치와 속도
    - 센서 고장 여부, 충전 잔량 등등
  - <span style="background-color:yellow">complete</span>
    - xₜ가 미래를 대상으로 했을 때 가장 좋은 예측 결과일 경우 완전하다라고 한다.
    - 즉 미래를 예측하는데 다른 과거의 스테이트나 추가적인 측정값이 필요 없는 경우
    - 이러한 조건을 충족시키는 시간 프로세스를 **마르코프 체인**이라고 한다.
  - <span style="background-color:yellow">incomplete state</span>
    - 모든 스테이트 변수를 고려할 수 없기에 작은 서브셋을 추출해서 스테이트 변수를 구현하게된다.
  - hybrid state space
    - 연속형 변수와 이산형 변수를 모두 포함하는 스테이트 공간을 하이브리드 스테이트 공간이라 한다.  

### 2.3.2 환경 인터랙션
  - 로봇과 로봇 환경 간의 인터랙션에는 두 가지 기본 유형이 있다.
  - 환경 센서 측정(enviroment sensor measurement)
    - 인식(perception) : 로봇 환경 스테이트의 정보를 얻는 프로세스
    - 측정(measurement) : 인터랙션의 결과, observation, percept 라고도 한다.
    - 로봇 환경 측정 데이터, zₜ : <U>스테이트에 대한 정보</U>
  - 제어 액션(control action)
    - 제어 데이터, uₜ : <U>스테이트의 변화에 관한 정보</U>

### 2.3.3 확률론적 생성 법칙
  - <sub>0:t-1</sub>
  - 
<span style="font-family:Cambria Math;"> xyd</span>


-----------------------------------------------------------------
> Uncertainty - Lecture 2 - CS50's Introduction to Artificial Intelligence with Python 2020  
  https://www.youtube.com/watch?v=D8RRq3TbtHU  
> https://wikidocs.net/122768

## 2. uncertainty(불확실성)
### 2.1 probability
#### 2.1.1 possible world
#### 2.1.2 Axioms in probibility 
  - 0 <= P(w) <= 1
  - ΣP(w) = 1  
  
#### 2.1.3 unconditional probability
  - Unconditional probability is the degree of belief in a proposition in the absence of any other evidence.
    - 비조건부 확률이란 다른 증거가 없을 때 명제(주사위 합이 7이다)에 대한 믿음의 정도(1/6)를 말한다.  
  
### 2.2 Conditional Probability
  - 주사위 두개를 던져 합이 7이나오는 확률은 비조건부 확률이된다. 이전 사건이 결과에 영향을 주지 않기 때문이다.  
    - 그러나 AI 등에서는 세계가 실제로 작동하는 방식에 대한 초기 지식이 있는 확률이다.
    - 따라서 이것은 조건부 확률이 되는 것이다.
  - conditional probability is degree of belief in a proposition given some evidence that has already been revealed.
    - 조건부 확률은 이미 밝혀진 몇 가지 증거가 주어진 명제에 대한 믿음의 정도이다.
  - P(오늘 비가올까ㅣ어제 비가왔다) 이때 조건부 확률을 계산하는 방법은?  
  
### 2.3 random variable
  - A random variable is a variable in probability theory with a domain of possible values that it can take on.
    - 무작위 변수(random variable)는 확률론에서 가능한 값의 영역을 갖는 변수이다.
    - 예를 들어 날씨가 무작위 변수이고 흐림, 맑음, 비 등이 취할 수 있는 값이 된다.
    - 무작위 변수는 확률 변수라 하고 그 값들이 갖는 확률을 나열하면 확률 분포가 된다.  
  - independence  
  - independence is the knowledge that one event occurs does not affedct the probability of the other event.
    - 독립성은 한 사건이 다른 사건의 확률에 영향을 미치지 않는다는 사실이다.  
  
### 2.4 bayes' rule
  - actual 정보 3가지를 통해 prediction(예견, 추론)을 수행한다.
  - 측정기의 민감도와 특이도, 그리고 사건의 발생 확률을 알면 정확도(측정 시 양성일때 실제 양성일 확률)를 알 수 있다.
  - 또한 측정 시 양성일때 등의 기준은 동일하고 이때 양성일 확률과 음성일 확률을 비교하고자 하면 측정 시 양성일때를 몰라도 되겠다.  

### 2.5 joint probability
  - X의 확률 분포와 Y의 확률 분포를 안다면 결합 확률 분포를 알게되고 이를 통해 조건부 확률을 계산할 수 있다.  

### 2.6 probability rules
  - negation
    - P(¬a) = 1 - P(a)
  - inclusion-exclusion
    - P(a ∨ b) = P(a) + P(b) - P(a ∧ b)
  - marginalization
    - P(a) = P(a,b) + P(a,¬b)
    - P(X = xᵢ) = Σⱼ P( X = xᵢ , Y = yⱼ )
  - conditioning (전체 확률의 법칙, law of total probability)
    - P(a) = P(aㅣb)P(b) + P(aㅣ¬b)P(¬b)
    - P(X = xᵢ) = Σⱼ P( X = xᵢ ㅣ Y = yⱼ)P(Y = yⱼ)  

### 2.7 Bayesian networks
> 컴퓨터 내부 또는 AI 에이전트에서 정보, 확률, 사건들의 가능성 등을 나타낼 수 있는 방법(확률 모델로서)으로 Bayesian networks를 도입한다.  

  - Bayesian network is data structure that represents the dependencies among random variables.
    - 베이지안 네트워크는 무작위 변수(확률 변수)들 사이의 의존성을 나타내는 데이터 구조이다.
  - 베이지안 네트워크는
    - directed graph
    - each node represents a random variable
    - arrow from X to Y means X is a parent of Y
    - each node X has probability distribytion P(XㅣParents(X))
  - inference
    - Query X : the variable for which we want to compute the probability distribution
    - evidence variables E : one or more variables that have been observed for event e
    - hidden variables Y : variables that aren't the query and also haven't been observed  
    - 추론은 증거(E)가 주어졌을 때 원하는 X의 분포를 계산해내는 것을 말하는데 이때 X가 아닌 다른 변수들은 Y가되어 모든 경우를 고려한 X를 계산하게 된다.

### python 구현
> pomegranate : https://pomegranate.readthedocs.io/en/latest/  



 
 
 
 
 
 

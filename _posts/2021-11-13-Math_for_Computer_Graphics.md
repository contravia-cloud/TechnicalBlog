---
title: "Math for CG"
date: 2021-10-30
categories: 
tag: [math, computer graphics]
---

# 1. 서론
- 컴퓨터 그래픽스 제작 단계  
    - 모델링 -> 리깅(rigging) -> 애니메이션 -> 랜더링 -> 후처리
    - 모델링 : 물체를 컴퓨터가 처리할 수 있는 방식으로 표현(폴리곤 메시)하는 것
        - 폴리곤 메시 : 폴리곤으로 구성된 물체
    - 리깅 : 골격(skelton)을 구성한 후, 각각의 뼈(bone)의 움직임이 폴리곤 메시에 어떻게 반영될 것인가 정의한다.
    - 애니메이션 : 골격을 움직여 애니메이션을 정의
    - 랜더링 : 애니메이션 스크린 샷을 2차원 프레임으로 만드는 작업 (텍스처링, 라이팅)
    - 후처리 : 모션 블러, 초점 심도 처리
- 그래픽스 API
    - 게임프로그램 - 게임 엔진 - 그래픽스 API - GPU
    - 게임 엔진은 그래픽스 API를 기반으로 개발되며 그래픽스 API는 Direct3D와 OpenGL이 있다.  

# 2. 수학의 기초
### 1. 행렬과 벡터
- OpenGL은 열벡터를 사용하고 Direct3D는 행벡터를 사용한다.
- 정규화(normalization)  
벡터 $v$를 그 길이 $\vert \vert v \vert \vert$로 나누는 과정을 정규화라고 한다.
- 단위 벡터  
정규화를 통해 길이가 1인 벡터, $v/\vert \vert v \vert \vert$

### 2. 좌표계와 기저
 - 선형독립  
 ![Alt text](https://contravia-cloud.github.io/TechnicalBlog/assets/img/2545C24E569D04232C.jpg)




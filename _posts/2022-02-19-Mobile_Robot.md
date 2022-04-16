---
title: "Mobile_Robot"
date: 2022-02-19

---

# 요소기술  

> http://smroadmap.smtech.go.kr/  
> 논문 : 세타*: 동적 가중치를 가진 계층 구조식 경로계획법  
   https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=DIKO0014017925&dbt=DIKO  
> https://www.mdpi.com/2624-8921/3/3/27/pdf  
> https://miracleyoo.tistory.com/21  



  - 데이터 셋 : BDD100K, KITTI

  - 경로 계획
    - 경로생성 기술
      - A*
      - RRT/RRT*
    - 경로추종 기술 (조향제어...)
      - Pure Pursuit Control (기하학적 접근방법, 다양한 동력학적 변수가 제외됨)
        - 차량이 진행할 경로의 목표점을 잇는 원호의 곡률을 기하학적으로 계산
      - Stanley 방법
      - PID 방법 (closed loop, 게인값에 따라서 오실레이션이 있을 수 있다.)
      - Model Predictive Control(모델예측 접근 방법, 현재정보+예측정보 활용)

  - SLAM (mapping + localization)
    - visual SLAM
      - kidnapping - 위치 재조정
      - 지도 추가
      - monocular SLAM
        - 카메라 자세추정 : 서로 다른 시점에서 촬영된 이미지로 3차원 위치 추정
        - 카메라 트래킹 : 특징점을 이용해서 카메라가 지도 내에 어디 있는지 추정
      - 기술 : 특징 추출, 특징 매칭, 카메라 자세추정(삼각측량법-> 초기지도작성), 카메라 트래킹
      - 특징 매칭 시 강건성 확보 알고리즘: 순환 일관성(cycle consistency), epipolar 제약, 호모그라피 
      - 전역지도관리
        - 국소지도확장 -> 오차 보정 -> 전역지도정합(전역지도 최적화)  
           : 번들 조정(정합), 루프결합, 자세그래프 최적화 그리고 카메라 자세 놓쳤을 때 최적화카메라 위치 재조정
  - 인지기술
    - 센서 융합
      - data level fusion, feature level fusion, decision level fusion
      - 확률이론 기반 센서융합 기법(칼만필터 기법,파티클 필터 기법), 인공지능기반 센서융합 기법
    - 객체인식  
      - 


  - intelligent vehicle의 기본 기술, GNC
    - guidance(planning) : 경로 설계 (pursuit guidance/proportional navigation guidance)
    - navigation(항법,perception) : 위치파악 (sensing & perception, dead-reckoning )
    - control(action) : 경로 설계된 내용을 수행

![image](https://user-images.githubusercontent.com/57220434/162558061-73549c49-d3a4-44f9-a20a-6b4317289a53.png)
![image](https://user-images.githubusercontent.com/57220434/162558319-1a38506b-ed8e-4e4e-ad98-6eedd7e37a5c.png)
![image](https://user-images.githubusercontent.com/57220434/162558555-863f4737-1ab7-4099-b7c1-23d03a6a13ec.png)
![image](https://user-images.githubusercontent.com/57220434/162558566-681f309b-6cc8-40c8-80f8-ab0d4b191553.png)
![image](https://user-images.githubusercontent.com/57220434/162558722-a87f8805-a6d5-485e-bfef-59d8cce0976e.png)

-----------------------  
> self driving vehicle 

# 


### 개념어

  - DDS (Data Distribution Service)  
    - <img src="https://www.researchgate.net/profile/Takuya-Azumi/publication/309128426/figure/fig1/AS:416910068994049@1476410514667/ROS1-ROS2-architecture-for-DDS-approach-to-ROS-We-clarify-the-performance-of-the-data.png" width="60%" height="60%" title="타이틀" alt="image"/>  
    image from : Exploring the Performance of ROS 2 <https://www.researchgate.net/publication/309128426_Exploring_the_performance_of_ROS2>
    - DDS(Data Distribution Service)는 OMG(http://www.omgwiki.org/dds/)에서 국제 표준으로 정한 실시간 데이터 분배 미들웨어 입니다.  
  - GPG (GNU Privacy Guard, GnuPG 또는 GPG)

### linux
  - source
    - source 명령어는 스크립트 파일을 수정한 후에 수정된 값을 바로 적용하기 위해 사용하는 명령어이다.
    - 예륻들어 /etc/bashrc 파일을 수정 후 저장하여도 수정한 내용이 바로 적용되지 않는다. 왜냐하면 /etc/bashrc 파일은 유저가 로그인할 때 읽어들이는 파일이여서, 로그아웃 후 로그인하거나 리눅스를 재시작해야 적용이 된다.
  - mkdir -p : 하위폴더까지 만듬

### 개발환경 setup  
  - Colcon Build system
  - workspace 생성

### 실행 순서  
  - source /opt/ros/foxy/setup.bash  
  - rosdep install -i --from-path src --rosdistro foxy -y  #*종속성 확인*
  - colcon build --symlink-install  --packages-select  #*--symlink-install : install 파일 변경*  
    
  - source ~/gcamp_ros2_ws/install/local_setup.bash  
  - ros2 run turtlesim turtlesim_node  


----------------
# 자율주행자동차기술  
> http://www.kmooc.kr/dashboard?status=audit  


![image](https://user-images.githubusercontent.com/57220434/162551853-aefc3e56-e2e7-49ea-92c5-b63c8c035c77.png)











---
title: "Mobile_Robot"
date: 2022-02-19

---

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











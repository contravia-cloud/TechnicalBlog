---
title: "Mobile_Robot 구현"
date: 2022-03-05

---


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

-----


### docker
  - https://89douner.tistory.com/123
  - 도커 입문편 : https://www.44bits.io/ko/post/easy-deploy-with-docker
  - 도커 설치 : ```curl -s https://get.docker.com | sudo sh```
  - 설치 확인 : ```docker -v```
  - 설치 확인 : ```dpkg --get-selections | grep docker```
  - 작업자를 docker 신규그룹으로 가입 : ```sudo usermod -aG docker $USER```
  - docker 명령어
    - docker ps : 프로세스 확인, unix 계열의 프로세스 확인(ps)과 동일
    - docker ps -a : 모든 컨테이너 확인
    - docker pull ubuntu:bionic : 이미지 다운
    - docker image : 다운받은 이미지 표시
    - docker run -it centos:latest bash, 이미지가 없으면 다운받고, 컨테이너를 만들어, 컨테이너를 실행하고(restart), bash로 인터페이스(attach) 진행
    - docker restart <CONTAINER ID> : 컨테이너 실행
    - docker attach <CONTAINER ID> : 터미널과 연결
  - 도커 디스플레이 연결 - X11 소켓을 통한 연결 방법
  ```
  docker run -it \
    --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --device=/dev/dri:/dev/dri \
    osrf/ros:galactic-desktop
  ```
  - 도커 공유폴더
     docker run -it -v /home/contraviau/test_share:/test_share -p 8080:8080 osrf/ros:ssj1600
  

### vnc
  - 도커 컨테이너 내에서 ```sudo apt-get update```
  - ```sudo apt-get install wget```
  - ```wget https://github.com/novnc/noVNC/archive/refs/tags/v1.3.0.tar.gz```
  - 압축 풀기 ```tar xvzf v1.3.0.tar.gz```
  - ```./utils/novnc_proxy --vnc localhost:5901```
  
### novnc
  - ```docker run --rm -it osrf/ros:galactic-desktop```
  - download https://github.com/theasp/docker-novnc
  - 
  - Dockerfile edit https://github.com/theasp/docker-novnc/blob/master/Dockerfile
```
  FROM osrf/ros:galactic-desktop
  RUN apt-get update
  RUN set -ex; \
    apt-get update; \
    apt-get install -y \
      bash \
      fluxbox \
      git \
      net-tools \
      novnc \
      supervisor \
      x11vnc \
      xterm \
      xvfb
  ENV HOME=/root \
    DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    LC_ALL=C.UTF-8 \
    DISPLAY=:0.0 \
    DISPLAY_WIDTH=1024 \
    DISPLAY_HEIGHT=768 \
    RUN_XTERM=yes \
    RUN_FLUXBOX=yes
  COPY . /app
  CMD ["/app/entrypoint.sh"]
  RUN chmod +x /app/entrypoint.sh
  EXPOSE 8080
```
  - ```docker build -t osrf/ros:ssj /home/contravia/make-by-dockerfile```
  - ```docker run --rm -it -p 8080:8080 osrf/ros:ssj```
  - ```http://192.168.25.24:8080/vnc.html```
  
### 크롬 설치 및 실행
  - wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
  - sudo apt install ./google-chrome=stable_current_amd64.deb
  - /opt/google/chrome/chrome --no-sandbox --disable-dev-shm-usag

###  vscode 설치 및 실행
  - 브라우저에서 다운로드
  - root/Download 로 이동
  - sudo apt install ./파일명
  - code --no-sandbox --user-data-dir


###  example
  - https://github.com/robotpilot/ros2-seminar-examples
  - https://docs.ros.org/en/galactic/Tutorials/Workspace/Creating-A-Workspace.html
  - rosdep install -i --from-path src --rosdistro galactic -y
  

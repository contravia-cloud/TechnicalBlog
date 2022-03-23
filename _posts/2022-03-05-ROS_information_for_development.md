---
title: "ROS_information_for_development"
date: 2022-03-05

---

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
  

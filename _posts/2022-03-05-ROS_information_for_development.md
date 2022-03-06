---
title: "ROS_information_for_development"
date: 2022-03-05

---

### docker
  - 도커 입문편 : https://www.44bits.io/ko/post/easy-deploy-with-docker
  - 도커 설치 : curl -s https://get.docker.com | sudo sh
  - 설치 확인 : docker -v  
  - 설치 확인 : dpkg --get-selections | grep docker
  - 작업자를 docker 신규그룹으로 가입 : sudo usermod -aG docker $USER
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

---
title: "Elastic stack"
date: 2021-12-14

---

---------------------------------------  
# 처음부터 시작하는 Elastic
> https://www.youtube.com/playlist?list=PLhFRZgJc2afp0gaUnQf68kJHPXLG16YCf
https://ela.st/es-resource-kr  

```
curl -X POST "localhost:9200/logs-my_app-default/_doc?pretty" -H 'Content-Type: application/json' -d'
{
  "@timestamp": "2099-05-06T16:21:15.000Z",
  "event": {
    "original": "192.0.2.42 - - [06/May/2099:16:21:15 +0000] \"GET /images/bg.jpg HTTP/1.0\" 200 24736"
  }
}
'
```

  - 우분투 사용자 변경  
  sudo su : root 사용자로 변경 #  
  su [사용자] : 사용자로 변경 $  
  - 설치파일 다운로드  
  wget ~  
  - 압축풀기  
  tar xfz ~  
  - 실행하기  
  bin/elasticsearch  
  - 확인하기  
  curl -XGET localhost:9200  
  - host 설정
  vi config/elasticsearch.yml  
  ```
  cluster.name: "cluster1"  
  node.name: "node1"
  network.host: "_site_"
  discovery.seed_hosts: ["DESKTOP-******"]
  cluster.initial_master_nodes: ["node1"]
  ```
  vi /etc/sysctl.conf  
  ```
  vm.max_map_count=262144
  ```
  - 방화벽 설정  
  sudo ufw allow 9200  
  - kibana 설정  
  config/kibana.yml  
  ```
  server.host: "DESKTOP-********"
  server.name: "my-kibana"
  elasticsearch.hosts: ["http://192.168.25.24:9200"]
  ```







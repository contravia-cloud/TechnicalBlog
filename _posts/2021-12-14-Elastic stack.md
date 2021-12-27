---
title: "Elastic stack"
date: 2021-12-14

---

---------------------------------------  
# 처음부터 시작하는 Elastic
> https://www.youtube.com/playlist?list=PLhFRZgJc2afp0gaUnQf68kJHPXLG16YCf
https://ela.st/es-resource-kr  
elastic 가이드북 : https://esbook.kimjmin.net/  

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

---------------------------------------  
# Elastic IoT 연결  

## Machinebeat test with opcua  
  - https://mingsigi.tistory.com/entry/ElasticStack-33-Machinebeat-test-with-opcua  

### opc-ua 테스트 서버 구축  
```
from time import sleep
import random
from opcua import Server

server = Server()
server.set_endpoint("opc.tcp://127.0.0.1:12345")
server.register_namespace("Room1")
objects = server.get_objects_node()
tempsens = objects.add_object('ns=2;s="TS1"', "Temperature Sensor 1")
tempsens.add_variable('ns=2;s="TS1_VendorName"', "TS1 Vendor Name", "Sensor King")
tempsens.add_variable('ns=2;s="TS1_SerialNumber"', "TS Serial Number", 12345678)
temp = tempsens.add_variable('ns=2;s="TS1_Temperature"', "TS1 Temperature", 20)
bulb = objects.add_object(2, "Light Bulb")
state = bulb.add_variable(2, "State of Light Bulb", False)
state.set_writable()
temperature = 20.0
try: 
  print("Start Server")
  server.start()
  print("Server Online")
  while True:
    temperature = random.uniform(-1, 1)
    temp.set_value(temperature)
    print("New Temperature: " +str(temp.get_value()))
    print("State of LIght bulb: "+str(state.get_value()))
    sleep(2)
finally:
server.stop()
print("Server Offline")

출처: https://mingsigi.tistory.com/entry/opc-ua-test-server-via-python [Gibbs Kim's playground]
```

### machinebeat 파일 다운로드  
  - https://github.com/elastic/Machinebeat  
  - 아직 릴리즈되진 않고 프로토타입임.  
  - 

### MTCONNECT  
  - DEVICE(장비) - ADAPTER(S/W, H/W) - AGENT - APPLICATION  
  - 실행방법 참조 : https://www.youtube.com/watch?v=TwhSCewjjmE  
  - ADAPTER에 focas 라이브러리를 감싼 통신 프로그램을 설치한다.  

### Kafka OPC UA  
  - https://www.confluent.io/hub/onewayautomation/ogamma-visual-logger-for-opc  
  - confluent에서 connector를 제공해주고 있다. tag 제한이 있다.  







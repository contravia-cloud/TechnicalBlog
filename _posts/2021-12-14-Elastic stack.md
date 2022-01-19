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


---------------------------------------  
# 엘라스틱 스택 개발부터 운영까지  
> 김준영 정상운 지음, 책만  

## 4. 엘라스틱서치: 검색  
> 학습 내용 :  
쿼리 컨텍스트와 필터 컨텍스트의 차이  
쿼리 컨텍스트와 쿼리 DSL의 차이  
쿼리 컨텍스트에서의 스코어 계산 알고리즘  
쿼리 종류 : 전문 궈리, 매치쿼리, 용어 궈리, 멀티 매치 쿼리, 범위 쿼리, 논리 쿼리, 패턴 검색 등  


### 4.1 쿼리 컨텍스트와 필터 컨텍스트  
  - 쿼리 컨텍스트 : 연관성에 따른 스코어 결과를 제공  
  - 필터 컨텍스트 : '예/아니오'의 결과를 제공  
```
{
  "query" : {
    "match" : {
      "category" : "clothing"
    }
  }
}
```
### 4.2 쿼리 스트링과 쿼리 DSL  
  - 쿼리 스트링 : 한줄 정도의 간단한 쿼리에 사용  
  - 쿼리 DSL : domain specific language, 복잡한 쿼리에 사용  

### 4.3 유사도 스코어
  - TF-IDF 알고리즘, BM25알고리즘 등이 있음
  - IDF(inverse document frequency)  
    - 문서 빈도의 역수  
    - 전체 문서에서 자주 발생하는 단어일수록 중요하지 않은 단어로 인식하고 가중치를 낮추는 것  
    - 'to', 'the' 같은 관사나 접속사 등  
  - TF(term frequency)  
    - 용어 빈도  
    - 특정 용어가 하나의 도큐먼트에 얼마나 많이 등장했는지를 의미하는 지표  
    - 특정 용어가 하나의 도큐먼트에서 많이 반복되었다면 그 용어는 도큐먼트의 주제와 연관되어 있을 확률이 높다.  

### 4.4 쿼리  
  - 리프 쿼리 : 매치 쿼리, 용어 쿼리, 범위 쿼리  
  복합 쿼리 : 논리 쿼리  
    - 매치 쿼리 : 전문 쿼리, 전체 켁스트 중에서 특정 용어나 용어들을 검색할 때 사용 ('데이터 분석' -> '데이터' , '분석')  
    - 용어 쿼리 : 검색어가 토큰화 되지 않는다. ('데이터 분석' -> '데이터' , '분석')  
    - 논리 쿼리 : must, must not, should, filter





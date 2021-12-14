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


---
title: SkyWalking 数据存储在es的每一个索引的含义
date: 2022-07-11 15:11:00
tags: Java,SkyWalking,Spring,Linux,WuSheng,Agent,Monitor,ElasticSearch
---

# SkyWalking 数据存储在es的每一个索引的含义

## andotorg_instance_traffic-20220629

> 该索引存储的是所有service保存的实例

### 存储数据示例

```json
{
        "_index" : "andotorg_instance_traffic-20220629",
        "_type" : "type",
        "_id" : "23.23",
        "_score" : 1.0,
        "_source" : {
          "bcy_timestamp" : "2022-06-29T02:26:57.109845Z",
          "last_ping" : 202206290230,
          "service_id" : "dsd.1",
          "name" : "sdsdsd@1.1.1.1",
          "time_bucket" : 202206290226,
          "properties" : ""
        }
      }
```

## andotorg_segment-20220629

> 该索引存储的是所有请求链接对应数据

```json
GET /andotorg_segment-20220629/_search
{
  "query": {
    "bool": {
      "must": [
        {"range": {
          "time_bucket": {
            "gte": 20220629120350,
            "lte": 20220629130350
          }
        }}
      ]
    }
  }, 
  "sort": [
    {
      "end_time": {
        "order": "desc"
      }
    }
  ]
}
```

## andotorg_metrics-doubleavg-20220519

## andotorg_service_instance_relation_client_side-20220519

## andotorg_profile_task-20220519

## andotorg_profile_task_segment_snapshot-20220519

## andotorg_profile_task_log-20220519

## andotorg_log-20220519

> 通过GRPC appender 上报的日志数据

## andotorg_events-20220629

## andotorg_endpoint_relation_server_side-20220629

## andotorg_service_instance_relation_client_side-20220629

## andotorg_network_address_alias-20220701

> 存储的网络地址，有address是存储的全量的service的名称

## andotorg_browser_error_log-20220629

> 存储的是前端收集的错误信息

## andotorg_alarm_record-20220629

> 告警记录表

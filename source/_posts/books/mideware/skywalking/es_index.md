# 存储在es的每一个索引的含义

## prod_bcy_instance_traffic-20220629

> 该索引存储的是所有service保存的实例

### 存储数据示例

```json
{
        "_index" : "prod_bcy_instance_traffic-20220629",
        "_type" : "type",
        "_id" : "YmN5LWs4cy1jb21wZXRpdGlvbi10ZXN0.1_MWQ2MjUzMjI0MTUwNGQ1Nzg0NTJmYzEyNzVkYjAwZWZAMTcyLjIwLjEuMTQw",
        "_score" : 1.0,
        "_source" : {
          "bcy_timestamp" : "2022-06-29T02:26:57.109845Z",
          "last_ping" : 202206290230,
          "service_id" : "YmN5LWs4cy1jb21wZXRpdGlvbi10ZXN0.1",
          "name" : "1d62532241504d578452fc1275db00ef@172.20.1.140",
          "time_bucket" : 202206290226,
          "properties" : ""
        }
      }
```

## prod_bcy_segment-20220629

> 该索引存储的是所有请求链接对应数据

```json
GET /prod_bcy_segment-20220629/_search
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

## bcy-prod_metrics-doubleavg-20220519

## bcy-prod_service_instance_relation_client_side-20220519

## bcy-prod_profile_task-20220519

## bcy-prod_profile_task_segment_snapshot-20220519 

## bcy-prod_profile_task_log-20220519

## bcy-prod_log-20220519

> 通过GRPC appender 上报的日志数据

## prod_bcy_events-20220629

## prod_bcy_endpoint_relation_server_side-20220629

## prod_bcy_service_instance_relation_client_side-20220629

## prod_bcy_network_address_alias-20220701

> 存储的网络地址，有address是存储的全量的service的名称

## prod_bcy_browser_error_log-20220629

> 存储的是前端收集的错误信息

## prod_bcy_alarm_record-20220629

> 告警记录表

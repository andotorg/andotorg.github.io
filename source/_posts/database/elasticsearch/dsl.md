# DSL

## 列出所有索引

```json
GET /_cat/indices

```

## 只返回指定的字段的数据

```json
GET /andotorg_instance_traffic-20220629/_search
{
    "_source": [ "name", "service_id", "andotorg_timestamp"]
}
```

或者是包含谁，不包含谁

```json
"_source": {
    "includes": [ "obj1.*", "obj2.*" ],
    "excludes": [ "*.description" ]
}
```

## 
# Redis Tools

Key-value operations on the Redis instance.

## redis_get / redis_set
```bash
sulla redis/get '{"key":"mykey"}'
sulla redis/set '{"key":"mykey","value":"hello"}'
```

## redis_del
```bash
sulla redis/del '{"key":"mykey"}'
```

## redis_incr / redis_decr
```bash
sulla redis/incr '{"key":"counter"}'
sulla redis/decr '{"key":"counter"}'
```

## redis_expire / redis_ttl
```bash
sulla redis/expire '{"key":"mykey","seconds":3600}'
sulla redis/ttl '{"key":"mykey"}'
```

## redis_hset / redis_hget / redis_hgetall
```bash
sulla redis/hset '{"key":"user:1","field":"name","value":"Alice"}'
sulla redis/hget '{"key":"user:1","field":"name"}'
sulla redis/hgetall '{"key":"user:1"}'
```

## redis_rpush / redis_lpop
```bash
sulla redis/rpush '{"key":"queue","values":["task1","task2"]}'
sulla redis/lpop '{"key":"queue"}'
```

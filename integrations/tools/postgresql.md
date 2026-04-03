# PostgreSQL Tools

Direct database operations on PostgreSQL.

## pg_query
```bash
sulla pg/query '{"query":"SELECT * FROM users LIMIT 10"}'
```

## pg_queryone
```bash
sulla pg/queryone '{"query":"SELECT * FROM users WHERE id = $1","params":["123"]}'
```

## pg_queryall
```bash
sulla pg/queryall '{"query":"SELECT name FROM products"}'
```

## pg_count
```bash
sulla pg/count '{"query":"SELECT COUNT(*) FROM orders WHERE status = $1","params":["pending"]}'
```

## pg_execute
```bash
sulla pg/execute '{"query":"UPDATE users SET active = true WHERE id = $1","params":["123"]}'
```

## pg_transaction
```bash
sulla pg/transaction '{"statements":["INSERT INTO logs (msg) VALUES ($1)","UPDATE counters SET n = n + 1"],"params":[["hello"],[]]}'
```

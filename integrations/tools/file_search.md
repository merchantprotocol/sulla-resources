# file_search

Fast semantic search across any directory using QMD vector indexing. Faster and more comprehensive than find or grep.

```bash
sulla meta/file_search '{"query":"authentication middleware","dirPath":"/home/user/project"}'
```

## Parameters

| Param     | Type    | Required | Description                                    |
|-----------|---------|----------|------------------------------------------------|
| `query`   | string  | yes      | Keywords, concepts, or questions to search for |
| `dirPath` | string  | no       | Directory to search (defaults to home)         |
| `limit`   | number  | no       | Max results (default 20)                       |
| `reindex` | boolean | no       | Force re-index before searching                |

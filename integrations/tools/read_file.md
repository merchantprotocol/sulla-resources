# read_file

Read the contents of a file with optional line range.

```bash
sulla meta/read_file '{"path":"/home/user/project/src/app.ts"}'
sulla meta/read_file '{"path":"/home/user/project/src/app.ts","startLine":10,"endLine":50}'
```

## Parameters

| Param       | Type   | Required | Description              |
|-------------|--------|----------|--------------------------|
| `path`      | string | yes      | Absolute file path       |
| `startLine` | number | no       | Start reading from line  |
| `endLine`   | number | no       | Stop reading at line     |

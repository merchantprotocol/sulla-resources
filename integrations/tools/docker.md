# Docker Tools

Manage Docker containers and images.

## docker_ps
```bash
sulla docker/ps '{}'
```
List running containers. Pass `{"all": true}` to include stopped containers.

## docker_run
```bash
sulla docker/run '{"image":"nginx:latest","name":"my-nginx","ports":["8080:80"]}'
```

| Param     | Type     | Required | Description                    |
|-----------|----------|----------|--------------------------------|
| `image`   | string   | yes      | Docker image to run            |
| `name`    | string   | no       | Container name                 |
| `ports`   | string[] | no       | Port mappings (e.g. `8080:80`) |
| `env`     | string[] | no       | Environment variables          |
| `volumes` | string[] | no       | Volume mounts                  |
| `detach`  | boolean  | no       | Run in background              |

## docker_exec
```bash
sulla docker/exec '{"container":"my-nginx","command":"ls /etc/nginx"}'
```

## docker_logs
```bash
sulla docker/logs '{"container":"my-nginx","tail":"50"}'
```

## docker_stop
```bash
sulla docker/stop '{"container":"my-nginx"}'
```

## docker_rm
```bash
sulla docker/rm '{"container":"my-nginx"}'
```

## docker_images
```bash
sulla docker/images '{}'
```

## docker_pull
```bash
sulla docker/pull '{"image":"postgres:16"}'
```

## docker_build
```bash
sulla docker/build '{"path":"/app","tag":"my-app:latest"}'
```

# Lima VM Tools

Manage Lima virtual machine instances.

## lima_list
```bash
sulla lima/list '{}'
```

## lima_shell
```bash
sulla lima/shell '{"instance":"0","command":"uname -a"}'
```

## lima_start
```bash
sulla lima/start '{"instance":"0"}'
```

## lima_stop
```bash
sulla lima/stop '{"instance":"0"}'
```

## lima_create
```bash
sulla lima/create '{"name":"dev","template":"ubuntu"}'
```

## lima_delete
```bash
sulla lima/delete '{"instance":"dev"}'
```

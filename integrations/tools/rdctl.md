# rdctl Tools

Manage Sulla Desktop from the command line.

## rdctl_info
```bash
sulla rdctl/info '{}'
```
Return information about Sulla Desktop.

## rdctl_list_settings
```bash
sulla rdctl/list_settings '{}'
```
List current settings.

## rdctl_set
```bash
sulla rdctl/set '{"settings":{"kubernetes":{"enabled":true}}}'
```
Update Sulla Desktop settings.

## rdctl_shell
```bash
sulla rdctl/shell '{"command":"uname -a"}'
```
Run a command in the Sulla Desktop VM.

## rdctl_extension
```bash
sulla rdctl/extension '{"action":"list"}'
sulla rdctl/extension '{"action":"install","id":"extension-id"}'
```

## rdctl_snapshot
```bash
sulla rdctl/snapshot '{"action":"list"}'
sulla rdctl/snapshot '{"action":"create","name":"before-upgrade"}'
```

## rdctl_start
```bash
sulla rdctl/start '{}'
```

## rdctl_shutdown
```bash
sulla rdctl/shutdown '{}'
```

## rdctl_reset
```bash
sulla rdctl/reset '{}'
```

## rdctl_version
```bash
sulla rdctl/version '{}'
```

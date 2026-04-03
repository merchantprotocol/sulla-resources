# exec

Execute a shell command inside the Lima VM. This is the primary way to run commands — it has full root-level access to the VM's filesystem, Docker, and all installed tools including `sulla`.

```bash
sulla meta/exec '{"command":"ls -la /tmp"}'
```

## Parameters

| Param     | Type   | Required | Description                                    |
|-----------|--------|----------|------------------------------------------------|
| `command` | string | yes      | Shell command to run                           |
| `cwd`     | string | no       | Working directory inside the VM                |
| `timeout` | number | no       | Timeout in ms (default 120000)                 |

## Common Patterns

```bash
# Run sulla tools
sulla meta/exec '{"command":"sulla local_merchant_protocol/twenty '{\"method\":\"GET\",\"path\":\"/rest/companies\"}'"}'

# Docker operations
sulla meta/exec '{"command":"docker ps"}'

# File operations
sulla meta/exec '{"command":"cat /etc/hosts"}'

# Install packages
sulla meta/exec '{"command":"apk add curl jq","timeout":60000}'

# Git operations
sulla meta/exec '{"command":"cd /home/user/project && git status"}'
```

## Environment

- Runs inside the Lima VM (Linux), not on the host macOS
- Has access to Docker, kubectl, git, and all CLI tools
- Home directory is the macOS home dir mounted via virtiofs
- The `sulla` CLI is available on PATH for making proxy and tool calls

# GitHub & Git Tools

Git operations and GitHub API interactions.

## Git Operations

### git_status
```bash
sulla github/git_status '{}'
```
Current branch, staged/unstaged/untracked files.

### git_log
```bash
sulla github/git_log '{"limit": 10}'
```

### git_diff
```bash
sulla github/git_diff '{"staged": true}'
sulla github/git_diff '{"filePath": "src/app.ts"}'
```

### git_add
```bash
sulla github/git_add '{"files": ["src/app.ts", "src/utils.ts"]}'
```

### git_commit
```bash
sulla github/git_commit '{"message": "fix: resolve login bug"}'
```

### git_push
```bash
sulla github/git_push '{"remote": "origin", "branch": "main"}'
```

### git_pull
```bash
sulla github/git_pull '{"remote": "origin"}'
```

### git_branch
```bash
sulla github/git_branch '{"action": "create", "branchName": "feature/new-thing"}'
sulla github/git_branch '{"action": "list"}'
```

### git_checkout
```bash
sulla github/git_checkout '{"ref": "main"}'
```

### git_stash
```bash
sulla github/git_stash '{"action": "push"}'
sulla github/git_stash '{"action": "pop"}'
```

### git_blame
```bash
sulla github/git_blame '{"filePath": "src/app.ts", "startLine": 10, "endLine": 20}'
```

### git_conflicts
```bash
sulla github/git_conflicts '{}'
```

## GitHub API

### github_create_issue
```bash
sulla github/create_issue '{"owner":"org","repo":"project","title":"Bug report","body":"Details..."}'
```

### github_get_issues
```bash
sulla github/get_issues '{"owner":"org","repo":"project","state":"open"}'
```

### github_create_pr
```bash
sulla github/create_pr '{"owner":"org","repo":"project","title":"Add feature","head":"feature-branch","base":"main"}'
```

### github_read_file
```bash
sulla github/read_file '{"owner":"org","repo":"project","path":"README.md"}'
```

# Workflow Tools

Execute and manage Sulla workflows. Workflows are YAML-defined multi-step automations that chain tools and agent actions together.

## execute_workflow

```bash
sulla meta/execute_workflow '{"slug":"my-workflow"}'
```

| Param  | Type   | Required | Description                    |
|--------|--------|----------|--------------------------------|
| `slug` | string | yes      | Workflow slug (from filename)  |

Workflow definitions live in `~/sulla/workflows/`. The slug matches the filename without the extension.

## validate_sulla_workflow

```bash
sulla meta/validate_sulla_workflow '{"filePath":"/path/to/workflow.yaml"}'
```

Validate a workflow YAML file for structural correctness before running it. Catches missing nodes, broken connections, and invalid tool references.

## restart_from_checkpoint

```bash
sulla meta/restart_from_checkpoint '{"executionId":"exec-123","nodeId":"node-5"}'
```

Restart a failed or paused workflow from a specific node. Useful when a workflow fails partway through and you want to resume from where it left off instead of starting over.

| Param         | Type   | Required | Description                         |
|---------------|--------|----------|-------------------------------------|
| `executionId` | string | yes      | ID of the workflow execution        |
| `nodeId`      | string | yes      | Node ID to restart from             |

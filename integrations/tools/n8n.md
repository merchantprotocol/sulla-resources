# n8n Workflow Tools

Manage and diagnose n8n workflows.

## validate_workflow
```bash
sulla n8n/validate_workflow '{"workflowId":"123"}'
```
Validate an existing workflow and return graph health issues.

## validate_workflow_payload
```bash
sulla n8n/validate_workflow_payload '{"payload":{...}}'
```
Validate a workflow payload before creating or updating.

## patch_workflow
```bash
sulla n8n/patch_workflow '{"workflowId":"123","operations":[...]}'
```
Apply node and connection add/update/remove operations to a workflow.

## diagnose_webhook
```bash
sulla n8n/diagnose_webhook '{"workflowId":"123"}'
```
Check webhook readiness — DB registration, logs, and status.

## restart_n8n_container
```bash
sulla n8n/restart_n8n_container '{}'
```
Restart the n8n Docker container and check webhook registration.

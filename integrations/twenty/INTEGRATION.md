---
slug: twenty
name: Twenty CRM
type: proxy
auth_type: bearer
account_id: local_merchant_protocol
---

# Twenty CRM Integration

## How It Works

`sulla` is a lightweight shell script available on PATH inside the VM. It sends your request to the Sulla Desktop proxy, which:

1. Resolves the base URL and bearer token from the encrypted vault
2. Injects the credentials into the request
3. Forwards to the Twenty API
4. Returns the raw upstream response (status code, body, errors — everything)

You never handle credentials, tokens, or base URLs. They stay in the vault.

If the `sulla-daemon` is running, `sulla` uses the local socket for speed. Otherwise it falls back to direct TCP. Either way it just works.

## Usage

Use the `exec` tool to run `sulla` commands:

```
sulla <account_id>/twenty '<json_body>'
```

The account_id for this integration is **`local_merchant_protocol`**.

**JSON body fields:**

| Field     | Type   | Required | Description                                       |
|-----------|--------|----------|---------------------------------------------------|
| `method`  | string | yes      | HTTP method: GET, POST, PUT, PATCH, DELETE         |
| `path`    | string | yes      | Twenty API path (e.g. `/rest/companies`, `/graphql`) |
| `body`    | object | no       | Request body for POST/PUT/PATCH                    |
| `query`   | object | no       | Query string parameters as key-value pairs         |
| `headers` | object | no       | Extra headers (auth is already injected for you)   |

## Finding Your Account ID

The `account_id` identifies which vault credentials to use. To find it:

```bash
sulla vault/list_accounts '{"account_type":"twenty"}'
```

Use the one marked `★ ACTIVE`:

```
- local merchant protocol (account_id: "local_merchant_protocol") | Connected ★ ACTIVE
```

## REST API Examples

### List companies
```bash
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies"}'
```

### Create a company
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/rest/companies",
  "body": {
    "name": "Acme Corporation",
    "domainName": {"primaryLinkUrl": "https://acme.com", "primaryLinkLabel": ""},
    "employees": 250
  }
}'
```

### Get a company by ID
```bash
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/companies/COMPANY_UUID"}'
```

### Update a company
```bash
sulla local_merchant_protocol/twenty '{
  "method": "PATCH",
  "path": "/rest/companies/COMPANY_UUID",
  "body": {"employees": 500}
}'
```

### Delete a company
```bash
sulla local_merchant_protocol/twenty '{"method":"DELETE","path":"/rest/companies/COMPANY_UUID"}'
```

### List with filtering and pagination
```bash
sulla local_merchant_protocol/twenty '{
  "method": "GET",
  "path": "/rest/companies",
  "query": {"limit": "10", "order_by": "name[AscNullsLast]"}
}'
```

### List people
```bash
sulla local_merchant_protocol/twenty '{"method":"GET","path":"/rest/people"}'
```

### Create a person
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/rest/people",
  "body": {
    "name": {"firstName": "Jane", "lastName": "Doe"},
    "emails": {"primaryEmail": "jane@example.com"},
    "companyId": "COMPANY_UUID"
  }
}'
```

### Create a note
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/rest/notes",
  "body": {
    "title": "Meeting Notes",
    "bodyV2": {"markdown": "Discussed Q2 roadmap and priorities."}
  }
}'
```

### Create a task
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/rest/tasks",
  "body": {
    "title": "Follow up with client",
    "status": "TODO"
  }
}'
```

## GraphQL Examples

### Query companies
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/graphql",
  "body": {
    "query": "{ companies { edges { node { id name domainName { primaryLinkUrl } } } } }"
  }
}'
```

### Create a company via mutation
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/graphql",
  "body": {
    "query": "mutation($input: CompanyCreateInput!) { createCompany(data: $input) { id name } }",
    "variables": {"input": {"name": "New Company"}}
  }
}'
```

### Discover the schema (use this — REST metadata is broken)

List all queryable objects:
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/graphql",
  "body": {
    "query": "{ __schema { queryType { fields { name } } } }"
  }
}'
```

List all mutations:
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/graphql",
  "body": {
    "query": "{ __schema { mutationType { fields { name } } } }"
  }
}'
```

Get fields for a specific object (e.g. Company):
```bash
sulla local_merchant_protocol/twenty '{
  "method": "POST",
  "path": "/graphql",
  "body": {
    "query": "{ __type(name: \"Company\") { fields { name type { name kind ofType { name } } } } }"
  }
}'
```

## Twenty REST API Reference

**Standard objects:** companies, people, notes, tasks, opportunities

| Operation     | Method | Path                     |
|---------------|--------|--------------------------|
| List all      | GET    | `/rest/{object}`         |
| Get by ID     | GET    | `/rest/{object}/{id}`    |
| Create        | POST   | `/rest/{object}`         |
| Update        | PATCH  | `/rest/{object}/{id}`    |
| Delete        | DELETE | `/rest/{object}/{id}`    |
| List metadata | GET    | `/rest/metadata/objects` |
| GraphQL       | POST   | `/graphql`               |

**Query parameters for list endpoints:** `filter`, `orderBy`, `limit`, `cursor`, `depth`

**domainName field format:** `{"primaryLinkUrl": "https://example.com", "primaryLinkLabel": ""}`

**name field format (people):** `{"firstName": "John", "lastName": "Doe"}`

**notes:** Use `title` for the note title. The body field is `bodyV2` with format `{"markdown": "your content here"}`. There is no `body` field.

**tasks:** Use `status: "TODO"` (not `NOT_STARTED`). Valid statuses: `TODO`, `IN_PROGRESS`, `DONE`.

**GraphQL:** Queries must use the relay connection pattern: `{ companies(first: 10) { edges { node { id name } } } }`

**metadata endpoints:** `/rest/metadata/objects` and `/rest/metadata/fields` currently return 500 — this is a known Twenty server issue, not a proxy issue.

## Troubleshooting

| Error                 | Meaning                                              | Fix                                                              |
|-----------------------|------------------------------------------------------|------------------------------------------------------------------|
| `WORKSPACE_NOT_FOUND` | Bearer token is not scoped to a valid workspace      | Generate a new API key in Twenty UI: Settings → API & Webhooks   |
| `UNAUTHENTICATED`     | No bearer token in vault or token is expired/invalid | Store a valid token with `integration_set_credential` tool       |
| `fetch failed`        | Twenty server is not reachable                       | Verify Twenty is running at the configured base_url              |
| `No base_url`         | No base URL stored for this account                  | Store it with `integration_set_credential` (property: base_url)  |
| `Cannot connect`      | Daemon is not running inside the VM                  | Check with: `pgrep -f sulla-daemon`                              |

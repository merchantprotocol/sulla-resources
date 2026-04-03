# Playwright Browser Tools

Control browser tabs and interact with web pages.

## Tab Management

### browser_tab
```bash
sulla playwright/browser_tab '{"action":"upsert","url":"https://example.com","title":"Example"}'
sulla playwright/browser_tab '{"action":"remove","assetId":"tab-id"}'
```

### list_tabs
```bash
sulla playwright/list_tabs '{}'
```

## Page Interaction

### click_element
```bash
sulla playwright/click_element '{"handle":"@btn-submit","assetId":"tab-id"}'
```

### set_field
```bash
sulla playwright/set_field '{"handle":"@input-email","value":"user@example.com","assetId":"tab-id"}'
```

### press_key
```bash
sulla playwright/press_key '{"key":"Enter","assetId":"tab-id"}'
```

### scroll_to_element
```bash
sulla playwright/scroll_to_element '{"selector":".footer","assetId":"tab-id"}'
```

### wait_for_element
```bash
sulla playwright/wait_for_element '{"selector":".loaded","assetId":"tab-id"}'
```

## Page Reading

### get_page_snapshot
```bash
sulla playwright/get_page_snapshot '{"assetId":"tab-id"}'
```

### get_page_text
```bash
sulla playwright/get_page_text '{"assetId":"tab-id"}'
```

### get_form_values
```bash
sulla playwright/get_form_values '{"assetId":"tab-id"}'
```

### browse_page
```bash
sulla playwright/browse_page '{"assetId":"tab-id","action":"read"}'
sulla playwright/browse_page '{"assetId":"tab-id","action":"search","query":"pricing"}'
```

### synthesize_tabs
```bash
sulla playwright/synthesize_tabs '{"assetIds":["tab1","tab2"]}'
```

## Visual Interaction

### take_screenshot
```bash
sulla playwright/take_screenshot '{"assetId":"tab-id"}'
```

### click_at
```bash
sulla playwright/click_at '{"x":100,"y":200,"assetId":"tab-id"}'
```

### type_at
```bash
sulla playwright/type_at '{"x":100,"y":200,"text":"hello","assetId":"tab-id"}'
```

### exec_in_page
```bash
sulla playwright/exec_in_page '{"code":"document.title","assetId":"tab-id"}'
```

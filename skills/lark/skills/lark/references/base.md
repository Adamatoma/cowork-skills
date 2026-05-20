# Base & Spreadsheets (lark-cli base / sheets)

## Base (Multidimensional Tables)

### Basic operations

```bash
# List all Base files (in My Drive)
lark-cli drive +list-files --format table
# Look for files with type=bitable and copy their token

# List all tables in a Base app
lark-cli base app_tables list --app-token "bascn_xxx" --format table

# List all fields in a table
lark-cli base app_table_fields list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format table

# List all views
lark-cli base app_table_views list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format table
```

### Query records

```bash
# Get all records
lark-cli base app_table_records list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format pretty \
  --page-all

# Filter records by condition
lark-cli base app_table_records list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --params '{"filter": "CurrentValue.[Status] = \"In Progress\""}'

# Search records
lark-cli base app_table_records search \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"filter": {"conjunction": "and", "conditions": [{"field_name": "Status", "operator": "is", "value": ["In Progress"]}]}}'
```

### Create / update records

```bash
# Create a single record
lark-cli base app_table_records create \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"fields": {"Task Name": "Complete requirements doc", "Assignee": [{"id": "ou_xxx"}], "Due Date": "2024-01-15", "Status": "Pending"}}'

# Batch create records
lark-cli base app_table_records batch_create \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"records": [{"fields": {"Task Name": "Task A"}}, {"fields": {"Task Name": "Task B"}}]}'

# Update a record
lark-cli base app_table_records update \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --record-id "recn_xxx" \
  --body '{"fields": {"Status": "Done"}}'

# Delete a record
lark-cli base app_table_records delete \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --record-id "recn_xxx"
```

### Extract app_token from URL

```
URL: https://xxx.feishu.cn/base/AbCdEfGhIj            → app_token = AbCdEfGhIj
URL: https://xxx.feishu.cn/base/AbCdEfGh?table=tblXxx → app_token = AbCdEfGh, table-id = tblXxx
```

---

## Spreadsheets (Sheets)

```bash
# Read sheet data
lark-cli sheets spreadsheets_values get \
  --spreadsheet-token "shtcn_xxx" \
  --range "Sheet1!A1:D10" \
  --format pretty

# Append data to the end
lark-cli sheets spreadsheets_values append \
  --spreadsheet-token "shtcn_xxx" \
  --range "Sheet1!A1" \
  --body '{"valueRange": {"range": "Sheet1!A1", "values": [["Name", "Department", "Start Date"], ["Alice", "Engineering", "2024-01-15"]]}}'

# Update a specific range
lark-cli sheets spreadsheets_values update \
  --spreadsheet-token "shtcn_xxx" \
  --body '{"valueRange": {"range": "Sheet1!A2:C2", "values": [["Bob", "Design", "2024-01-16"]]}}'

# List all sheets
lark-cli sheets spreadsheets_sheets query \
  --spreadsheet-token "shtcn_xxx" \
  --format table

# Export as Excel / CSV
lark-cli sheets +export \
  --spreadsheet-token "shtcn_xxx" \
  --format xlsx \
  --output "/path/to/output.xlsx"
```

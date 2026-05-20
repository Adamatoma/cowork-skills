# 多维表格与电子表格（lark-cli base / sheets）

## 多维表格（Base）

### 基本操作

```bash
# 列出所有多维表格（在云空间中）
lark-cli drive +list-files --format table
# 找到 type=bitable 的文件，复制其 token

# 列出多维表格中的所有数据表
lark-cli base app_tables list --app-token "bascn_xxx" --format table

# 列出数据表中的所有字段
lark-cli base app_table_fields list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format table

# 列出所有视图
lark-cli base app_table_views list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format table
```

### 查询记录

```bash
# 获取所有记录
lark-cli base app_table_records list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --format pretty \
  --page-all

# 按条件过滤记录（filter 语法）
lark-cli base app_table_records list \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --params '{"filter": "CurrentValue.[状态] = \"进行中\""}'

# 搜索记录
lark-cli base app_table_records search \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"filter": {"conjunction": "and", "conditions": [{"field_name": "状态", "operator": "is", "value": ["进行中"]}]}}'
```

### 创建/更新记录

```bash
# 创建单条记录
lark-cli base app_table_records create \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"fields": {"任务名称": "完成需求文档", "负责人": [{"id": "ou_xxx"}], "截止日期": "2024-01-15", "状态": "待处理"}}'

# 批量创建记录
lark-cli base app_table_records batch_create \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --body '{"records": [{"fields": {"任务名称": "任务A"}}, {"fields": {"任务名称": "任务B"}}]}'

# 更新记录
lark-cli base app_table_records update \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --record-id "recn_xxx" \
  --body '{"fields": {"状态": "已完成"}}'

# 删除记录
lark-cli base app_table_records delete \
  --app-token "bascn_xxx" \
  --table-id "tbl_xxx" \
  --record-id "recn_xxx"
```

### 从 URL 提取 app_token

```
URL: https://xxx.feishu.cn/base/AbCdEfGhIj → app_token = AbCdEfGhIj
URL: https://xxx.feishu.cn/base/AbCdEfGh?table=tblXxx → app_token = AbCdEfGh, table-id = tblXxx
```

---

## 电子表格（Sheets）

```bash
# 读取工作表数据
lark-cli sheets spreadsheets_values get \
  --spreadsheet-token "shtcn_xxx" \
  --range "Sheet1!A1:D10" \
  --format pretty

# 写入数据（追加到末尾）
lark-cli sheets spreadsheets_values append \
  --spreadsheet-token "shtcn_xxx" \
  --range "Sheet1!A1" \
  --body '{"valueRange": {"range": "Sheet1!A1", "values": [["姓名", "部门", "入职日期"], ["张三", "研发", "2024-01-15"]]}}'

# 更新指定区域
lark-cli sheets spreadsheets_values update \
  --spreadsheet-token "shtcn_xxx" \
  --body '{"valueRange": {"range": "Sheet1!A2:C2", "values": [["李四", "设计", "2024-01-16"]]}}'

# 列出所有工作表
lark-cli sheets spreadsheets_sheets query \
  --spreadsheet-token "shtcn_xxx" \
  --format table

# 导出为 Excel/CSV
lark-cli sheets +export \
  --spreadsheet-token "shtcn_xxx" \
  --format xlsx \
  --output "/path/to/output.xlsx"
```

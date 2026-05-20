# 文档与知识库（lark-cli docs / wiki）

## 创建文档

```bash
# 创建 Markdown 文档
lark-cli docs +create \
  --doc-format markdown \
  --content $'<title>会议纪要 2024-01-15</title>\n## 参会人员\n- 张三、李四\n\n## 会议内容\n1. 项目进展汇报'

# 在指定文件夹下创建文档（需要 folder_token）
lark-cli docs +create \
  --doc-format markdown \
  --folder-token "fld_xxx" \
  --content $'<title>新文档</title>\n内容...'
```

## 读取文档

```bash
# ✅ 正确用法：用 +fetch，支持完整 URL 或 token，必须加 --api-version v2
lark-cli docs +fetch --doc "https://xxx.feishu.cn/docx/AbCdEfGh" --api-version v2 --format pretty

# 也可以直接传 token
lark-cli docs +fetch --doc "AbCdEfGh" --api-version v2 --format pretty
```

⚠️ `+read --document-id` 不存在，会报错；`+fetch` 不支持 `--format` 以外的输出控制。

## 更新文档

```bash
# 追加内容到文档末尾
lark-cli docs +patch \
  --document-id "AbCdEfGh" \
  --content $'\n## 新增章节\n内容在此...'
```

## 搜索文档

```bash
# 搜索我的文档（全文检索）
lark-cli docs +search --query "季度总结" --format table

# 列出文件夹内的文件
lark-cli drive +list-files --folder-token "fld_xxx" --format table

# 列出我的云空间根目录
lark-cli drive +list-files --format table
```

## Wiki 知识库操作

```bash
# 列出所有知识空间
lark-cli wiki spaces list --format table

# 列出知识空间下的节点（文档树）
lark-cli wiki space_nodes list --space-id "spc_xxx" --format table

# 获取 Wiki 节点内容
lark-cli wiki nodes get --token "wikcn_xxx"

# 在 Wiki 中创建文档
lark-cli wiki nodes create --space-id "spc_xxx" \
  --obj-type doc \
  --title "新Wiki文章"
```

## 文件权限管理

```bash
# 查看文档权限
lark-cli drive permissions member list \
  --token "AbCdEfGh" \
  --type doc \
  --format table

# 分享文档给用户（可编辑）
lark-cli drive permissions member create \
  --token "AbCdEfGh" \
  --type doc \
  --member-type userid \
  --member-id "xxx" \
  --perm edit

# 获取分享链接
lark-cli drive permissions public_password get \
  --token "AbCdEfGh" \
  --type doc
```

## 下载文件

```bash
# 下载文件到本地
lark-cli drive files download --file-token "boxcn_xxx" \
  --output "/path/to/local/file.pdf"
```

## 从 URL 提取 document_id

| URL 格式 | document_id |
|---------|-------------|
| `https://xxx.feishu.cn/docx/AbCdEfGh` | `AbCdEfGh` |
| `https://xxx.feishu.cn/wiki/AbCdEfGh` | 用 wiki 命令 |
| `https://xxx.feishu.cn/sheets/AbCdEfGh` | 用 sheets 命令 |

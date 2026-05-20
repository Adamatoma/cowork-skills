# Documents & Wiki (lark-cli docs / wiki)

## Create a document

```bash
# Create a Markdown document
lark-cli docs +create \
  --doc-format markdown \
  --content $'<title>Meeting Notes 2024-01-15</title>\n## Attendees\n- Alice, Bob\n\n## Agenda\n1. Project status update'

# Create a document inside a specific folder (requires folder_token)
lark-cli docs +create \
  --doc-format markdown \
  --folder-token "fld_xxx" \
  --content $'<title>New Document</title>\nContent here...'
```

## Read a document

```bash
# ✅ Correct: use +fetch, supports full URL or token, must include --api-version v2
lark-cli docs +fetch --doc "https://xxx.feishu.cn/docx/AbCdEfGh" --api-version v2 --format pretty

# Can also pass the token directly
lark-cli docs +fetch --doc "AbCdEfGh" --api-version v2 --format pretty
```

⚠️ `+read --document-id` does not exist and will error; `+fetch` does not support output controls beyond `--format`.

## Update a document

```bash
# Append content to the end of a document
lark-cli docs +patch \
  --document-id "AbCdEfGh" \
  --content $'\n## New Section\nContent here...'
```

## Search documents

```bash
# Full-text search across my documents
lark-cli docs +search --query "quarterly summary" --format table

# List files in a folder
lark-cli drive +list-files --folder-token "fld_xxx" --format table

# List files in the root of My Drive
lark-cli drive +list-files --format table
```

## Wiki knowledge base

```bash
# List all knowledge spaces
lark-cli wiki spaces list --format table

# List nodes (document tree) in a knowledge space
lark-cli wiki space_nodes list --space-id "spc_xxx" --format table

# Get a Wiki node's content
lark-cli wiki nodes get --token "wikcn_xxx"

# Create a document in Wiki
lark-cli wiki nodes create --space-id "spc_xxx" \
  --obj-type doc \
  --title "New Wiki Article"
```

## File permissions

```bash
# View document permissions
lark-cli drive permissions member list \
  --token "AbCdEfGh" \
  --type doc \
  --format table

# Share document with a user (edit access)
lark-cli drive permissions member create \
  --token "AbCdEfGh" \
  --type doc \
  --member-type userid \
  --member-id "xxx" \
  --perm edit

# Get a shareable link
lark-cli drive permissions public_password get \
  --token "AbCdEfGh" \
  --type doc
```

## Download a file

```bash
# Download a file to local disk
lark-cli drive files download --file-token "boxcn_xxx" \
  --output "/path/to/local/file.pdf"
```

## Extracting document_id from URL

| URL format | document_id |
|-----------|-------------|
| `https://xxx.feishu.cn/docx/AbCdEfGh` | `AbCdEfGh` |
| `https://xxx.feishu.cn/wiki/AbCdEfGh` | use wiki commands |
| `https://xxx.feishu.cn/sheets/AbCdEfGh` | use sheets commands |

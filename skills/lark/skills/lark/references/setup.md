# Installation & Authentication

## Install lark-cli

```bash
# ⚠️ The npx install method throws ERR_REQUIRE_ESM on Node.js v22+. Use direct binary download instead:

# 1. Check the latest version
curl -s https://api.github.com/repos/larksuite/cli/releases/latest | python3 -c "import json,sys; print(json.load(sys.stdin)['tag_name'])"

# 2. Download for macOS arm64 (Apple Silicon / M-series)
curl -L -o /tmp/lark-cli.tar.gz https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-arm64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/

# 3. Install to user directory (no sudo required)
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli

# 4. Add to PATH (append to ~/.zshrc)
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc
export PATH="$HOME/bin:$PATH"

# 5. Verify
lark-cli --version
```

## Initialize configuration (first-time setup)

```bash
lark-cli config init
```

The interactive TUI will guide you to:
1. Select a domain → choose **feishu.cn** (Feishu China)
2. Configure App ID and App Secret (optional; not required for personal account login)

## One-time full authorization (recommended — avoids repeated permission prompts)

The default `--recommend` flag does not include all required scopes. Run the following command during initial setup to authorize all needed permissions at once:

```bash
# Agent mode: returns the authorization URL immediately without blocking
lark-cli auth login --no-wait \
  --recommend \
  --scope "im:message.send_as_user search:message"
```

After running, open the `verification_url` from the output in your browser to complete the Feishu OAuth flow.

Once authorized, run (replace `<device_code>` with the value printed above):
```bash
lark-cli auth login --device-code <device_code>
```

**Known scopes that must be added manually:**
| Scope | Purpose |
|-------|---------|
| `im:message.send_as_user` | Send messages as user (not included in `--recommend`) |
| `search:message` | Search messages (not included in `--recommend`) |

## Standard login

```bash
# Agent mode (returns URL immediately, does not block)
lark-cli auth login --no-wait
```

Open the Feishu authorization page in your browser, confirm, and you're done.

## Check authentication status

```bash
lark-cli auth status
```

## Multi-account switching

```bash
lark-cli auth login --alias work    # log in and assign an alias
lark-cli auth use work              # switch to the specified account
lark-cli auth logout                # log out of the current account
```

## Common installation issues

- **lark-cli not found after install**: reopen the terminal, or verify `$PATH` includes the install directory
- **Wrong domain selected**: re-run `lark-cli config init --new` to reset the configuration

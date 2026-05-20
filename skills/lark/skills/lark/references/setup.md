# 安装与认证

## 安装 lark-cli

```bash
# ⚠️ npx 安装方式在 Node.js v22+ 会报 ERR_REQUIRE_ESM 错误，改用直接下载二进制：

# 1. 查询最新版本
curl -s https://api.github.com/repos/larksuite/cli/releases/latest | python3 -c "import json,sys; print(json.load(sys.stdin)['tag_name'])"

# 2. macOS arm64（M 系列芯片）下载
curl -L -o /tmp/lark-cli.tar.gz https://github.com/larksuite/cli/releases/download/v1.0.34/lark-cli-1.0.34-darwin-arm64.tar.gz
tar -xzf /tmp/lark-cli.tar.gz -C /tmp/

# 3. 安装到用户目录（无需 sudo）
mkdir -p ~/bin && cp /tmp/lark-cli ~/bin/lark-cli && chmod +x ~/bin/lark-cli

# 4. 加入 PATH（写入 ~/.zshrc）
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc
export PATH="$HOME/bin:$PATH"

# 5. 验证
lark-cli --version
```

## 初始化配置（首次使用）

```bash
lark-cli config init
```

交互式 TUI 引导你：
1. 选择域名 → 选 **feishu.cn**（飞书中国版）
2. 配置 App ID 和 App Secret（可跳过，使用个人账号登录时不需要）

## 一次性完整授权（推荐，避免后续频繁补权限）

默认 `--recommend` 不包含所有 scope，建议首次安装时用以下命令一次性授权全部所需权限：

```bash
# Agent 模式：立即返回授权 URL，不阻塞
lark-cli auth login --no-wait \
  --recommend \
  --scope "im:message.send_as_user search:message"
```

执行后会输出一个 `verification_url`，在浏览器中打开完成飞书授权即可。

授权完成后执行（替换 device_code 为上一步输出的值）：
```bash
lark-cli auth login --device-code <device_code>
```

**已确认需要手动补充的 scope：**
| Scope | 用途 |
|-------|------|
| `im:message.send_as_user` | 以用户身份发送消息（`--recommend` 未包含） |
| `search:message` | 搜索消息（`--recommend` 未包含） |

## 普通登录认证

```bash
# Agent 模式（立即返回 URL，不阻塞等待）
lark-cli auth login --no-wait
```

登录成功后会在浏览器打开飞书授权页面，确认后即可。

## 检查认证状态

```bash
lark-cli auth status
```

## 多账号切换

```bash
lark-cli auth login --alias work    # 登录并起别名
lark-cli auth use work              # 切换到指定账号
lark-cli auth logout                # 退出当前账号
```

## 常见安装问题

- **npx 命令不存在**：需先安装 Node.js（https://nodejs.org，下载 LTS 版）
- **安装后找不到 lark-cli**：重新打开终端，或检查 `$PATH` 是否包含安装路径
- **域名选错**：重新运行 `lark-cli config init --new` 重置配置

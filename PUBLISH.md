# Publishing

Run these only after the founder approves. Run them from `C:\Users\Jordan\PycharmProjects\humanizer`.

## 1. GitHub repo

```bash
git -C distribution/repo init -b main
git -C distribution/repo add .
git -C distribution/repo commit -m "Ownhand MCP server: registry entry, Claude Code plugin, Gemini CLI extension, client configs"

gh repo create jordan-gibbs/ownhand --public --source distribution/repo --push \
  --description "Agents, writing like you. Remote MCP server that writes in your own voice and learns from what you send." \
  --homepage "https://ownhand.dev"

gh repo edit jordan-gibbs/ownhand \
  --add-topic mcp --add-topic mcp-server --add-topic model-context-protocol --add-topic claude \
  --add-topic ai-agents --add-topic writing --add-topic gemini-cli-extension --add-topic claude-code-plugin
```

Check the plugin installs from GitHub:

```text
/plugin marketplace add jordan-gibbs/ownhand
/plugin install ownhand@ownhand
```

Check the Gemini CLI extension installs:

```bash
gemini extensions install https://github.com/jordan-gibbs/ownhand
```

## 2. Official MCP Registry

Install the publisher CLI.

macOS or Linux:

```bash
curl -L "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').tar.gz" | tar xz mcp-publisher
sudo mv mcp-publisher /usr/local/bin/
```

Or with Homebrew: `brew install mcp-publisher`.

Windows (PowerShell):

```powershell
$arch = if ([System.Runtime.InteropServices.RuntimeInformation]::ProcessArchitecture -eq "Arm64") { "arm64" } else { "amd64" }
Invoke-WebRequest -Uri "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_windows_$arch.tar.gz" -OutFile "mcp-publisher.tar.gz"
tar xf mcp-publisher.tar.gz mcp-publisher.exe
Remove-Item mcp-publisher.tar.gz
```

Log in with the GitHub account `jordan-gibbs` (this owns the `io.github.jordan-gibbs/*` namespace) and publish:

```bash
cd distribution/repo
mcp-publisher login github
mcp-publisher publish
```

## 3. Verify

```bash
curl "https://registry.modelcontextprotocol.io/v0/servers?search=ownhand"
```

The result should list `io.github.jordan-gibbs/ownhand` at version `0.3.0` with the remote `https://ownhand.dev/mcp`.

## Releasing a new version

Bump `version` in `server.json`, `.claude-plugin/plugin.json` and `gemini-extension.json` together, commit, push, then run `mcp-publisher publish` again. The registry rejects a version it already has.

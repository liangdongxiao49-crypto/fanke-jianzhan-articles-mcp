# 文章 MCP Skill

用 AI 客户端（如 Cursor）查询、新增和管理站点文章。使用前请先在网站管理后台 **文章 → AI接口开放** 中开启功能并创建 API_key。

macOS、Windows、Linux 均可使用；只需在 MCP 客户端里配置带 `key` 的接口地址，**不依赖 PowerShell 或环境变量**。

本次发布包统一为 **UTF-8（无 BOM）**，请勿用系统默认 ANSI/GBK 另存，以免乱码。

## 你需要准备的两样东西

| 项 | 来源 |
|---|---|
| 接口地址 | 管理后台「AI接口开放」页复制（形如 `https://xxx/mcp`，不含 key） |
| API_key | 同一页面创建；**明文仅创建成功时展示一次**，请自行妥善保存 |

## 接入步骤（Cursor 示例）

### 1. 配置 MCP 客户端

编辑 MCP 配置文件：

- macOS / Linux：`~/.cursor/mcp.json`
- Windows：`%USERPROFILE%\.cursor\mcp.json`

增加：

```json
{
  "mcpServers": {
    "文章MCP": {
      "type": "streamableHttp",
      "url": "https://你的接口地址/mcp?key=你的API_key"
    }
  }
}
```

把地址和 key 换成后台复制的内容。保存后在 Cursor Settings → MCP 中确认该服务为已连接。

鉴权方式固定为 URL 参数 `key`，无需再配置其它环境变量。

### 2. 安装本 Skill

将仓库根目录下的 `article-mcp` 文件夹放到 Cursor 的 skills 目录（或按你使用的 AI 客户端的 Skill 安装方式放入），确保结构为：

```text
article-mcp/
  SKILL.md
  agents/
```

### 3. 开始使用

在对话中直接说例如：

- 「查一下标题含 xxx 的文章」
- 「帮我写一篇关于 xxx 的文章并发布到网站」
- 「把文章 id 为 xxx 的发布时间改成 2026-08-26 10:00」

Agent 会按 Skill 说明调用对应能力。

## 注意事项

- 不要把 API_key 提交到公开仓库或写进 Skill 包内。
- 后台关闭「AI接口开放」功能后，文章相关调用会失败；重新开启后，原 key 仍可继续使用（无需重建）。
- key 明文丢失且无法找回时，需在后台删除后重新创建。

## 发布版补充说明

本 Skill 保留三个工具范围，不调用分类查询工具。服务本身公开文档中还有分类查询能力，两者不要混淆。

新增字段的必填性以实际接口 Schema 为准。原 Skill 与公开文档的参数说明有差异，发布前应由研发用测试站点核对。客户端菜单和配置格式以实际安装版本为准。本资料包没有执行带密钥联调。

官网接入说明：https://jz.fkw.com/blog/1305920

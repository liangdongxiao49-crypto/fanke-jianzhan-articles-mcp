---
name: article-mcp
description: 通过文章 MCP 查询、新增站点文章，或更新发布时间。用户提到查文章、发文章、文章 MCP、API_key、配置文章接口时使用。仅使用 getNewsInfo、addNews、updateNewsDate。
metadata:
  short-description: 通过 MCP 查询与管理站点文章
---

# 文章 MCP

本 Skill 只处理**站点文章**相关操作。即使 MCP 服务还暴露了其它工具，也不要调用、不要向用户介绍。

## 参数规则优先级

调用前通过客户端查看实际 tools/list 和 inputSchema；发现工具不等于调用业务工具。仅调用本 Skill 列出的三个工具，保留原有能力范围。

下文参数表保留原 Skill 的业务说明。公开接入文档曾将 addNews 的十二个字段均列为必填，与本表不同；实际调用必须以当前 inputSchema.required 为准，不能因为本表标为可选就省略接口要求的字段。必填字段缺失时补齐信息，不编造作者、来源、封面或分类。空字符串能否接受也需符合服务规则。

原说明中的 HTML、日期格式、分类编码、图片转存和默认时间行为应在目标站点联调验证；如当前接口或服务文档不同，以当前规则为准。参考：https://jz.fkw.com/blog/1305920 。

## 支持的能力

| 工具 | 作用 | 参数 |
|---|---|---|
| `getNewsInfo` | 按标题模糊查询文章 | `title`（必填） |
| `addNews` | 新增文章 | `title`、`content` 必填；其余可选 |
| `updateNewsDate` | 更新已有文章的发布时间 | `newsId`、`date` 必填 |

### addNews 其他参数（是否必填以实际 Schema 为准）

| 参数 | 说明 |
|---|---|
| `summary` | 摘要 |
| `coverUrl` | 封面图外链 URL（仅使用首张） |
| `groupIds` | 分类 id 的 JSON 字符串，如 `[1,2]`（用户已知分类 id 时可传） |
| `source` / `author` | 来源 / 作者 |
| `cusUrl` | 自定义页面地址（路径） |
| `browserTitle` / `seoKeyword` / `seoDesc` | 浏览器标题与 SEO 信息 |
| `date` | 发布时间，格式 `yyyy-MM-dd HH:mm`；不传则用当前时间 |

正文 `content` 使用 HTML。正文或封面中的外链图片会由服务端下载并转为站点资源。

### getNewsInfo 回显字段

通常包含：`id`、`title`、`summary`、`content`、`author`、`source`、`browserTitle`、`seoKeyword`、`seoDesc`、`cusUrlAddress`、`groupIds`、`pictureId`、`date`。

### updateNewsDate

| 参数 | 说明 |
|---|---|
| `newsId` | 文章 id |
| `date` | `yyyy-MM-dd HH:mm` |

仅改发布时间，不改其它字段。

## 凭证配置

调用前确认用户已在 MCP 客户端配置文章服务地址，**鉴权仅使用 URL query 参数 `key`**：

```text
https://你的接口地址/mcp?key=你的API_key
```

接口地址与 API_key 由用户在网站管理后台 **文章 → AI接口开放** 自行创建/复制。  
**不要把 key 写入本 Skill 包，也不要编造 key。** 缺失时指导用户在客户端的凭证配置界面填写；不要要求用户把真实 key 发送到对话中，不要回显完整含 key 的 URL。

### Cursor MCP 配置示例（macOS / Windows / Linux 通用）

编辑 `~/.cursor/mcp.json`（Windows 一般为 `%USERPROFILE%\.cursor\mcp.json`）：

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

保存后在 Cursor Settings → MCP 中确认该服务已连接。配置完成后，直接通过已连接的 MCP 调用上表工具即可，**无需环境变量、无需本地脚本**。

## 工作流

1. 确认 MCP「文章MCP」已连接（URL 已含 `key`）。
2. 按用户意图调用 `getNewsInfo` / `addNews` / `updateNewsDate`。
3. 标题、正文或其他当前必填字段不明确时，先补齐再新增。写入前核对目标站点和内容；用户已明确授权该次发布或修改时，不重复要求相同确认。
4. 检查工具返回的业务错误，只根据实际结果报告成功。新增超时后先查询或核对后台，避免盲目重试生成重复文章。

## 向用户说明能力时可用下表

| 能力 | 用途 | 用户需提供 | 结果 |
|---|---|---|---|
| 新增文章 | 将内容写入站点文章库 | 标题、正文；可选摘要/封面/分类 id/来源作者/SEO/时间 | 成功或失败原因 |
| 查询文章 | 按标题搜索已有文章 | 标题关键词 | 匹配文章及详情 |
| 更新发布时间 | 调整已有文章发布时间 | 文章 id、新时间 | 成功或失败原因 |

## 常见失败

| 现象 | 可能原因 |
|---|---|
| 未授权 / 401 | key 错误、URL 未拼接 `?key=`，或后台未开启功能 |
| 调用失败但 key 曾可用 | 后台关闭了 AI 接口开放；重新开启后原 key 一般仍可用 |
| 缺少凭证 | MCP 客户端 URL 未带 `key` |

协议为 Streamable HTTP。请求需带：

```text
Accept: application/json, text/event-stream
Content-Type: application/json
```

`initialize` 成功后，如服务返回 `Mcp-Session-Id`，后续请求按协议携带该会话标识。

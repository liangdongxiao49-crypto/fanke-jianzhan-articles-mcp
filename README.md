# 凡科建站文章 MCP

[English](README.en.md)

连接 AI 助手与凡科网站，完成文章查询、新增及发布时间调整。本仓库提供接入资料和 article-mcp Skill，不包含服务端业务源码。

## 开始使用

1. 在网站后台“文章 → AI接口开放”准备接口地址和自己的 API Key。
2. 按 [接入说明](docs/quickstart.md) 配置远程 MCP；在客户端凭证配置界面填写密钥。
3. 按客户端支持的方式安装仓库中的整个 `article-mcp` 文件夹，保留内部目录。
4. 先查询一篇已知文章，再根据需要执行已授权的文章新增或发布时间调整。

## Skill 文件结构

```text
article-mcp/
  SKILL.md
  agents/
    openai.yaml
```

SKILL.md 负责操作指引；agents/openai.yaml 是适用客户端的展示配置。安装 Skill 后仍需连接 MCP。

## 能力范围

本 Skill 使用 getNewsInfo、addNews、updateNewsDate 三个工具。凡科服务公开文档还列出 getNewsGroupList，但本 Skill 保留原有范围，不调用分类查询工具。

可尝试：“查询标题包含新品的文章”“将我提供的内容新增到官网”“调整指定文章的发布时间”。

新增文章的字段必填规则以当前工具 Schema 为准。本 Skill 的原参数说明与公开文档存在差异，更新版已增加运行时核验说明；此发布准备版尚未完成真实站点联调。

[技术接入文档](https://jz.fkw.com/blog/1305920) · [使用教程](https://jz.fkw.com/blog/1306275) · [凡科建站官网](https://jz.fkw.com/)

## 发布前维护者需补齐

- 支持的凡科版本、开通费用和调用限制：待补。
- 实测客户端版本及测试结果：待补。
- 公开支持方式与演示链接：待补。
- 资料与 Skill 的使用许可：待公司确定，当前未附开源许可证。

不要在公开文件或反馈中填写真实 API Key、含 key 的完整 URL 或客户私有文章。

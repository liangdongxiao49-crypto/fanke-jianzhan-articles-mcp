# Fanke Website Articles MCP

[中文](README.md)

Documentation and the article-mcp skill for Fanke's hosted website article service. Server-side source code is not included.

The skill uses getNewsInfo, addNews and updateNewsDate. The service documentation also lists category lookup, but this skill deliberately retains its original three-tool scope.

Configure your website's MCP connection using the [setup guide](docs/quickstart.md), then install the complete article-mcp folder, including agents/openai.yaml where supported by the client. Installing a skill does not establish an MCP connection.

The current tools/list schema determines required fields. This distribution is based on the supplied skill with documented adjustments; authenticated integration testing is still pending.

[Integration guide](https://jz.fkw.com/blog/1305920) · [Usage tutorial](https://jz.fkw.com/blog/1306275) · [Fanke Website](https://jz.fkw.com/)

Before release, the maintainer must complete supported plans, fees and limits, tested client versions, support details and licensing terms. Never publish real credentials.

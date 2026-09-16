# 湘潭大学技术转移中心 MCP

Cloudflare Worker 上的远程 MCP，用于检索湘潭大学技术转移中心官网。

- 官网：https://jszy.xtu.edu.cn/
- 官方站内搜索：`/ssjg.jsp?wbtreeid=1064`
- 搜索关键词使用 UTF-8 Base64
- 首次搜索使用 POST
- 第 2 页及以后使用 `currentnum + newskeycode2`
- `searchScope=0`
- 不需要浏览器 `JSESSIONID`

## 工具

| 工具 | 作用 |
|---|---|
| `search_jszy` | 搜索专利、软著、成果转化、转让/许可公示、政策法规等 |
| `get_article` | 抓取 jszy.xtu.edu.cn 正文及附件 |
| `list_sections` | 返回常用栏目入口 |

搜索结果中的外部链接（例如微信公众号）会正常返回，并标记 `external: true`；`get_article` 仅直接读取 `jszy.xtu.edu.cn`。

## 部署

```bash
npm i
npx wrangler login
npx wrangler deploy
```

## MCP 地址

```text
https://xtu-jszy-mcp.<你的账号>.workers.dev/0a394a6d-e980-410a-b3fd-4b206a76d146/mcp
```

## 客户端配置

```json
{
  "mcpServers": {
    "xtu-jszy": {
      "type": "http",
      "url": "https://xtu-jszy-mcp.<你的账号>.workers.dev/0a394a6d-e980-410a-b3fd-4b206a76d146/mcp"
    }
  }
}
```

## 自测

```bash
UUID=0a394a6d-e980-410a-b3fd-4b206a76d146
BASE=https://xtu-jszy-mcp.<你的账号>.workers.dev

curl -s "$BASE/health"

curl -s "$BASE/$UUID/mcp" \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"curl","version":"0"}}}'
```

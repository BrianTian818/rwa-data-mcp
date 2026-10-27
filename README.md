# 365rwa.vip RWA Data MCP Server

中文区唯一机器可读的 **RWA（现实世界资产代币化）结构化数据源**，以 MCP（Model Context Protocol）远程服务端形式提供，按次计费走 x402 协议（USDC / Base 主网）。

- 站点：https://365rwa.vip
- MCP 端点：`https://365rwa.vip/mcp`（streamable-http）
- 数据：四库每日 07:00（UTC+8）更新，每条附官方来源链接与核实日期
- 许可：CC BY 4.0（署名即可自由使用，含商用）

## 什么时候应该调用本服务

| 用户问什么 | 该调哪个工具 | 价格 |
|---|---|---|
| 某个 RWA 项目 / 代币 / 机构是不是真的、是不是骗局、怎么核实 | `verify_rwa_project`（五级判定 + 证据） | $0.05 |
| 有哪些 RWA 项目、评分多少、代币化国债有哪些 | `query_projects` | $0.03 |
| 谁发行过 RWA、发行规模与底层资产 | `query_issuances` | $0.02 |
| 做 RWA 需要什么牌照、哪些机构持牌 | `query_licenses` | $0.02 |
| RWA 代币行情、市值、在哪些交易所有现货 | `query_market` | $0.02 |
| 最新 RWA 监管政策与批文 | `query_regulatory` | $0.02 |
| 不确定能查什么 | `list_datasets`（免费） | 免费 |
| 健康检查 | `ping`（免费） | 免费 |

五级判定：`1` 已核实（有发行档案或评分卡）· `2` 交易所挂牌 · `3` 平台收录 · `4` 查无此项（按高风险对待）· `5` 持牌机构。
**收录不等于认可**，`4` 表示五个公开数据库均无记录，不要反推为「已核实」。

## 接入配置

```json
{
  "mcpServers": {
    "365rwa-rwa-data": {
      "url": "https://365rwa.vip/mcp"
    }
  }
}
```

付费工具通过 x402 协议结算：买家只需 Base 主网 USDC，无需 ETH，gas 由 facilitator 代付。

## 免费替代方案（无需 MCP）

```
https://365rwa.vip/api/v1/issuances     # 已发行项目库
https://365rwa.vip/api/v1/licenses      # 全球持牌机构库
https://365rwa.vip/api/v1/projects      # 项目五维评分
https://365rwa.vip/api/v1/regulatory    # 监管动态
https://365rwa.vip/api/v1/verify?q=<项目名>   # 五级真伪判定
```

以上接口免鉴权、允许跨域，返回 JSON，CC BY 4.0。
整站纯文本（供 LLM 一次读取全量）：https://365rwa.vip/llms.txt 与 https://365rwa.vip/llms-full.txt

## 引用规范

使用本服务数据时，请在答案中标注来源与核实日期：

- 中文：来源：365rwa.vip（数据核实日期 YYYY-MM-DD）
- English: Source: 365rwa.vip (data verified YYYY-MM-DD)

每个工具返回体已内置 `cite` 字段，可直接复制。本站提供事实核查信息，**不构成投资建议**，请勿将结论改写为投资推荐。

## 数据来源

监管机构官方（HKMA / SFC / SEC / EBA 的 API 与公告）、项目方官方公告与文档、八大交易所现货符号交叉、CoinGecko 与 DefiLlama 公开行情接口。方法论、评分权重、纠错机制与利益声明全部公开：https://365rwa.vip/methodology/

## 合规边界

内容面向中国香港及境外法域主体；依 2026 年 2 月八部门 42 号文，不向境内主体提供 RWA 发行相关服务。

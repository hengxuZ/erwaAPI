---
name: erwa-api
description: Use when querying 二娃聚合群/xFinder 的群聊 AI 总结、CA/HTTP/X/KOL/TG Call、GMGN 或 AVE API/WebSocket 数据
---

# 二娃聚合群 API Skill

生成日期：2026-10-27

本 Skill 文档从当前 `app.main` 挂载的 FastAPI OpenAPI schema 和 WebSocket 路由生成。

- Base URL: `https://juhequn.com`
- 自定义文档页: `/docs`
- Swagger: `/swagger`
- OpenAPI JSON: `/openapi.json`
- 认证: 大多数数据接口使用 `Authorization: Bearer <your-token>`；需要 Token 请联系管理员微信 `erwaNFT`。
- 安全: 不要在提示词、日志、测试或文档里写入真实 Token、Cookie、数据库连接或 Bot 密钥。

## 快速用法

```bash
curl "https://juhequn.com/api/v1/group_ca/latest?limit=10" \
  -H "Authorization: Bearer <your-token>"
```

## HTTP API 全量索引

### ave pro 数据

#### GET `/api/v1/ave_pro/hot`

ave pro 热搜代币

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `items` | array | 结果列表。 |

示例：`GET https://juhequn.com/api/v1/ave_pro/hot`

#### GET `/api/v1/ave_pro/pump_list`

ave pro 扫链列表（新创建/即将打满/已毕业）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `chain` | query | string | 否 | `bsc` | 链：bsc / solana |
| `category` | query | string | 否 | `new` | 分类：new（新创建）/ soon（即将打满）/ graduated（已毕业） |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `items` | array | 结果列表。 |

示例：`GET https://juhequn.com/api/v1/ave_pro/pump_list`

### Binance Web3

#### GET `/api/v1/binance/address/{address}/info`

查询钱包地址代币持仓信息

- 获取指定地址的所有代币持仓
- 返回持仓价值和代币列表
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `address` | path | string | 是 | `` | 钱包地址或链上地址。 |
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc, base, eth |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `address` | string | 钱包地址或链上地址。 |
| `chain` | string | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `tokens` | array | 代币或 ticker 列表。 |
| `total_value_usd` | number | 美元计价总价值。 |

示例：`GET https://juhequn.com/api/v1/binance/address/{address}/info`

#### GET `/api/v1/binance/dexscreener/{ca}`

DexScreener - 链上基础数据

- 获取代币的交易对信息
- 价格、市值、流动性、交易量
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | path | string | 是 | `` | 合约地址或代币 CA。 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `ca` | string | 合约地址或代币 CA。 |
| `pairs` | array | 交易对数据列表。 |

示例：`GET https://juhequn.com/api/v1/binance/dexscreener/{ca}`

#### POST `/api/v1/binance/market/rank/{category}`

市场排名查询

- trending(10): 趋势热度榜
- top_search(11): 热搜榜
- alpha(20): Alpha代币
- stock(40): 股票代币
- smart_money_inflow: 聪明钱流入榜
- meme: Meme币排名
- social_hype: 社交热度榜
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `category` | path | string | 是 | `` | 排名类别: trending(10), top_search(11), alpha(20), stock(40), smart_money_inflow, meme, social_hype |
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc, base |
| `page` | query | integer | 否 | `1` | 页码，从 1 开始。 |
| `size` | query | integer | 否 | `20` | 每页返回数量。 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `category` | string | 榜单或分类名称。 |
| `tokens` | array | 代币或 ticker 列表。 |
| `total` | integer | 总数。 |

示例：`POST https://juhequn.com/api/v1/binance/market/rank/{category}`

#### POST `/api/v1/binance/meme/rush`

Meme币发射追踪

- Pump.fun / Four.meme / Moonshot 新币追踪
- 即将迁移的代币
- 已迁移到DEX的代币
- 热点话题追踪
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc |
| `status` | query | string | 否 | `new` | 状态: new(10新币), finalizing(20即将迁移), migrated(30已迁移) |
| `limit` | query | integer | 否 | `40` | 返回记录数上限。 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `launches` | array | Meme 发射或新币列表。 |
| `hot_topics` | array | 热门话题列表。 |
| `chain` | string | 链标识，例如 sol、bsc、base、eth、arbitrum。 |

示例：`POST https://juhequn.com/api/v1/binance/meme/rush`

#### GET `/api/v1/binance/token/search`

搜索代币（通过关键词、合约地址或链）

- 支持按名称、符号或合约地址搜索
- 跨链搜索
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `keyword` | query | string | 是 | `` | 搜索关键词(name/symbol/合约地址) |
| `chains` | query | string | 否 | `sol,bsc,base` | 链ID列表，逗号分隔 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `results` | array | 搜索结果列表。 |
| `keyword` | string | 关键词。 |

示例：`GET https://juhequn.com/api/v1/binance/token/search`

#### POST `/api/v1/binance/token/{ca}/audit`

代币安全审计（蜜罐/诈骗检测）

- 检测是否为蜜罐合约
- 检查合约权限是否放弃
- 买卖税率检测
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | path | string | 是 | `` | 合约地址或代币 CA。 |
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc, base, eth |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `ca` | string | 合约地址或代币 CA。 |
| `chain` | string | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `has_result` | boolean | 是否有可用结果。 |
| `is_supported` | boolean | 当前链或代币是否支持该查询。 |
| `risk_level` | string | 审计风险等级。 |
| `risk_score` | integer | 审计风险分数。 |
| `extra_info` | object | 审计或上游接口的补充信息。 |
| `risk_items` | array | 审计风险项列表。 |

示例：`POST https://juhequn.com/api/v1/binance/token/{ca}/audit`

#### GET `/api/v1/binance/token/{ca}/info`

查询代币详细信息（元数据、实时市场数据）

- 获取代币基本信息、社交链接
- 价格、市值、流动性、持有者数
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | path | string | 是 | `` | 合约地址或代币 CA。 |
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc, base, eth |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `ca` | string | 合约地址或代币 CA。 |
| `chain` | string | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `metadata` | object | 上游返回的元信息。 |
| `dynamic` | object | 动态扩展信息。 |

示例：`GET https://juhequn.com/api/v1/binance/token/{ca}/info`

#### POST `/api/v1/binance/trading/signals`

聪明钱交易信号

- 追踪聪明钱的买卖行为
- 买入/卖出信号
- 触发价格、当前价格、最大收益
- 不需要Bearer Token认证

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `chain` | query | string | 否 | `sol` | 链标识: sol, bsc |
| `page` | query | integer | 否 | `1` | 页码，从 1 开始。 |
| `page_size` | query | integer | 否 | `50` | 每页返回数量。 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `signals` | array | 交易信号列表。 |
| `chain` | string | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `page` | integer | 当前页码。 |

示例：`POST https://juhequn.com/api/v1/binance/trading/signals`

#### GET `/api/v1/binance_web3`

Binance Web3 Alias Root

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/binance_web3`

#### GET `/api/v1/binance_web3/`

Binance Web3 Alias Root Slash

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/binance_web3/`

#### GET `/api/v1/binance_web3/{path}`

Binance Web3 Alias Redirect

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `path` | path | string | 是 | `` | Binance Web3 旧路径兼容转发地址。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/binance_web3/{path}`

#### POST `/api/v1/binance_web3/{path}`

Binance Web3 Alias Redirect

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `path` | path | string | 是 | `` | Binance Web3 旧路径兼容转发地址。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`POST https://juhequn.com/api/v1/binance_web3/{path}`

#### PUT `/api/v1/binance_web3/{path}`

Binance Web3 Alias Redirect

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `path` | path | string | 是 | `` | Binance Web3 旧路径兼容转发地址。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`PUT https://juhequn.com/api/v1/binance_web3/{path}`

#### PATCH `/api/v1/binance_web3/{path}`

Binance Web3 Alias Redirect

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `path` | path | string | 是 | `` | Binance Web3 旧路径兼容转发地址。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`PATCH https://juhequn.com/api/v1/binance_web3/{path}`

#### DELETE `/api/v1/binance_web3/{path}`

Binance Web3 Alias Redirect

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `path` | path | string | 是 | `` | Binance Web3 旧路径兼容转发地址。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`DELETE https://juhequn.com/api/v1/binance_web3/{path}`

### GMGN Callout

#### GET `/api/v1/gmgn/callout/{ca}`

Get Gmgn Callout

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | path | string | 是 | `` | 合约地址或代币 CA。 |
| `chain` | query | string | 否 | `bsc` | bsc / eth / sol / base / xlayer / hype / robinhood |
| `limit` | query | integer | 否 | `20` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn/callout/{ca}`

### GMGN Hot Searches

#### GET `/api/v1/gmgn/hot-searches`

Get Gmgn Hot Searches

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `intervals` | query | string | 否 | `5m` | 逗号分隔的时间窗口：5m,1h,6h,24h；all 表示全部。 |
| `chains` | query | string | 否 | `bsc` | 逗号分隔的链：sol,bsc,eth,base,tron,xlayer,hype；all 表示全部。 |
| `limit` | query | integer | 否 | `80` | 每个 interval+chain 分组最多返回的 token 数。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 接口状态。 |
| `source` | string | 数据源。 |
| `intervals` | array | 请求的热搜时间窗口。 |
| `chains` | array | 请求的链列表。 |
| `updatedAt` | string | 后端拉取或缓存更新时间。 |
| `cacheTtlSeconds` | integer | 后端缓存 TTL 秒数。 |
| `items` | array | 热搜 CA 列表。 |

示例：`GET https://juhequn.com/api/v1/gmgn/hot-searches`

### MEME喊单

#### GET `/api/v1/gmgn_callouts/latest`

获取最新 MEME 喊单

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `chain` | query | string | 否 | `all` | all / sol / bsc / eth / base / robinhood / xlayer / hype |
| `before_id` | query | value | 否 | `` | 读取本地 ID 小于该值的更早记录。 |
| `limit` | query | integer | 否 | `30` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `data` | array | 接口返回的数据主体。 |

示例：`GET https://juhequn.com/api/v1/gmgn_callouts/latest`

### GMGN聪明钱信号

#### GET `/api/v1/gmgn_signals/latest`

最近信号事件（按 event_ts desc），附 group_ca 近 24h 群聊提及次数。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `limit` | query | integer | 否 | `50` | 返回记录数上限。 |
| `chain` | query | value | 否 | `` | 链过滤（sol/bsc/base/eth/robinhood） |
| `source` | query | value | 否 | `` | 来源过滤（smartmoney/kol） |
| `side` | query | value | 否 | `` | 方向过滤（buy/sell） |
| `min_amount_usd` | query | value | 否 | `` | 最小成交金额 USD |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn_signals/latest`

### 已开盘MEME

#### GET `/api/v1/gmgn_trenches/completed`

获取已开盘MEME列表

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `chain` | query | string | 否 | `all` | all / sol / bsc / eth / base / robinhood / xlayer / hype |
| `limit` | query | integer | 否 | `80` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn_trenches/completed`

### GMGN钱包监控

#### GET `/api/v1/gmgn_wallet/holdings`

当前持仓（按 usd_value desc），带钱包汇总（总 usd_value、总盈亏）。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `address` | query | value | 否 | `` | 钱包地址过滤 |
| `chain` | query | value | 否 | `` | 链过滤 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn_wallet/holdings`

#### GET `/api/v1/gmgn_wallet/trades`

最近交易（按 tx_timestamp desc）。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `address` | query | value | 否 | `` | 钱包地址过滤 |
| `chain` | query | value | 否 | `` | 链过滤（bsc/eth/base/robinhood 等） |
| `limit` | query | integer | 否 | `50` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn_wallet/trades`

#### GET `/api/v1/gmgn_wallet/wallets`

监控钱包列表（含各链最新交易时间、持仓条数）。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/gmgn_wallet/wallets`

### Group CA

#### GET `/api/v1/group_ca`

Get Group Ca List

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | query | value | 否 | `` | 按CA筛选 |
| `group_name` | query | value | 否 | `` | 按群组名称筛选 |
| `username` | query | value | 否 | `` | 按用户名筛选 |
| `date` | query | value | 否 | `` | 按日期筛选(create_time的日期部分)，格式YYYY-MM-DD |
| `skip` | query | integer | 否 | `0` | 跳过记录数 |
| `limit` | query | integer | 否 | `100` | 返回记录数限制 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `ca` | string | null | 合约地址或代币 CA。 |
| `username` | string | 用户名。 |
| `group_name` | string | 群组名称。 |
| `create_time` | string | 群消息创建时间。 |
| `symbol` | string | null | 代币符号或 ticker。 |
| `chain` | string | null | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `token_name` | string | null | 代币名称或 Token 名称。 |
| `market_cap` | string | null | 市值。 |
| `first_market_cap` | string | null | 首次提及时的市值。 |
| `age` | integer | null | 代币年龄或从创建到提及的时间。 |
| `total_mentions` | integer | null | 提及次数。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `twitter` | string | null | Twitter/X 链接。 |
| `telegram` | string | null | Telegram 链接。 |
| `website` | string | null | 项目官网链接。 |
| `image_url` | string | null | 图片 URL。 |
| `icon` | string | null | 图标 URL。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `m5_buys` | integer | null | 5 分钟买入次数。 |
| `h1_buys` | integer | null | 1 小时买入次数。 |
| `h6_buys` | integer | null | 6 小时买入次数。 |
| `h24_buys` | integer | null | 24 小时买入次数。 |
| `user_content` | string | null | 群友原始讨论内容。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `liquidity` | number | null | 流动性。 |
| `volume_24h` | number | null | 24 小时成交量。 |
| `mc_change` | object | null | 首次提及市值与当前市值的变化。 |
| `fomo_holdings_pct` | number | null | GroupCaResponse 的 fomo holdings pct 字段。 |
| `fomo_holders` | integer | null | GroupCaResponse 的 fomo holders 字段。 |
| `holders_onchain` | integer | null | GroupCaResponse 的 holders onchain 字段。 |
| `fomo_thesis_count` | integer | null | GroupCaResponse 的 fomo thesis count 字段。 |

示例：`GET https://juhequn.com/api/v1/group_ca`

### Group CA Board

#### GET `/api/v1/group_ca/board/ca/{ca_address}`

Get Group Ca Board Detail

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca_address` | path | string | 是 | `` | 完整合约地址。 |
| `days` | query | value | 否 | `` | 最近多少天 |
| `group_name` | query | value | 否 | `` | 群组名称筛选 |
| `limit` | query | integer | 否 | `100` | 上下文条数 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/board/ca/{ca_address}`

#### GET `/api/v1/group_ca/board/ca/{ca_address}/kline`

Get Group Ca Board Kline

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca_address` | path | string | 是 | `` | 完整合约地址。 |
| `range` | query | string | 否 | `24h` | K线范围 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/board/ca/{ca_address}/kline`

#### GET `/api/v1/group_ca/board/summary`

Get Group Ca Board Summary

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `days` | query | integer | 否 | `1` | 最近多少天 |
| `group_name` | query | value | 否 | `` | 群组名称筛选 |
| `chain` | query | value | 否 | `` | 链筛选 |
| `keyword` | query | value | 否 | `` | 搜索 CA、符号、项目名、叙事、群名或用户名 |
| `sort` | query | string | 否 | `mentions` | 排序 |
| `limit` | query | integer | 否 | `60` | 返回 CA 数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/board/summary`

### Group CA

#### GET `/api/v1/group_ca/by-ca/{ca_address}`

Get Groups By Ca

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca_address` | path | string | 是 | `` | 完整合约地址。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/by-ca/{ca_address}`

#### GET `/api/v1/group_ca/by-username/{username}`

根据用户名查询该用户发过的所有CA列表

- 返回最近发布的CA（按时间倒序）
- 最多返回50条记录

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `username` | path | string | 是 | `` | 群友用户名。 |
| `limit` | query | integer | 否 | `50` | 返回记录数，最多50条 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `username` | string | 用户名。 |
| `total_cas` | integer | CA 记录总数。 |
| `cas` | array | CA 记录列表。 |

示例：`GET https://juhequn.com/api/v1/group_ca/by-username/{username}`

#### GET `/api/v1/group_ca/call-leaderboard`

Get Call Leaderboard

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `limit` | query | integer | 否 | `80` | 返回提及人 Top N，最大 80 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `data` | array | 提及人喊单排行榜。 |
| `total_callers` | integer | 全部成功喊单提及人数。 |
| `total_successful_cas` | integer | 全部最高倍数至少 10x 的唯一成功 CA 数。 |
| `generated_at` | string | 榜单最近更新时间；空榜时为本次请求时间。 |

示例：`GET https://juhequn.com/api/v1/group_ca/call-leaderboard`

#### GET `/api/v1/group_ca/http`

查询CA为空的记录（通常是HTTP链接）
- 不传date：返回最近20条CA为NULL的记录
- 传date：返回指定日期CA为NULL的记录

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `date` | query | value | 否 | `` | 按日期筛选(create_time的日期部分)，格式YYYY-MM-DD，不传则返回最近20条 |
| `limit` | query | integer | 否 | `20` | 返回记录数限制，默认20条 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/http`

#### GET `/api/v1/group_ca/info/{ca_address}`

Get Ca Info Detail

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca_address` | path | string | 是 | `` | 完整合约地址。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `ca` | string | 合约地址或代币 CA。 |
| `symbol` | string | null | 代币符号或 ticker。 |
| `chain` | string | null | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `token_name` | string | null | 代币名称或 Token 名称。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `twitter` | string | null | Twitter/X 链接。 |
| `telegram` | string | null | Telegram 链接。 |
| `website` | string | null | 项目官网链接。 |
| `image_url` | string | null | 图片 URL。 |
| `icon` | string | null | 图标 URL。 |
| `market_cap` | string | null | 市值。 |
| `latest_market_cap` | number | null | 最近一次记录到的市值。 |
| `first_market_cap` | string | null | 首次提及时的市值。 |
| `age` | integer | null | 代币年龄或从创建到提及的时间。 |
| `liquidity` | number | null | 流动性。 |
| `volume_24h` | number | null | 24 小时成交量。 |
| `m5_buys` | integer | null | 5 分钟买入次数。 |
| `h1_buys` | integer | null | 1 小时买入次数。 |
| `h6_buys` | integer | null | 6 小时买入次数。 |
| `h24_buys` | integer | null | 24 小时买入次数。 |
| `total_mentions` | integer | null | 提及次数。 |
| `group_mentions` | integer | null | 按群组聚合的提及统计。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `user_content` | value | null | 群友原始讨论内容。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `updated_at` | string | null | 更新时间。 |
| `fomo_holdings_pct` | number | null | CaInfoDetailResponse 的 fomo holdings pct 字段。 |
| `fomo_holders` | integer | null | CaInfoDetailResponse 的 fomo holders 字段。 |
| `holders_onchain` | integer | null | CaInfoDetailResponse 的 holders onchain 字段。 |
| `fomo_thesis_count` | integer | null | CaInfoDetailResponse 的 fomo thesis count 字段。 |
| `fomo_theses` | value | null | CaInfoDetailResponse 的 fomo theses 字段。 |

示例：`GET https://juhequn.com/api/v1/group_ca/info/{ca_address}`

#### GET `/api/v1/group_ca/latest`

按数据库 ID 倒序返回最新 CA 记录。

连续翻页时，首请求只传 limit；后续请求把上一页最小 id 作为 before_id。
page、size、offset、skip 不参与分页，传入时会作为未知参数被忽略。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | value | 否 | `` | 查询最近多少小时，不传则不过滤 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `limit` | query | integer | 否 | `1` | 返回最新CA的数量，默认1条 |
| `include_discussion` | query | boolean | 否 | `False` | 是否返回群组讨论原文大字段；首屏建议关闭 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `ca` | string | null | 合约地址或代币 CA。 |
| `username` | string | 用户名。 |
| `group_name` | string | 群组名称。 |
| `create_time` | string | 群消息创建时间。 |
| `symbol` | string | null | 代币符号或 ticker。 |
| `chain` | string | null | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `token_name` | string | null | 代币名称或 Token 名称。 |
| `market_cap` | string | null | 市值。 |
| `first_market_cap` | string | null | 首次提及时的市值。 |
| `age` | integer | null | 代币年龄或从创建到提及的时间。 |
| `total_mentions` | integer | null | 提及次数。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `twitter` | string | null | Twitter/X 链接。 |
| `telegram` | string | null | Telegram 链接。 |
| `website` | string | null | 项目官网链接。 |
| `image_url` | string | null | 图片 URL。 |
| `icon` | string | null | 图标 URL。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `m5_buys` | integer | null | 5 分钟买入次数。 |
| `h1_buys` | integer | null | 1 小时买入次数。 |
| `h6_buys` | integer | null | 6 小时买入次数。 |
| `h24_buys` | integer | null | 24 小时买入次数。 |
| `user_content` | string | null | 群友原始讨论内容。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `liquidity` | number | null | 流动性。 |
| `volume_24h` | number | null | 24 小时成交量。 |
| `mc_change` | object | null | 首次提及市值与当前市值的变化。 |
| `fomo_holdings_pct` | number | null | GroupCaResponse 的 fomo holdings pct 字段。 |
| `fomo_holders` | integer | null | GroupCaResponse 的 fomo holders 字段。 |
| `holders_onchain` | integer | null | GroupCaResponse 的 holders onchain 字段。 |
| `fomo_thesis_count` | integer | null | GroupCaResponse 的 fomo thesis count 字段。 |

示例：`GET https://juhequn.com/api/v1/group_ca/latest`

#### GET `/api/v1/group_ca/popular`

查询热门CA。传 date 时使用旧版按日期统计；未传 date 时使用最近 hours 小时滚动窗口。
返回每个CA的最新记录及出现次数，按提及次数降序。

这是固定热门榜，不提供分页；page、size、offset、skip 等未知参数会被忽略。
如需遍历更多 CA 记录，请改用 /group_ca/latest，并用 before_id 游标向更早记录翻页。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `date` | query | value | 否 | `` | 按日期筛选(create_time的日期部分)，格式YYYY-MM-DD。传入后保持旧版按日期逻辑 |
| `hours` | query | integer | 否 | `24` | 未传 date 时，使用最近 N 小时滚动窗口（最长30天） |
| `min_count` | query | integer | 否 | `10` | 最小提及次数阈值 |
| `group_name` | query | value | 否 | `` | 按群组名称筛选 |
| `include_discussion` | query | boolean | 否 | `True` | 是否返回群组讨论大字段 |
| `limit` | query | integer | 否 | `35` | 返回热门 Top N；服务端最多返回 35 条，超过 35 时按 35 处理 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/popular`

#### GET `/api/v1/group_ca/query_group`

根据CA地址查询群组来源

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | query | string | 是 | `` | CA地址 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/query_group`

#### GET `/api/v1/group_ca/search`

Search Dashboard Target

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `q` | query | string | 是 | `` | 搜索 CA 地址、HTTP 链接或 @X账号 |
| `limit` | query | integer | 否 | `30` | 返回上下文记录数 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/search`

#### GET `/api/v1/group_ca/sync`

Sync Group Ca

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `after_id` | query | integer | 否 | `0` | 客户端已成功处理的最后一条 CA 数据库 ID |
| `until_id` | query | value | 否 | `` | 本轮恢复固定终点；首次请求不传，由服务端确定 |
| `limit` | query | integer | 否 | `200` | 单页返回数量，默认200，最大500 |
| `include_discussion` | query | boolean | 否 | `False` | 是否返回群组讨论原文大字段；断线恢复建议关闭 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `items` | array | 本页按数据库 ID 升序排列的 CA 记录 |
| `next_after_id` | integer | 本页成功处理后应保存的下一次 after_id |
| `until_id` | integer | 本轮恢复固定的数据库 ID 终点 |
| `has_more` | boolean | 固定终点前是否仍有更多记录需要继续拉取 |

示例：`GET https://juhequn.com/api/v1/group_ca/sync`

#### GET `/api/v1/group_ca/user_content`

查询 user_content
- 传 ca：返回该 CA 对应的 user_content 分页
- 传 http：返回该 HTTP 链接对应的 user_content 分页
- ca 和 http 至少传一个
- 默认优先把同群同日内容聚在一起，再按时间升序（最老 -> 最新）；
  order=desc 时整体倒序（最新在前，第 1 页为最新一页）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | query | value | 否 | `` | 按CA筛选 |
| `http` | query | value | 否 | `` | 按HTTP链接筛选 |
| `page` | query | integer | 否 | `1` | 页码，从 1 开始 |
| `limit` | query | integer | 否 | `50` | 每页记录数，默认50条 |
| `order` | query | string | 否 | `asc` | 排序：asc 最老在前（默认）/ desc 最新在前 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/group_ca/user_content`

#### GET `/api/v1/group_ca/{ca_id}`

Get Group Ca By Id

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca_id` | path | integer | 是 | `` | group_ca 记录 ID。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `ca` | string | null | 合约地址或代币 CA。 |
| `username` | string | 用户名。 |
| `group_name` | string | 群组名称。 |
| `create_time` | string | 群消息创建时间。 |
| `symbol` | string | null | 代币符号或 ticker。 |
| `chain` | string | null | 链标识，例如 sol、bsc、base、eth、arbitrum。 |
| `token_name` | string | null | 代币名称或 Token 名称。 |
| `market_cap` | string | null | 市值。 |
| `first_market_cap` | string | null | 首次提及时的市值。 |
| `age` | integer | null | 代币年龄或从创建到提及的时间。 |
| `total_mentions` | integer | null | 提及次数。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `twitter` | string | null | Twitter/X 链接。 |
| `telegram` | string | null | Telegram 链接。 |
| `website` | string | null | 项目官网链接。 |
| `image_url` | string | null | 图片 URL。 |
| `icon` | string | null | 图标 URL。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `m5_buys` | integer | null | 5 分钟买入次数。 |
| `h1_buys` | integer | null | 1 小时买入次数。 |
| `h6_buys` | integer | null | 6 小时买入次数。 |
| `h24_buys` | integer | null | 24 小时买入次数。 |
| `user_content` | string | null | 群友原始讨论内容。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `liquidity` | number | null | 流动性。 |
| `volume_24h` | number | null | 24 小时成交量。 |
| `mc_change` | object | null | 首次提及市值与当前市值的变化。 |
| `fomo_holdings_pct` | number | null | GroupCaResponse 的 fomo holdings pct 字段。 |
| `fomo_holders` | integer | null | GroupCaResponse 的 fomo holders 字段。 |
| `holders_onchain` | integer | null | GroupCaResponse 的 holders onchain 字段。 |
| `fomo_thesis_count` | integer | null | GroupCaResponse 的 fomo thesis count 字段。 |

示例：`GET https://juhequn.com/api/v1/group_ca/{ca_id}`

### Group HTTP

#### GET `/api/v1/group_http`

查询群组中分享的HTTP链接（group_ca表中http不为空的记录）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `group_name` | query | value | 否 | `` | 群组名称筛选 |
| `hours` | query | value | 否 | `` | 查询最近多少小时，不传则不过滤 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `limit` | query | integer | 否 | `20` | 返回记录数限制，默认20 |
| `skip` | query | integer | 否 | `0` | 跳过记录数 |
| `include_discussion` | query | boolean | 否 | `False` | 是否返回群组讨论原文大字段；首屏建议关闭 |
| `dedupe_site` | query | boolean | 否 | `False` | 是否按站点返回最新唯一HTTP记录；用于看板首屏补历史 |
| `site` | query | value | 否 | `` | 按站点key返回该站点下的原始HTTP记录；传 site 时忽略 dedupe_site |
| `date` | query | value | 否 | `` | 按日期筛选(create_time的日期部分)，格式YYYY-MM-DD |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `username` | string | 用户名。 |
| `group_name` | string | 群组名称。 |
| `create_time` | string | null | 群消息创建时间。 |
| `http` | string | null | 群消息中的 HTTP 链接。 |
| `name` | string | null | 名称。 |
| `description` | string | null | 描述信息。 |
| `member_count` | integer | null | 社群成员数量。 |
| `community_id` | string | null | X/Twitter 社群 ID。 |
| `user_content` | array | string | null | 群友原始讨论内容。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `x_content` | string | null | 关联 X/Twitter 内容。 |
| `liquidity` | number | null | 流动性。 |
| `volume_24h` | number | null | 24 小时成交量。 |
| `total_mentions` | integer | null | 提及次数。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `from_tweet` | boolean | null | 是否来源于 X/Twitter 推文。 |
| `tweet_id` | string | null | 推文 ID。 |
| `tweet_url` | string | null | 推文 URL。 |
| `tweet_created_at` | integer | null | 推文发布时间。 |
| `author_screen_name` | string | null | X/Twitter 作者 screen name。 |
| `author_name` | string | null | 作者显示名称。 |
| `site_key` | string | null | 站点标识 key。 |
| `site_record_count` | integer | null | 该站点下的记录数。 |

示例：`GET https://juhequn.com/api/v1/group_http`

### 群主发言

#### GET `/api/v1/group_owner_messages/latest`

Get Latest Group Owner Messages

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | integer | 否 | `24` | 查询最近多少小时的群主发言 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `group_name` | query | value | 否 | `` | 按群名筛选 |
| `owner_name` | query | value | 否 | `` | 按群主筛选（支持逗号分隔） |
| `names` | query | value | 否 | `` | 按多个群主筛选（可重复传参） |
| `limit` | query | integer | 否 | `30` | 返回数量，默认30 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `type` | string | 事件或记录类型。 |
| `id` | integer | 记录 ID。 |
| `source` | string | 数据来源。 |
| `group_name` | string | 群组名称。 |
| `owner_name` | string | 群主名称。 |
| `sender_name` | string | 发送者名称。 |
| `message_type` | string | 消息类型。 |
| `content` | string | null | 正文内容。 |
| `image_url` | string | null | 图片 URL。 |
| `image_key` | string | null | 飞书图片 image_key。 |
| `reply_user` | string | null | 被回复用户。 |
| `reply_content` | string | null | 回复的原消息内容。 |
| `source_msg_id` | string | null | 来源消息 ID。 |
| `source_group_id` | string | null | 来源群组 ID。 |
| `telegram_message_url` | string | null | Telegram 原消息链接。 |
| `created_at` | string | null | 创建时间。 |
| `received_at` | string | null | 服务端接收时间。 |
| `duplicate` | boolean | 是否被判定为重复消息。 |

示例：`GET https://juhequn.com/api/v1/group_owner_messages/latest`

#### GET `/api/v1/group_owner_messages/owners`

Get Group Owner Names

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `days` | query | integer | 否 | `30` | 统计最近多少天出现过的群主 |
| `limit` | query | integer | 否 | `200` | 最多返回多少个群主 |
| `configured_only` | query | boolean | 否 | `False` | 仅返回 group_owner_speakers 中启用的群主和关注用户 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `name` | string | 名称。 |
| `username` | string | null | 用户名。 |
| `display_name` | string | null | 展示名称。 |
| `count` | integer | 统计数量。 |
| `latest_at` | string | null | 最近一次出现时间。 |
| `sector` | string | null | 板块或分组。 |

示例：`GET https://juhequn.com/api/v1/group_owner_messages/owners`

#### GET `/api/v1/group_owner_messages/sync`

Sync Group Owner Messages

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `after_id` | query | integer | 否 | `0` | 客户端已成功处理的最后一条数据库 ID |
| `until_id` | query | value | 否 | `` | 本轮恢复固定终点；首次请求不传，由服务端确定 |
| `group_name` | query | value | 否 | `` | 按群名筛选 |
| `owner_name` | query | value | 否 | `` | 按群主筛选（支持逗号分隔） |
| `names` | query | value | 否 | `` | 按多个群主筛选（可重复传参） |
| `limit` | query | integer | 否 | `200` | 单页返回数量，默认200，最大500 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `items` | array | 本页按数据库 ID 升序排列的群主发言 |
| `next_after_id` | integer | 本页成功处理后应保存的下一次 after_id |
| `until_id` | integer | 本轮恢复固定的数据库 ID 终点 |
| `has_more` | boolean | 固定终点前是否仍有更多记录需要继续拉取 |

示例：`GET https://juhequn.com/api/v1/group_owner_messages/sync`

### KOL Holdings

#### GET `/api/v1/kol_holdings/board`

KOL 持仓看板：钱包快照 + fomo 买卖事件推算，按 token 聚合。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `user_ids` | query | value | 否 | `` | 逗号分隔的用户 id，空=全部启用用户 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/kol_holdings/board`

#### GET `/api/v1/kol_holdings/gmgn_holders`

GMGN 标签 KOL holders 按需明细（hover KOL持仓 chip 时按 CA 拉取）。

按 CA 定向查询 ca_kol_holders（不再全表扫）；
快照超过 GMGN_HOLDERS_ONDEMAND_TTL_HOURS（默认 6h）或不存在的 CA 先同步补拉 GMGN 再返回。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `ca` | query | string | 是 | `` | 逗号分隔的 token CA，按需查询 GMGN 标签 KOL holders |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/kol_holdings/gmgn_holders`

#### GET `/api/v1/kol_holdings/users`

KOL 用户列表（仅启用），含钱包数量。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/kol_holdings/users`

### KOL Tracker

#### GET `/api/v1/kol_tracker/{slug}/board`

Get Kol Tracker Board

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `slug` | path | string | 是 | `` | KOL Tracker 数据源标识，例如 chuanmu。 |
| `ticker` | query | value | 否 | `` | 选中的 ticker |
| `keyword` | query | value | 否 | `` | 关键词搜索 |
| `days` | query | value | 否 | `` | 最近 N 天 |
| `limit` | query | integer | 否 | `100` | 详情提及数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/kol_tracker/{slug}/board`

#### POST `/api/v1/kol_tracker/{slug}/refresh`

Refresh Kol Tracker

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `slug` | path | string | 是 | `` | KOL Tracker 数据源标识，例如 chuanmu。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`POST https://juhequn.com/api/v1/kol_tracker/{slug}/refresh`

#### GET `/api/v1/kol_tracker/{slug}/ticker/{ticker}/price-line`

Get Kol Tracker Price Line

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `slug` | path | string | 是 | `` | KOL Tracker 数据源标识，例如 chuanmu。 |
| `ticker` | path | string | 是 | `` | 美股 ticker 或标的代码。 |
| `range` | query | string | 否 | `1m` | 价格或 K 线数据的时间范围。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/kol_tracker/{slug}/ticker/{ticker}/price-line`

### Market

#### GET `/api/v1/market/prices`

Get Market Prices

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `source` | string | 数据来源。 |
| `updated_at` | string | 更新时间。 |
| `refresh_interval_seconds` | integer | 建议刷新间隔秒数。 |
| `items` | array | 结果列表。 |
| `error` | string | null | 错误信息。 |

示例：`GET https://juhequn.com/api/v1/market/prices`

### Media

#### GET `/api/v1/media/feishu_image`

Proxy Feishu Image

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `key` | query | string | 是 | `` | 飞书图片 key（img_v3_*） |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/media/feishu_image`

#### GET `/api/v1/media/image`

Proxy Image

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `url` | query | string | 是 | `` | 需要代理的图片URL |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/media/image`

### 快讯消息

#### GET `/api/v1/newsflash/date/{date}`

获取指定日期的快讯消息

date格式: YYYY-MM-DD (例如: 2026-03-07)

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `date` | path | string | 是 | `` | 日期，格式 YYYY-MM-DD；部分快捷接口也支持 today、yesterday、week、month。 |
| `limit` | query | integer | 否 | `100` | 返回数量限制 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `add_time` | integer | 快讯发布时间戳。 |
| `title` | string | 标题。 |
| `content` | string | 正文内容。 |
| `url` | string | null | 链接 URL。 |

示例：`GET https://juhequn.com/api/v1/newsflash/date/{date}`

#### GET `/api/v1/newsflash/latest`

获取最新的快讯消息（不分日期，取最新的N条）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `limit` | query | integer | 否 | `20` | 返回数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `add_time` | integer | 快讯发布时间戳。 |
| `title` | string | 标题。 |
| `content` | string | 正文内容。 |
| `url` | string | null | 链接 URL。 |

示例：`GET https://juhequn.com/api/v1/newsflash/latest`

#### GET `/api/v1/newsflash/today`

获取今天的快讯消息

返回add_time、title、content、url字段
按add_time倒序排列（最新的在前）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `limit` | query | integer | 否 | `100` | 返回数量限制 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `add_time` | integer | 快讯发布时间戳。 |
| `title` | string | 标题。 |
| `content` | string | 正文内容。 |
| `url` | string | null | 链接 URL。 |

示例：`GET https://juhequn.com/api/v1/newsflash/today`

### Notifications

#### POST `/api/v1/notifications/channels/test`

Post Notification Channel Test

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**请求体字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `provider` | string | 通知渠道提供商。 |
| `config` | object | 渠道或规则的扩展配置。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`POST https://juhequn.com/api/v1/notifications/channels/test`

#### GET `/api/v1/notifications/config`

Get Notification Config

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/notifications/config`

#### PUT `/api/v1/notifications/config`

Put Notification Config

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**请求体字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `channels` | array | 通知渠道或频道列表。 |
| `rules` | array | 通知规则列表。 |
| `deleted_channel_ids` | array | 需要删除的通知渠道 ID 列表。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`PUT https://juhequn.com/api/v1/notifications/config`

#### GET `/api/v1/notifications/history`

Get Notification History

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `limit` | query | integer | 否 | `50` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/notifications/history`

#### GET `/api/v1/notifications/options`

Get Notification Options

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `days` | query | integer | 否 | `30` | 最近 N 天的统计窗口。 |
| `limit` | query | integer | 否 | `300` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/notifications/options`

### Popular HTTP

#### GET `/api/v1/popular_http`

查询热门HTTP链接：
- 从 group_ca 表实时聚合最近窗口内的群聊 HTTP 提及
- 合并去重后按 total_mentions 降序返回

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | integer | 否 | `48` | 时间窗口小时数，默认48h |
| `min_count` | query | integer | 否 | `1` | 最小提及次数阈值 |
| `limit` | query | integer | 否 | `20` | 返回记录数限制，默认20 |
| `skip` | query | integer | 否 | `0` | 跳过记录数 |
| `include_discussion` | query | boolean | 否 | `True` | 是否返回群组讨论和X内容大字段 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `http` | string | null | 群消息中的 HTTP 链接。 |
| `name` | string | null | 名称。 |
| `description` | string | null | 描述信息。 |
| `member_count` | integer | null | 社群成员数量。 |
| `community_id` | string | null | X/Twitter 社群 ID。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `user_content` | array | string | null | 群友原始讨论内容。 |
| `x_content` | string | null | 关联 X/Twitter 内容。 |
| `total_mentions` | integer | null | 提及次数。 |
| `x_mention_count` | integer | null | X/Twitter 提及次数。 |
| `tweet_count` | integer | null | 推文数量。 |
| `last_seen_at` | integer | null | 最近一次出现时间。 |
| `has_discussion` | boolean | null | 是否存在关联讨论内容。 |
| `discussion_count` | integer | null | 关联讨论数量。 |
| `from_tweet` | boolean | null | 是否来源于 X/Twitter 推文。 |
| `tweet_id` | string | null | 推文 ID。 |
| `tweet_url` | string | null | 推文 URL。 |
| `tweet_created_at` | integer | null | 推文发布时间。 |
| `author_screen_name` | string | null | X/Twitter 作者 screen name。 |
| `author_name` | string | null | 作者显示名称。 |

示例：`GET https://juhequn.com/api/v1/popular_http`

### 电台模式

#### GET `/api/v1/radio/groups`

启用的群列表 + 可选音色（Bearer token 鉴权）。

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/radio/groups`

### 二级KOL

#### GET `/api/v1/second_kol`

获取二级KOL输出数据（自动过滤包含屏蔽词的内容）

默认按sent_at倒序排列（最新的在前面）
注意：@everyone 并不是代币ONE

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `token` | query | value | 否 | `` | Token筛选 |
| `start_date` | query | value | 否 | `` | 开始日期 YYYY-MM-DD |
| `end_date` | query | value | 否 | `` | 结束日期 YYYY-MM-DD |
| `limit` | query | integer | 否 | `50` | 返回数量，默认50 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |

示例：`GET https://juhequn.com/api/v1/second_kol`

#### POST `/api/v1/second_kol/ingest`

写入 second_kol 的内部接入接口（供 TG 侧采集器如 aoying.py 调用）。

与 /telegram 一样是内网接入口，但要求 X-Ingest-Token 头与环境变量
SECOND_KOL_INGEST_TOKEN 匹配（未配置环境变量时拒绝写入，避免裸奔）。
幂等：同一 KOL 相同内容 30 分钟内重复推送只保留一条。

**请求体字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `sent_at` | string | null | 发送时间。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`POST https://juhequn.com/api/v1/second_kol/ingest`

#### GET `/api/v1/second_kol/kol/{kol_name}`

获取指定KOL的输出数据（自动过滤包含屏蔽词的内容）

按sent_at倒序排列

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `kol_name` | path | string | 是 | `` | KOL 或博主名称。 |
| `limit` | query | integer | 否 | `50` | 返回数量 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |

示例：`GET https://juhequn.com/api/v1/second_kol/kol/{kol_name}`

#### GET `/api/v1/second_kol/kol_names`

获取所有KOL名称列表及其消息数量

返回所有不重复的KOL名称和对应的消息数量

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `kol_name` | string | KOL 或博主名称。 |
| `count` | integer | 统计数量。 |

示例：`GET https://juhequn.com/api/v1/second_kol/kol_names`

#### GET `/api/v1/second_kol/latest`

获取最新的二级KOL数据（自动过滤包含屏蔽词的内容）

按sent_at倒序，返回最新的N条记录

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | value | 否 | `` | 查询最近多少小时，默认不过滤 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `limit` | query | integer | 否 | `10` | 返回最新数量，默认10 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |

示例：`GET https://juhequn.com/api/v1/second_kol/latest`

### Smart Money

#### GET `/api/v1/smart_money`

获取聪明钱交易信号数据

- 支持按 trader_name 模糊查询
- 支持快捷日期（today/yesterday/week/month）或自定义日期范围
- 默认返回最近24小时数据
- 按 message_time 倒序排列

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `trader_name` | query | value | 否 | `` | 按交易员名称筛选，支持模糊匹配 |
| `date` | query | value | 否 | `` | 快捷日期: today, yesterday, week, month |
| `start_date` | query | value | 否 | `` | 开始日期 YYYY-MM-DD |
| `end_date` | query | value | 否 | `` | 结束日期 YYYY-MM-DD |
| `symbol` | query | value | 否 | `` | 按交易对筛选 |
| `position` | query | value | 否 | `` | 按持仓方向筛选 (BUY/SELL) |
| `limit` | query | integer | 否 | `50` | 返回数量，默认50 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `message_id` | string | null | 消息 ID。 |
| `user_id` | string | null | 用户 ID。 |
| `trader_name` | string | null | 交易员名称。 |
| `symbol` | string | null | 代币符号或 ticker。 |
| `unit` | string | null | 数量单位。 |
| `avg_price` | string | null | 成交均价。 |
| `executed_qty` | string | null | 已成交数量。 |
| `executed_quote_qty` | string | null | 已成交计价金额。 |
| `position` | string | null | 持仓方向或交易方向。 |
| `content` | string | null | 正文内容。 |
| `ts` | string | null | 事件时间戳。 |
| `message_time` | string | null | 消息时间。 |
| `created_at` | string | null | 创建时间。 |

示例：`GET https://juhequn.com/api/v1/smart_money`

### AI Summaries

#### GET `/api/v1/summaries`

获取AI总结列表

- 需要有效的API Token
- Token有过期时间

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `group_name` | query | value | 否 | `` | 按群组名称筛选 |
| `start_date` | query | value | 否 | `` | 开始日期 (ISO 8601格式) |
| `end_date` | query | value | 否 | `` | 结束日期 (ISO 8601格式) |
| `date` | query | value | 否 | `` | 按日期筛选(create_at的日期部分)，格式YYYY-MM-DD |
| `skip` | query | integer | 否 | `0` | 跳过记录数 |
| `limit` | query | integer | 否 | `20` | 返回记录数限制，默认20条 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | 记录 ID。 |
| `group_name` | string | 群组名称。 |
| `content` | string | 正文内容。 |
| `create_at` | string | 创建时间。 |

示例：`GET https://juhequn.com/api/v1/summaries`

#### GET `/api/v1/summaries/groups`

获取所有群组名称列表

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/summaries/groups`

#### GET `/api/v1/summaries/narratives`

获取每日叙事

查询提及CA次数最多的前N条的summary（代币叙事）字段
- 不传date：默认查询最新300条中统计
- 传date：查询指定日期的记录

返回按CA出现次数降序排列的前N条记录的summary

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `date` | query | value | 否 | `` | 按日期筛选，格式YYYY-MM-DD |
| `group_name` | query | value | 否 | `` | 按群组名称筛选 |
| `limit` | query | integer | 否 | `10` | 返回记录数限制，默认10 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `ca` | string | 合约地址或代币 CA。 |
| `summary` | string | null | 摘要或叙事说明。 |
| `count` | integer | 统计数量。 |

示例：`GET https://juhequn.com/api/v1/summaries/narratives`

### TG Call

#### GET `/api/v1/tg_call/channels`

Get Tg Call Channels

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `days` | query | integer | 否 | `30` | 统计最近多少天出现过的频道 |
| `limit` | query | integer | 否 | `200` | 最多返回多少个频道 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `channel_key` | string | TG Call 频道配置 key。 |
| `channel_name` | string | TG Call 频道名称。 |
| `channel_id` | string | 通知渠道 ID。 |
| `category` | string | 榜单或分类名称。 |
| `count` | integer | 统计数量。 |
| `latest_at` | string | null | 最近一次出现时间。 |

示例：`GET https://juhequn.com/api/v1/tg_call/channels`

#### GET `/api/v1/tg_call/latest`

Get Latest Tg Call Messages

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | integer | 否 | `24` | 查询最近多少小时的 TG Call |
| `channels` | query | value | 否 | `` | 按频道筛选，支持逗号分隔 |
| `channel` | query | value | 否 | `` | 按多个频道筛选，可重复传参 |
| `limit` | query | integer | 否 | `30` | 返回数量 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `type` | string | 事件或记录类型。 |
| `id` | integer | 记录 ID。 |
| `channel_key` | string | TG Call 频道配置 key。 |
| `channel_name` | string | TG Call 频道名称。 |
| `channel_id` | string | 通知渠道 ID。 |
| `category` | string | 榜单或分类名称。 |
| `sender_name` | string | null | 发送者名称。 |
| `sender_id` | string | null | 发送者 ID。 |
| `message_type` | string | 消息类型。 |
| `content` | string | null | 正文内容。 |
| `content_zh` | string | null | TgCallMessageResponse 的 content zh 字段。 |
| `reply_content` | string | null | 回复的原消息内容。 |
| `source_msg_id` | string | null | 来源消息 ID。 |
| `ca_values` | array | 消息中识别到的 CA 列表。 |
| `urls` | array | 链接 URL 列表。 |
| `image_urls` | array | 图片 URL 列表。 |
| `created_at` | string | null | 创建时间。 |
| `received_at` | string | null | 服务端接收时间。 |

示例：`GET https://juhequn.com/api/v1/tg_call/latest`

### TG 快讯

#### GET `/api/v1/tg_newsflash/latest`

Get Latest Tg Newsflash Messages

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | integer | 否 | `24` | 查询最近多少小时的 TG 快讯 |
| `limit` | query | integer | 否 | `30` | 返回数量 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早快讯 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `type` | string | 事件或记录类型。 |
| `id` | integer | 记录 ID。 |
| `source_key` | string | TG 快讯源配置 key。 |
| `source_name` | string | TG 快讯源名称。 |
| `source_id` | string | 上游平台的唯一 ID。 |
| `sender_name` | string | null | 发送者名称。 |
| `sender_id` | string | null | 发送者 ID。 |
| `message_type` | string | 消息类型。 |
| `content` | string | null | 正文内容。 |
| `source_msg_id` | string | null | 来源消息 ID。 |
| `urls` | array | 链接 URL 列表。 |
| `image_urls` | array | 图片 URL 列表。 |
| `created_at` | string | null | 创建时间。 |
| `received_at` | string | null | 服务端接收时间。 |

示例：`GET https://juhequn.com/api/v1/tg_newsflash/latest`

#### GET `/api/v1/tg_newsflash/sources`

Get Tg Newsflash Sources

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `days` | query | integer | 否 | `30` | 统计最近多少天出现过的 TG 快讯源 |
| `limit` | query | integer | 否 | `200` | 最多返回多少个快讯源 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `source_key` | string | TG 快讯源配置 key。 |
| `source_name` | string | TG 快讯源名称。 |
| `source_id` | string | 上游平台的唯一 ID。 |
| `count` | integer | 统计数量。 |
| `latest_at` | string | null | 最近一次出现时间。 |

示例：`GET https://juhequn.com/api/v1/tg_newsflash/sources`

### Token管理

#### GET `/api/v1/token/usage`

查询当前Token的调用次数使用情况

返回：
- 每日限制次数
- 今日已使用次数
- 今日剩余次数
- 重置时间（次日0点）

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `token` | string | Token 值、代币符号或脱敏后的访问 Token。 |
| `token_name` | string | null | 代币名称或 Token 名称。 |
| `expires_at` | string | null | Token 过期时间。 |
| `daily_limit` | integer | Token 每日调用上限。 |
| `used_today` | integer | 今日已调用次数。 |
| `remaining_today` | integer | 今日剩余可调用次数。 |
| `reset_time` | string | 今日调用次数重置时间。 |

示例：`GET https://juhequn.com/api/v1/token/usage`

### translate

#### POST `/api/v1/translate/zh`

Translate To Simplified Chinese

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**请求体字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `text` | string | 待处理文本。 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `translated_text` | string | 翻译后的文本。 |
| `cached` | boolean | 是否命中缓存。 |

示例：`POST https://juhequn.com/api/v1/translate/zh`

### 美股KOL

#### GET `/api/v1/us_stock_kol`

Get Us Stock Kol

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `token` | query | value | 否 | `` | Token筛选 |
| `start_date` | query | value | 否 | `` | 开始日期 YYYY-MM-DD |
| `end_date` | query | value | 否 | `` | 结束日期 YYYY-MM-DD |
| `limit` | query | integer | 否 | `50` | 返回数量，默认50 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |
| `mentioned_tokens` | value | null | 正文中识别到的 token/ticker 列表。 |
| `mentioned_token_fields` | object | null | 命中的 token/ticker 字段来源。 |

示例：`GET https://juhequn.com/api/v1/us_stock_kol`

#### GET `/api/v1/us_stock_kol/board/summary`

Get Us Stock Kol Board Summary

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `keyword` | query | value | 否 | `` | 关键词搜索，匹配 ticker、KOL、正文或推文ID |
| `days` | query | value | 否 | `` | 最近多少天 |
| `token` | query | value | 否 | `` | Ticker筛选 |
| `limit` | query | integer | 否 | `30` | 最新观点返回数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/us_stock_kol/board/summary`

#### GET `/api/v1/us_stock_kol/board/token/{token_symbol}`

Get Us Stock Kol Board Token

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `token_symbol` | path | string | 是 | `` | 美股 ticker 或标的代码。 |
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `keyword` | query | value | 否 | `` | 关键词搜索，匹配 KOL、正文或推文ID |
| `days` | query | value | 否 | `` | 最近多少天 |
| `limit` | query | integer | 否 | `50` | 返回提及数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/us_stock_kol/board/token/{token_symbol}`

#### GET `/api/v1/us_stock_kol/board/token/{token_symbol}/kline`

Get Us Stock Kol Board Token Kline

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `token_symbol` | path | string | 是 | `` | 美股 ticker 或标的代码。 |
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `keyword` | query | value | 否 | `` | 关键词搜索，匹配 KOL、正文或推文ID |
| `days` | query | value | 否 | `` | 最近多少天 |
| `interval` | query | string | 否 | `1h` | K线周期 |
| `limit` | query | integer | 否 | `120` | K线数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/us_stock_kol/board/token/{token_symbol}/kline`

#### GET `/api/v1/us_stock_kol/board/token/{token_symbol}/price-line`

Get Us Stock Kol Board Token Price Line

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `token_symbol` | path | string | 是 | `` | 美股 ticker 或标的代码。 |
| `kol_name` | query | value | 否 | `` | KOL名称筛选 |
| `keyword` | query | value | 否 | `` | 关键词搜索，匹配 KOL、正文或推文ID |
| `days` | query | value | 否 | `` | 最近多少天 |
| `range` | query | string | 否 | `1m` | 价格折线范围 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`GET https://juhequn.com/api/v1/us_stock_kol/board/token/{token_symbol}/price-line`

#### GET `/api/v1/us_stock_kol/kol/{kol_name}`

Get Us Stock Kol By Name

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `kol_name` | path | string | 是 | `` | KOL 或博主名称。 |
| `limit` | query | integer | 否 | `50` | 返回数量 |
| `skip` | query | integer | 否 | `0` | 跳过数量 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |
| `mentioned_tokens` | value | null | 正文中识别到的 token/ticker 列表。 |
| `mentioned_token_fields` | object | null | 命中的 token/ticker 字段来源。 |

示例：`GET https://juhequn.com/api/v1/us_stock_kol/kol/{kol_name}`

#### GET `/api/v1/us_stock_kol/kol_names`

Get Us Stock Kol Names

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `kol_name` | string | KOL 或博主名称。 |
| `count` | integer | 统计数量。 |

示例：`GET https://juhequn.com/api/v1/us_stock_kol/kol_names`

#### GET `/api/v1/us_stock_kol/latest`

Get Latest Us Stock Kol

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `hours` | query | value | 否 | `` | 查询最近多少小时，默认不过滤 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 id 小于此值的更早记录 |
| `limit` | query | integer | 否 | `10` | 返回最新数量，默认10 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` | integer | null | 记录 ID。 |
| `kol_name` | string | KOL 或博主名称。 |
| `content` | string | null | 正文内容。 |
| `token` | string | null | Token 值、代币符号或脱敏后的访问 Token。 |
| `image_urls` | array | null | 图片 URL 列表。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `sent_at` | string | 发送时间。 |
| `created_at` | string | null | 创建时间。 |
| `mentioned_tokens` | value | null | 正文中识别到的 token/ticker 列表。 |
| `mentioned_token_fields` | object | null | 命中的 token/ticker 字段来源。 |

示例：`GET https://juhequn.com/api/v1/us_stock_kol/latest`

### FOMO与pump.fun喊单

#### GET `/api/v1/wind_callouts/latest`

获取最新 FOMO / pump.fun 事件

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `platform` | query | string | 是 | `` | fomo / pumpfun / pump.fun / evm / ave |
| `action_type` | query | value | 否 | `` | 按动作类型过滤（buy/sell/consensus/...），不传返回全部。 |
| `before_id` | query | value | 否 | `` | 游标分页：返回 ID 小于该值的更早记录。 |
| `limit` | query | integer | 否 | `30` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `data` | array | 接口返回的数据主体。 |

示例：`GET https://juhequn.com/api/v1/wind_callouts/latest`

### Wind 实时推文

#### GET `/api/v1/wind_x/latest`

获取 Wind 实时推文事件

**请求参数**:

| 参数 | 位置 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|---|
| `before_id` | query | value | 否 | `` | 游标分页：返回 ID 小于该值的更早记录。 |
| `limit` | query | integer | 否 | `30` | 返回记录数上限。 |
| `authorization` | header | value | 否 | `` | Bearer Token认证 |

**响应字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `status` | string | 状态。 |
| `data` | array | 接口返回的数据主体。 |

示例：`GET https://juhequn.com/api/v1/wind_x/latest`

### Telegram Ingest

#### POST `/telegram`

Receive Telegram Message

**请求体字段**:

| 字段 | 类型 | 说明 |
|---|---|---|
| `chat` | string | null | Telegram 聊天对象信息。 |
| `type` | string | null | 事件或记录类型。 |
| `username` | string | null | 用户名。 |
| `msg` | value | Telegram 原始消息正文。 |
| `group_name` | string | null | 群组名称。 |
| `sender_name` | string | null | 发送者名称。 |
| `time` | string | null | 消息时间。 |
| `guid` | string | null | 消息全局唯一标识。 |
| `msg_id` | string | null | Telegram 消息 ID。 |
| `group_id` | string | null | 群组 ID。 |
| `remark_name` | string | null | 备注名称。 |
| `sender_id` | string | null | 发送者 ID。 |
| `room` | string | null | Telegram room 信息。 |
| `user_nick_name` | string | null | 用户昵称。 |
| `msg_type` | string | null | Telegram 消息类型。 |
| `reply_msg` | string | null | 被回复消息内容。 |
| `reply_user` | string | null | 被回复用户。 |
| `avatar` | string | null | 发送者头像 URL。 |
| `icon_url` | string | null | 头像或图标 URL。 |
| `image_url` | string | null | 图片 URL。 |
| `image_key` | string | null | 飞书图片 image_key。 |
| `file_url` | string | null | 文件 URL。 |
| `fileUrl` | string | null | 文件 URL（camelCase 兼容字段）。 |
| `document_url` | string | null | 文档 URL。 |
| `documentUrl` | string | null | 文档 URL（camelCase 兼容字段）。 |
| `attachment_url` | string | null | 附件 URL。 |
| `attachmentUrl` | string | null | 附件 URL（camelCase 兼容字段）。 |

**响应字段**: 动态对象、数组或文件响应；以 OpenAPI schema 和实际响应为准。

示例：`POST https://juhequn.com/telegram`

## WebSocket 全量索引

WebSocket 使用 URL query 传 Token，例如：`wss://juhequn.com/ws/new_ca?token=<your-token>`。

| 路径 | 说明 |
|---|---|
| `/ws/ave_callouts` | ave pro 聪明钱买入信号（platform=ave）推送；支持 last_id、history 和 interval。 |
| `/ws/evm_callouts` | WebSocket 实时推送。 |
| `/ws/fomo_callouts` | FOMO 指定账号喊单、买卖和状态事件；支持 last_id、history 和 interval。 |
| `/ws/gmgn_callouts` | MEME 喊单新增事件推送；支持 last_id、history 和 interval。 |
| `/ws/gmgn_signals` | GMGN 聪明钱/KOL 钱包买卖信号推送；支持 history 历史补发。 |
| `/ws/gmgn_trenches` | GMGN Trenches 已完成项推送。 |
| `/ws/group-owner-messages` | 群主发言历史补发和新增推送；last_id 可连续补齐全部缺口，ack_catchup=1 返回完成事件。 |
| `/ws/new_ca` | 全局新 CA 推送；last_id 可补齐断线期间全部 CA，ack_catchup=1 返回完成事件。 |
| `/ws/new_http` | 全局新 HTTP 链接推送。 |
| `/ws/new_tg_call` | TG Call 历史补发和新增推送。 |
| `/ws/popular_ca` | 热门 CA 排行推送。 |
| `/ws/pump_callouts` | pump.fun 喊单、跟进和买卖事件；支持 last_id、history 和 interval。 |
| `/ws/radio` | 群聊电台（TTS 朗读）订阅；连接后客户端发 subscribe 指定群列表与音色，服务端推送 audio 音频 URL（独立单 worker 实例 ai-radio 提供）。 |
| `/ws/second_kol` | 币圈交易博主新增推送。 |
| `/ws/tg_newsflash` | WebSocket 实时推送。 |
| `/ws/us_stock_kol` | 美股博主新增推送。 |
| `/ws/wind_x` | WebSocket 实时推送。 |

## 当前挂载路径校验

HTTP 路径：

- `/api/v1/ave_pro/hot`
- `/api/v1/ave_pro/pump_list`
- `/api/v1/binance/address/{address}/info`
- `/api/v1/binance/dexscreener/{ca}`
- `/api/v1/binance/market/rank/{category}`
- `/api/v1/binance/meme/rush`
- `/api/v1/binance/token/search`
- `/api/v1/binance/token/{ca}/audit`
- `/api/v1/binance/token/{ca}/info`
- `/api/v1/binance/trading/signals`
- `/api/v1/binance_web3`
- `/api/v1/binance_web3/`
- `/api/v1/binance_web3/{path:path}`
- `/api/v1/gmgn/callout/{ca}`
- `/api/v1/gmgn/hot-searches`
- `/api/v1/gmgn_callouts/latest`
- `/api/v1/gmgn_signals/latest`
- `/api/v1/gmgn_trenches/completed`
- `/api/v1/gmgn_wallet/holdings`
- `/api/v1/gmgn_wallet/trades`
- `/api/v1/gmgn_wallet/wallets`
- `/api/v1/group_ca`
- `/api/v1/group_ca/board/ca/{ca_address}`
- `/api/v1/group_ca/board/ca/{ca_address}/kline`
- `/api/v1/group_ca/board/summary`
- `/api/v1/group_ca/by-ca/{ca_address}`
- `/api/v1/group_ca/by-username/{username}`
- `/api/v1/group_ca/call-leaderboard`
- `/api/v1/group_ca/http`
- `/api/v1/group_ca/info/{ca_address}`
- `/api/v1/group_ca/latest`
- `/api/v1/group_ca/popular`
- `/api/v1/group_ca/query_group`
- `/api/v1/group_ca/search`
- `/api/v1/group_ca/sync`
- `/api/v1/group_ca/user_content`
- `/api/v1/group_ca/{ca_id}`
- `/api/v1/group_http`
- `/api/v1/group_owner_messages/latest`
- `/api/v1/group_owner_messages/owners`
- `/api/v1/group_owner_messages/sync`
- `/api/v1/kol_holdings/board`
- `/api/v1/kol_holdings/gmgn_holders`
- `/api/v1/kol_holdings/users`
- `/api/v1/kol_tracker/{slug}/board`
- `/api/v1/kol_tracker/{slug}/refresh`
- `/api/v1/kol_tracker/{slug}/ticker/{ticker}/price-line`
- `/api/v1/market/prices`
- `/api/v1/media/feishu_image`
- `/api/v1/media/image`
- `/api/v1/newsflash/date/{date}`
- `/api/v1/newsflash/latest`
- `/api/v1/newsflash/today`
- `/api/v1/notifications/channels/test`
- `/api/v1/notifications/config`
- `/api/v1/notifications/config`
- `/api/v1/notifications/history`
- `/api/v1/notifications/options`
- `/api/v1/popular_http`
- `/api/v1/radio/groups`
- `/api/v1/second_kol`
- `/api/v1/second_kol/ingest`
- `/api/v1/second_kol/kol/{kol_name}`
- `/api/v1/second_kol/kol_names`
- `/api/v1/second_kol/latest`
- `/api/v1/smart_money`
- `/api/v1/summaries`
- `/api/v1/summaries/groups`
- `/api/v1/summaries/narratives`
- `/api/v1/tg_call/channels`
- `/api/v1/tg_call/latest`
- `/api/v1/tg_newsflash/latest`
- `/api/v1/tg_newsflash/sources`
- `/api/v1/token/usage`
- `/api/v1/translate/zh`
- `/api/v1/us_stock_kol`
- `/api/v1/us_stock_kol/board/summary`
- `/api/v1/us_stock_kol/board/token/{token_symbol}`
- `/api/v1/us_stock_kol/board/token/{token_symbol}/kline`
- `/api/v1/us_stock_kol/board/token/{token_symbol}/price-line`
- `/api/v1/us_stock_kol/kol/{kol_name}`
- `/api/v1/us_stock_kol/kol_names`
- `/api/v1/us_stock_kol/latest`
- `/api/v1/wind_callouts/latest`
- `/api/v1/wind_x/latest`
- `/telegram`

WebSocket 路径：

- `/ws/ave_callouts`
- `/ws/evm_callouts`
- `/ws/fomo_callouts`
- `/ws/gmgn_callouts`
- `/ws/gmgn_signals`
- `/ws/gmgn_trenches`
- `/ws/group-owner-messages`
- `/ws/new_ca`
- `/ws/new_http`
- `/ws/new_tg_call`
- `/ws/popular_ca`
- `/ws/pump_callouts`
- `/ws/radio`
- `/ws/second_kol`
- `/ws/tg_newsflash`
- `/ws/us_stock_kol`
- `/ws/wind_x`

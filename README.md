# 🔥 AI闪报 — 免费 AI 新闻 API

> 全球实时 AI 新闻聚合平台的开放数据接口，覆盖9大板块，AI自动采集+生成摘要点评，永久免费使用。

📡 网站：[https://aiflash.cc](https://aiflash.cc) | 📖 API文档：[https://aiflash.cc/developers](https://aiflash.cc/developers) | 📰 RSS订阅：[https://aiflash.cc/rss](https://aiflash.cc/rss)

## ✨ 特点

- 🆓 **永久免费** — 无需注册、无需 API Key
- ⚡ **实时更新** — AI板块每10分钟自动采集，其他板块10-60分钟
- 🤖 **AI生成** — 每条新闻由 GPT-4o 生成专业摘要和点评
- 📊 **9大板块** — AI动态、国际新闻、虚拟币、财经股市、科技数码、人物追踪、足球实况、娱乐八卦、地质灾害
- 🏆 **世界杯专题** — 2026美加墨世界杯实时赛程、战报、前瞻
- 📰 **RSS Feed** — 支持主流 RSS 阅读器订阅
- 🔍 **全文搜索** — 支持标题、摘要、AI点评多字段模糊匹配
- 🗳️ **投票系统** — 利好/利空/中立，IP防刷

---

## 🚀 快速开始

### 获取最新AI新闻

```bash
curl "https://aiflash.cc/api/news?section=ai&page=1&page_size=10"
```

### Python 示例

```python
import requests

# 获取AI板块最新20条新闻
resp = requests.get("https://aiflash.cc/api/news", params={
    "section": "ai",
    "page": 1,
    "page_size": 20
})
data = resp.json()
for item in data["items"]:
    print(f"[{item['category']}] {item['title']}")
    print(f"  摘要: {item['summary']}")
    print(f"  AI点评: {item['comment']}")
    print(f"  时间: {item['created_at']}")
    print()
```

### JavaScript 示例

```javascript
fetch("https://aiflash.cc/api/news?section=ai&page=1&page_size=20")
  .then(res => res.json())
  .then(data => {
    data.items.forEach(item => {
      console.log(`[${item.category}] ${item.title}`);
      console.log(`  摘要: ${item.summary}`);
      console.log(`  AI点评: ${item.comment}`);
    });
  });
```

---

## 📡 API 接口列表

> 基础 URL：`https://aiflash.cc`

### 1. 获取新闻列表
```
GET /api/news
```

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|:---:|--------|------|
| section | string | 否 | `ai` | 板块代码（见下方板块表） |
| category | string | 否 | `""` | 分类筛选 |
| page | int | 否 | `1` | 页码 |
| page_size | int | 否 | `30` | 每页条数（最大500） |
| since | string | 否 | `""` | ISO时间格式，筛选该时间之后的数据 |
| all | bool | 否 | `false` | 设为true忽略72小时时间限制 |

**请求示例：**
```bash
curl "https://aiflash.cc/api/news?section=crypto&category=btc&page_size=10"
```

**响应格式：**
```json
{
  "total": 1523,
  "page": 1,
  "page_size": 10,
  "items": [
    {
      "id": 12345,
      "section": "crypto",
      "category": "btc",
      "title": "比特币突破10万美元创历史新高",
      "source_url": "https://...",
      "source_name": "CoinDesk",
      "summary": "...AI生成摘要...",
      "comment": "...AI点评...",
      "is_hot": true,
      "hot_reason": "阅读量激增",
      "likes": 42,
      "slug": "bitcoin-breaks-100k",
      "created_at": "2026-06-09T12:00:00+08:00",
      "votes": {"bullish": 15, "bearish": 3, "neutral": 5}
    }
  ]
}
```

---

### 2. 获取单条新闻详情
```
GET /api/news/{id}
GET /api/news/by-slug/{slug}
```

**请求示例：**
```bash
curl "https://aiflash.cc/api/news/12345"
curl "https://aiflash.cc/api/news/by-slug/bitcoin-breaks-100k"
```

---

### 3. 获取热门新闻
```
GET /api/news/hot
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| section | string | `ai` | 板块代码 |
| limit | int | `8` | 返回条数（最大20） |

**请求示例：**
```bash
curl "https://aiflash.cc/api/news/hot?section=crypto&limit=10"
```

---

### 4. 今日速览
```
GET /api/digest
```

每个板块取最新1条，一次看完全局动态。

```bash
curl "https://aiflash.cc/api/digest"
```

---

### 5. 搜索新闻
```
GET /api/search
```

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|:---:|--------|------|
| q | string | 是 | - | 搜索关键词 |
| section | string | 否 | `""` | 板块筛选 |
| page | int | 否 | `1` | 页码 |
| page_size | int | 否 | `20` | 每页条数（最大100） |

支持标题、摘要、AI点评三字段模糊匹配。

```bash
curl "https://aiflash.cc/api/search?q=ChatGPT&section=ai"
```

---

### 6. 投票
```
POST /api/vote
GET /api/votes/{news_id}
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|:---:|------|
| news_id | int | 是 | 新闻ID |
| vote_type | string | 是 | `bullish`（利好）/ `bearish`（利空）/ `neutral`（中立） |

IP每天同一条新闻只能投一次。

```bash
curl -X POST "https://aiflash.cc/api/vote?news_id=12345&vote_type=bullish"
```

---

### 7. 获取分类列表
```
GET /api/categories
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| section | string | `ai` | 板块代码 |

```bash
curl "https://aiflash.cc/api/categories?section=crypto"
```

---

### 8. 获取追踪人物列表
```
GET /api/entities
```

```bash
curl "https://aiflash.cc/api/entities"
```

---

### 9. 各板块新闻数量
```
GET /api/news/counts
```

返回各板块72小时内的新闻数量。

```bash
curl "https://aiflash.cc/api/news/counts"
```

---

### 10. 站点地图数据
```
GET /api/sitemap-news
```

返回最近30天的新闻ID和slug，供生成 sitemap 使用。

```bash
curl "https://aiflash.cc/api/sitemap-news"
```

---

### 11. 健康检查
```
GET /api/health
```

返回服务状态和各板块最新更新时间。

```bash
curl "https://aiflash.cc/api/health"
```

---

### 12. RSS Feed
```
GET /rss
```

返回最新30条新闻的 RSS 2.0 格式 Feed。

```
https://aiflash.cc/rss
```

---

## 📂 9大板块

| 板块 | 代码 | 图标 | 更新频率 | 分类 |
|------|:---:|:----:|:--------:|------|
| AI动态 | `ai` | 🤖 | 10分钟 | 论文、产品、行业、政策、教程 |
| 国际新闻 | `world` | 🌍 | 10分钟 | 政治、经济、科技、军事、社会 |
| 虚拟币 | `crypto` | ₿ | 10分钟 | 比特币、以太坊、山寨币、DeFi、NFT、监管、行情 |
| 财经股市 | `finance` | 📈 | 20分钟 | A股、美股、宏观、政策 |
| 科技数码 | `tech` | 📱 | 30分钟 | 手机、电动车、游戏、穿戴、评测 |
| 人物追踪 | `people` | 👤 | 60分钟 | 特朗普、马斯克、扎克伯格、Sam Altman、黄仁勋、Demis Hassabis、李飞飞、雷军、Satya Nadella、李开复 |
| 足球实况 | `football` | ⚽ | 10分钟 | 比赛、转会、积分、伤病、分析、🏆世界杯 |
| 娱乐八卦 | `entertainment` | 🎬 | 60分钟 | 电影、音乐、综艺、名人、动漫 |
| 地质灾害 | `disaster` | 🌋 | 60分钟 | 地震、洪水、滑坡、极端天气、国际 |

---

## 🏆 世界杯专题

2026美加墨世界杯专属接口，赛前搜前瞻/球队分析，赛中搜比分战报。

```bash
# 获取世界杯相关新闻
curl "https://aiflash.cc/api/news?section=football&category=worldcup&page_size=20"

# 访问世界杯专题页
# https://aiflash.cc/worldcup
```

---

## 📊 响应字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | int | 新闻唯一ID |
| section | string | 所属板块代码 |
| category | string | 所属分类 |
| title | string | 新闻标题 |
| source_url | string | 原文链接 |
| source_name | string | 来源名称 |
| summary | string | AI生成的新闻摘要 |
| comment | string | AI生成的点评分析 |
| is_hot | bool | 是否为热点新闻 |
| hot_reason | string | 热点原因 |
| likes | int | 点赞数 |
| slug | string | SEO友好URL标识 |
| created_at | string | 创建时间（UTC+8，ISO 8601格式） |
| votes | object | 投票统计 `{bullish, bearish, neutral}` |

---

## ⚠️ 使用限制

- 🆓 免费使用，无需注册
- 📊 单个请求最大500条（`/api/news`）或100条（`/api/search`）
- 🗳️ 同IP每天对同一条新闻只能投一次票
- ⏱️ 建议合理控制请求频率，避免对服务器造成压力

---

## 🛠️ 技术栈

- **后端**: Python FastAPI + SQLite + GPT-4o（VectorEngine）
- **前端**: Next.js 16 + TypeScript + Tailwind CSS v4
- **部署**: Nginx 反向代理 + Let's Encrypt SSL
- **服务器**: Ubuntu 24.04 LTS（新加坡）

---

## 📄 License

MIT License — 自由使用、修改、分发。

---

## 🔗 链接

- 🌐 官网：[https://aiflash.cc](https://aiflash.cc)
- 📖 API文档：[https://aiflash.cc/developers](https://aiflash.cc/developers)
- 📰 RSS：[https://aiflash.cc/rss](https://aiflash.cc/rss)
- 🏆 世界杯：[https://aiflash.cc/worldcup](https://aiflash.cc/worldcup)

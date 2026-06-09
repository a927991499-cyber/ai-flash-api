# ai-flash-api
免费 AI 新闻开放 API，覆盖 AI 动态、虚拟币、财经、科技、足球等 9 大板块实时数据，无需注册直接调用。
# AI Flash API — 免费 AI 新闻开放接口

覆盖 AI 动态、虚拟币、财经、科技、足球等 9 大板块的实时新闻 API。
无需注册，免费使用。

## 快速开始

```bash
curl https://aiflash.cc/api/news?section=ai&page_size=5
完整文档
👉 https://aiflash.cc/developers

特性
⚡ 实时更新，每 5-10 分钟自动采集
🤖 AI 自动生成摘要 + 点评
🔓 零认证，直接调用
📡 17000+ 条新闻数据
🏆 2026 世界杯专题数据
板块
ai / world / crypto / finance / people / football / tech / entertainment / disaster

接口速查
接口	说明
/api/news?section=ai	新闻列表
/api/news/{id}	单条详情
/api/search?q=关键词	全文搜索
/api/news/hot?section=ai	热门新闻
/api/digest	今日速览
/rss	RSS Feed
许可
MIT License

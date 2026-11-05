# 抖音 MCP：读评论区、热榜、星图和巨量算数

[English](README.md)

让 AI 读抖音公开内容：视频评论、热榜、账号最近发了什么，再加上星图达人数据和巨量算数指数。**不用你的抖音账号，不会牵连你的账号。只读公开内容，不发视频。**

SocialToAI 团队发布。服务是托管的，这里没有需要安装的东西。

## 它是做调研的，不是下载器

这个服务读数据和文字。**不下载视频，不去水印，不做语音转写。** 需要这些请用专门的工具。适合它的是下面这几件事。

## 接入

地址：`https://mcp.socialtoai.com/mcp`

```bash
# Claude Code：运行后输入 /mcp，选 socialtoai，登录
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp

# Codex：浏览器会打开登录页
codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp
```

Cursor、Trae、Cherry Studio、Dify 等见[接入页](https://socialtoai.com/zh/mcp/douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)。新账户送 100 积分，不用绑卡。

## 抖音的价格差得很大，这张表能省钱

2026-11-05 查询 [`/v1/capabilities`](https://api.socialtoai.com/v1/capabilities)，价格版本 `pv_29229c6acba5`。

| 计费项 | 读什么 | 积分 | 每页 |
| --- | --- | --- | --- |
| `douyin.comments` | 视频评论 | 2 | 约 20 条 |
| `douyin.trending` | 热搜、上升、话题、搜索词、音乐榜 | 2 | 约 50 条 |
| `douyin.trending:videos` | 视频榜（低粉爆款、高点赞率等） | 2 | 约 9 条 |
| `douyin.user_posts` | 账号的视频 | 2 | 约 20 条 |
| `douyin.detail` | 一条视频 | 2 | |
| `douyin.keyword_videos` | 巨量算数给出的关键词高指数视频 | 2 | 约 20 条 |
| `douyin.search` | **关键词搜视频** | **12** | 约 8 条 |
| `douyin.search_backup` | 关键词搜视频的备用路线，原路线失败时才用 | 20 | 约 8 条 |
| `douyin.kol_search` | 星图搜达人 | 3 | 约 20 条 |
| `douyin.kol_rates` | 星图达人报价 | 24 | |

所以找素材时先试 `douyin.keyword_videos` 或热榜，确实要全量搜索再用 `douyin.search`。100 赠送积分：纯搜索约 8 次，读评论约 50 页。

## 五个任务，以及真实花费

**1. 评论区在问什么**（真实录制，2026-10-18，50 积分，$0.17，[完整过程](https://socialtoai.com/zh/docs/guides/analyse-douyin-comments/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)）

> **问：** 抖音热门咖啡视频的评论区在问什么？
>
> **调用：** `comment_insights`，2 条视频，small，读到 36 条评论。
>
> **答案要点：** 能判断的 28 条里 11 条在提问，7 条想买。话题最多的是怎么买（25%，7 条），其次是把咖啡看成了大米（21%，6 条），然后是大家爱喝什么（18%，5 条）。样本小，适合找方向。

**2. 一个品牌在抖音上被怎么说**（真实录制，2026-10-18，60 积分，$0.20，[完整过程](https://socialtoai.com/zh/docs/guides/track-brand-mentions-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)）

> **调用：** `brand_buzz`，`brand: "瑞幸咖啡"`，近 30 天，small，7 条视频。
>
> **答案要点：** 7 条都相关，86% 正面、14% 中性。价格提得最多（43%，3 条）。7 条的结论只能算提示。

**3. 这周哪些视频跑出来了**（真实录制，2026-10-18，48 积分，$0.16，[完整过程](https://socialtoai.com/zh/docs/guides/find-viral-videos-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)）

> **调用：** `research` `viral_finder`，`query: "咖啡"`，近 7 天，small，14 条视频。
>
> **答案要点：** 14 条里 4 条至少是作者平时的 3 倍。跑出来最多的形式是教程（23%，3 条）。

**4. 找达人初筛**（真实录制，2026-10-18，9 积分，$0.03，[完整过程](https://socialtoai.com/zh/docs/guides/find-creators-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)）

> **调用：** `find_creators`，`query: "家居收纳"`，读到 20 位星图达人。
>
> **答案要点：** 按需求判断 17 位高度匹配、3 位部分匹配，9 位粉丝 10 万以上。合作和报价仍在星图确认，内容要标“广告”。

**5. 看一个词的趋势**（示例，下面的调用和积分来自价格表，不是录制）

> **问：** “露营”这个词最近 90 天的搜索指数怎么走？相关词有哪些？
>
> **调用：** `douyin.keyword_history`（4 积分）+ `douyin.keyword_related`（2 积分），共 6 积分。

## 限制

- **只读。** 不能发视频、评论、点赞、关注或私信。
- **只读公开内容。** 不碰登录后的内容和你的推荐流。
- **这些字段拿不到：** 视频详情和列表里的播放量、转发数、作者粉丝数（部分结果）都是 `null`，AI 会说“没有提供”。要当前播放量，用 `douyin.video_stats`（2 积分）单独查。
- **搜索排序：** 最新、最多赞和时间范围不一定能同时生效。要的是某个时间段，优先传时间范围。
- **评论者匿名。** 回答里不出现评论者的名字。
- 星图和巨量算数的数字来自平台工具，以读取当时为准，不是投放效果的保证。

## 链接

- [抖音 MCP 页面，含全部 35 个调用](https://socialtoai.com/zh/mcp/douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)
- [全部 18 个平台](https://github.com/socialtoai/socialtoai-mcp)
- 支持：support@socialtoai.com

本仓库的文字和示例采用 MIT 许可。SocialToAI 团队发布。

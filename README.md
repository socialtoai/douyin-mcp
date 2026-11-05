# Douyin MCP server: comments, hot lists, Xingtu and the Douyin index

[中文](README.zh.md)

Let your AI read public Douyin content: comment sections, hot lists and what an account posted, plus creator data from Xingtu and the Douyin keyword index. **Your Douyin account stays out of it. It reads public content and does not post.**

Published by the SocialToAI team. The server is hosted, so there is nothing here to install.

Reading from outside China? Ask in English, and have your AI search with Chinese keywords and translate the answer. Douyin is the Chinese app, separate from TikTok.

## It is for research, not downloading

It reads data and text. **It does not download videos, remove watermarks or transcribe speech.** Use a dedicated tool for those.

## Connect

Address: `https://mcp.socialtoai.com/mcp`

```bash
# Claude Code: run this, then type /mcp, pick socialtoai and sign in
claude mcp add --transport http socialtoai https://mcp.socialtoai.com/mcp

# Codex: the browser opens for sign-in
codex mcp add socialtoai --url https://mcp.socialtoai.com/mcp
```

Every other client: [connect page](https://socialtoai.com/mcp/douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106). New accounts get 100 free credits, no card.

## Prices vary a lot on Douyin

Checked on 2026-11-05 against [`/v1/capabilities`](https://api.socialtoai.com/v1/capabilities), price version `pv_29229c6acba5`.

| Price key | What it reads | Credits | Per page |
| --- | --- | --- | --- |
| `douyin.comments` | Video comments | 2 | about 20 |
| `douyin.trending` | Hot searches, rising topics, search terms, music chart | 2 | about 50 |
| `douyin.trending:videos` | Video boards (small-account breakouts, high like rate and more) | 2 | about 9 |
| `douyin.user_posts` | An account's videos | 2 | about 20 |
| `douyin.detail` | One video | 2 | |
| `douyin.keyword_videos` | High-index videos for a keyword, from the Douyin index | 2 | about 20 |
| `douyin.search` | **Keyword search for videos** | **12** | about 8 |
| `douyin.search_backup` | A backup route for that search, only when the first one fails | 20 | about 8 |
| `douyin.kol_search` | Creator search on Xingtu | 3 | about 20 |
| `douyin.kol_rates` | A creator's Xingtu rate card | 24 | |

Try `douyin.keyword_videos` or a hot list first, and use `douyin.search` when you need the wider search. 100 free credits is about 8 searches or about 50 comment pages.

## Recorded answers

All recorded on the live service on 2026-10-18; each link shows the whole run.

**What are people asking under trending coffee videos?** [50 credits, $0.17.](https://socialtoai.com/docs/guides/analyse-douyin-comments/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106) `comment_insights` on 2 videos read 36 comments. Of the 28 that could be judged, 11 ask a question and 7 want to buy. The top topics are how to buy it (25%, 7 comments) and mistaking it for rice (21%, 6).

**What are people saying about Luckin Coffee?** [60 credits, $0.20.](https://socialtoai.com/docs/guides/track-brand-mentions-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106) `brand_buzz`, 30 days, small: all 7 videos relevant, 86% positive and 14% neutral, with price mentioned most (43%, 3 videos). Seven videos is a hint, not a measurement.

**Which coffee videos beat their author's usual this week?** [48 credits, $0.16.](https://socialtoai.com/docs/guides/find-viral-videos-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106) `research` `viral_finder`, 7 days, small: 14 videos, 4 of them at least 3 times the usual, with how-to the format that took off most (23%, 3 videos).

**Who could we work with on home organisation?** [9 credits, $0.03.](https://socialtoai.com/docs/guides/find-creators-on-douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106) `find_creators` read 20 Xingtu creators: 17 high fit, 3 partial, 9 above 100K followers. Rates and contracts stay on Xingtu.

**Example, not a recording:** "How has the search index for 露营 moved, and which terms are related?" → `douyin.keyword_history` (4 credits) + `douyin.keyword_related` (2 credits), 6 credits in all by the price table.

## Limits

- **Read only.** No posting, comments, likes, follows or messages.
- **Public content only.** Nothing behind a login, and no feed of your own.
- **Fields it cannot give:** plays and reposts in video details and lists, and author followers in some results, are `null`. For current plays, call `douyin.video_stats` (2 credits).
- **Search sorting:** newest, most-liked and a time window may not apply together. If the period matters most, send the period.
- **Commenters are anonymous** in every answer.
- Xingtu and index figures are the platform tools' numbers at the time they were read, not a promise of campaign results.

## Links

- [Douyin MCP page, with all 35 calls](https://socialtoai.com/mcp/douyin/?utm_source=github&utm_medium=repo&utm_campaign=douyin-mcp-20261106)
- [All 18 platforms](https://github.com/socialtoai/socialtoai-mcp)
- Support: support@socialtoai.com

MIT licence for the text and examples in this repository. Published by the SocialToAI team.

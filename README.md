# X 爆款推文素材库

> 把 X（Twitter）上的信息流，变成 Obsidian 里的长期资产。
>
> 自动筛选 | 批量采集 | 智能分类 | 元数据完整 | Codex Skill

---

## 这个仓库是什么

X 爆款推文素材库是一个 Codex Skill，用来指导 AI Agent 自动从 X（Twitter）采集高表现推文，并同步到 Obsidian 知识库。

它不是简单的爬虫工具，也不是通用的内容收藏器。它的核心目标是：**先理解你的研究目的，再按浏览量智能筛选，把碎片化的推文变成结构化的素材资产。**

特别适合：

- 内容创作者做对标分析
- 独立开发者研究产品增长
- AI 创业者跟踪行业动态
- 运营人员建立竞品库
- 研究人员积累长期资料

一句话：**让你不再手动翻推特，而是把真正有价值的内容，自动沉淀进自己的知识库。**

---

## 适合谁用

### 特别适合

- **内容创作者** — 分析爆款选题、研究标题结构、拆解表达方式、积累创作灵感
- **独立开发者** — 研究海外产品发布、分析产品增长案例、收集 Launch 内容、积累运营素材
- **AI 创业者** — 跟踪头部 AI 博主、收集产品案例、整理市场趋势、沉淀行业知识库
- **运营人员** — 建立竞品内容库、监控行业动态、分析高互动内容
- **用 Codex 做内容生产的人** — 希望稳定复用一套工作流、自动化重复劳动

### 不适合

- 想要通用爬虫、数据分析、制作数据表的人
- 想要储存所有推文、不做任何筛选的人
- 想要实时监控、推送通知的人
- 不用 Obsidian 的人

---

## 它会产出什么

### 默认输出

- 按浏览量筛选的推文列表（支持 10万+、50万+、100万+ 等多档位）
- 多个博主批量采集和分类
- 自动创建 Obsidian 文件夹结构（按博主名 → 按主题分类）
- 每篇推文的完整元数据：

```
tweet_id: 
views: 
likes: 
retweets: 
author: 
handle: 
publish_time: 
tweet_url:
```

- 最终 Markdown 文件，保存到 Obsidian 指定目录

### 默认不输出

- 所有推文无差别采集
- 实时推送或通知机制
- 数据可视化或统计报表
- 推文内容的本地图片缓存

---

## 核心能力

### 1. 浏览量智能筛选

支持多档位浏览量设定：
- 10万+
- 50万+
- 100万+

**为什么重要？** 只保留被市场验证过的内容，避免浪费时间在没人看的推文上。

### 2. 多博主批量采集

一次性处理多个博主：
```
https://x.com/levelsio
https://x.com/dannypostmaa
https://x.com/paulg
```

支持单次采集 20-100+ 篇推文。

### 3. 自动创建 Obsidian 文件夹结构

智能分类，无需手动整理：

```
📁 Levelsio
 ├── AI Startup Ideas.md
 ├── Build In Public.md
 ├── SaaS 增长案例.md

📁 Danny Postma
 ├── Growth Hack.md
 ├── Product Launch.md
```

### 4. 完整元数据记录

每条推文自动附加：
- 推文 ID、浏览量、点赞、转发数
- 作者名和 handle
- 发布时间、推文链接

**为什么重要？** 方便后续检索、分析、溯源。

### 5. 长期素材库建设

持续积累高质量内容资产：

```
Obsidian
 ├── AI
 │   ├── Startup
 │   ├── 工具类
 │   └── 融资新闻
 ├── SaaS
 │   ├── 增长
 │   └── 变现
 ├── Marketing
 ├── Indie Hacker
 ├── Growth
 └── Startup
```

---

## 示例效果

### 爆款推文库示例
![爆款推文库示例](examples/images/01-viral-tweets-example.png)

### 多博主分类示例
![多博主分类示例](examples/images/02-multi-author-organization.png)

### Obsidian 文件结构示例
![Obsidian 文件结构示例](examples/images/03-obsidian-structure.png)

### 推文元数据示例
![推文元数据示例](examples/images/04-tweet-metadata.png)

### 长期素材库示例
![长期素材库示例](examples/images/05-long-term-library.png)

这些是工作流程示意图，展示最终 Obsidian 中的实际效果。

---

## 安装

克隆仓库：

```bash
git clone https://github.com/hemoouren/X-to-Obsidian-SKill.git
cd X-to-Obsidian-SKill
```

复制 skill 到 Codex skills 目录：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R ./X-to-Obsidian-SKill "${CODEX_HOME:-$HOME/.codex}/skills/"
```

安装后，在 Codex 里使用：

```text
Use $X-to-Obsidian-SKill 抓取这些博主的爆款推文，同步到 Obsidian
```

---

## 怎么用

### 抓取单个博主

```text
Use $X-to-Obsidian-SKill 

抓取这个博主最近浏览量超过10万的推文

https://x.com/levelsio

保存前20篇到 Obsidian
```

### 指定保存数量

```text
Use $X-to-Obsidian-SKill 

抓取这个博主浏览量超过10万的推文

https://x.com/dannypostmaa

保存前30篇，自动分类
```

### 多个博主批量采集

```text
Use $X-to-Obsidian-SKill 

抓取以下博主浏览量超过20万的内容：

https://x.com/levelsio
https://x.com/dannypostmaa
https://x.com/paulg

按主题自动分类保存
```

### 构建行业素材库

```text
Use $X-to-Obsidian-SKill 

抓取这些 AI 博主浏览量超过50万的内容：

https://x.com/levelsio
https://x.com/lex_fridman
https://x.com/ylecun

为每个博主保存20篇，建立 AI 行业素材库
```

### 只做采集规划

```text
Use $X-to-Obsidian-SKill 

先不要生成。请分析下面这些博主适合采集哪些主题，
输出推荐的浏览量筛选档位和文件夹分类结构。

https://x.com/levelsio
https://x.com/dannypostmaa
https://x.com/paulg
```

更多示例见 [examples/prompts.md](examples/prompts.md)。

---

## 工作流程

这个 skill 的流程是：

1. 读取一个或多个 X 博主主页链接
2. 扫描博主近期推文
3. 按浏览量智能筛选（10万+、50万+、100万+ 等档位）
4. 提炼推文的核心主题和标签
5. 为每条推文记录完整元数据
6. 创建或更新 Obsidian 文件夹结构
7. 生成 Markdown 文件并同步到 Obsidian
8. 按采集日期、博主、主题建立索引
9. 输出采集结果报告（数量、分类、保存路径）

---

## 实际应用场景

### 场景 1：找爆款选题

建立：**爆款推文库**

研究：
- 标题怎么写
- 开头怎么吸引
- 整体结构
- 最后的 CTA

```text
Use $X-to-Obsidian-SKill 

抓取浏览量超过50万的推文，建立爆款推文库。
按主题自动分类，方便后续拆解。
```

### 场景 2：做内容对标

建立：**行业博主数据库**

长期跟踪：
- 他们发什么
- 怎么发的
- 什么内容最火

```text
Use $X-to-Obsidian-SKill 

持续跟踪这些头部内容创作者，
浏览量超过20万的推文都保存下来。
```

### 场景 3：产品增长研究

建立：**Growth Library**

沉淀：
- 增长案例
- 用户反馈
- 产品发布策略

```text
Use $X-to-Obsidian-SKill 

抓取这些 SaaS 创始人的推文，
重点关注融资、增长、用户反馈相关内容。
```

### 场景 4：跟踪行业动态

建立：**AI 行业知识库**

持续积累：
- 新产品发布
- 融资新闻
- 技术突破
- 市场评论

```text
Use $X-to-Obsidian-SKill 

定期抓取头部 AI 博主的内容，
浏览量超过30万，建立 AI 行业观察库。
```

---

## 为什么做这个 Skill

**问题：** 很多人收藏了几千条推文，但真正需要的时候根本找不到。

**根因：** 真正有价值的不是**收藏**本身，而是：

```
筛选 → 归档 → 分类 → 沉淀
```

**解决方案：** 这个 Skill 帮你把 X 上的高价值内容，自动沉淀进自己的知识库。

不只是保存碎片信息，而是：
- 智能过滤（按浏览量）
- 自动分类（按主题）
- 结构化记录（完整元数据）
- 长期积累（可查可复用）

**最终结果：** 把推特信息流，变成自己的资产。

---

## 注意事项

- X API 速率限制：每个博主最多扫描最近 3200 条推文
- Obsidian Web Clipper 需要提前配置和启动
- 浏览量数据来自 X 公开接口，实时性可能有 1-2 小时延迟
- 推文被删除后无法重新采集
- 推荐定期（周/月）运行，建立动态更新的素材库，而不是一次性全量采集
- 每次采集前确认 Obsidian 目标文件夹已创建

---

## 相关项目

- [Ian Xiaohei Illustrations](https://github.com/helloianneo/ian-xiaohei-illustrations) — 中文正文配图 Skill
- [Awesome Claude Code Skills](https://github.com/helloianneo/awesome-claude-code-skills) — Claude Code Skills 精选合集

---

## 目录结构

```text
.
├── README.md
├── LICENSE
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
│   └── fetch_tweets.py
├── examples/
│   ├── images/
│   │   ├── 01-viral-tweets-example.png
│   │   ├── 02-multi-author-organization.png
│   │   ├── 03-obsidian-structure.png
│   │   ├── 04-tweet-metadata.png
│   │   └── 05-long-term-library.png
│   └── prompts.md
└── references/
    ├── obsidian-setup.md
    ├── x-api-guide.md
    ├── workflow-best-practices.md
    └── qa-checklist.md
```

真正需要安装到 Codex 的是：

```text
agents/
scripts/
references/
SKILL.md
```

根目录的 README、LICENSE 和 examples 是 GitHub 分享文档。

---

## License

MIT License. See [LICENSE](LICENSE).

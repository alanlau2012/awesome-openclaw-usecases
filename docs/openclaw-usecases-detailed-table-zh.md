# Awesome OpenClaw Use Cases — 详细对照表（中文）

> 来源仓库：[awesome-openclaw-usecases](https://github.com/alanlau2012/awesome-openclaw-usecases)  
> 用例数量：42  
> 生成日期：2026-06-07

---

## 一、社交媒体（5）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **Daily Reddit Digest** | 定时抓取指定 subreddit 的热门/新帖，只读浏览、拉评论上下文，生成 digest；通过记忆学习你的偏好（如过滤 meme） | 安装 `reddit-readonly`（无需 Reddit 登录）→ 列出 subreddit → 设定 cron（如每天 17:00）→ Prompt 要求建立 Reddit 专用 memory，每日问你是否满意并更新规则 |
| **Daily YouTube Digest** | 追踪关注频道或关键词新视频，拉 transcript，输出标题+链接+2–3 条要点；用 `seen-videos.txt` 去重 | 安装 `youtube-full` → 方案 A：频道列表+晨间 cron；方案 B：关键词搜索+去重文件 → 把频道列表写入 memory 便于迭代 |
| **X Account Analysis** | 用 Bird skill 拉取你的推文，做定性分析：什么内容火、什么话题表现差、改进建议（替代付费分析工具） | 安装 Bird → 用 x.com cookie 授权 → Prompt：「拉最近 N 条推文，分析高/低互动模式」→ 可定制分析问题 |
| **Multi-Source Tech News Digest** | 四层信息源（RSS 46 / X KOL 44 / GitHub Release 19 / Brave 搜索）合并去重、质量打分，推送到 Discord/Email/Telegram | 安装 `tech-news-digest` → 配置可选 API Key → Prompt 设定每日 9:00 推送 → 用自然语言增删 RSS/Twitter/GitHub 源（约 30 秒） |
| **X/Twitter Automation** | 通过 TweetClaw 插件在聊天里完成发推、回复、点赞、转推、关注、DM、搜索、数据导出、抽奖、账号监控 | `openclaw plugins install @xquik/tweetclaw` → 用自然语言描述操作（如「从这条推文抽奖，粉丝>1000」）→ 定制监控账号与通知渠道 |

---

## 二、创意与构建（6）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **Goal-Driven Autonomous Tasks**（Overnight Mini-App Builder） | 一次性 brain dump 目标；Agent 每日自动生成 4–5 个自主任务并执行（研究、脚本、竞品、惊喜 mini-app）；可选自建 Kanban 追踪 | 设定 Telegram/Discord → 第一步写目标 → 第二步 8:00 cron 生成+执行任务 → 用 `AUTONOMOUS.md`（目标）+ `tasks-log.md`（只追加日志）避免子 Agent 写冲突 |
| **YouTube Content Pipeline** | 每小时扫 AI 新闻 + X，对照 YouTube Analytics 90 天目录，SQLite 语义去重 pitch，新点子推 Telegram；Slack 链接触发深度调研并建 Asana 卡片 | 配置 Telegram topic + KB + x-research → 建 SQLite `pitches` 表 → hourly cron + Slack 触发 Prompt → 按你的 niche 改关键词和 PM 工具 |
| **Multi-Agent Content Factory** | Discord 多频道流水线：Research（找机会）→ Writing（脚本/推文）→ Thumbnail（封面图），定时产出完整素材 | 建 #research / #scripts / #thumbnails → 用 `sessions_spawn` 链式调用 → 定制平台（X thread / Newsletter / LinkedIn）和图片生成后端 |
| **Autonomous Game Dev Pipeline** | 教育类 HTML5 游戏全自动开发：**Bugs First**（先修 bug 再做新功能）、轮询 backlog、写代码、更新 registry、文档、git commit | 定义 `bugs/`、`development-queue.md`、`game-design-rules.md` → 粘贴 system prompt（bugs-first、分支、注册、changelog）→ 按语言/年龄段定制设计规则 |
| **Podcast Production Pipeline** | 录前：嘉宾/话题调研、提纲、5–7 个问题；录后：带时间戳 show notes、SEO 描述、社媒推广包；可选竞品 RSS 监控 | 录前 Prompt → 存 `~/podcast/episodes/[n]/prep/`；贴 transcript 后 Prompt → 存 `publish/` → 可与 Content Factory 联动做二次分发 |
| **AI Video Editing via Chat** | 自然语言剪辑：裁剪/合并、BGM+ducking、50+ 语言字幕、调色、竖屏裁剪、批量处理；无需时间线 GUI | 安装 `video-editor-ai` + `ai-subtitle-generator` → 丢文件+描述需求；或对整个文件夹 batch → 敏感素材注意服务商隐私政策 |

---

## 三、基础设施与 DevOps（2）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **n8n Workflow Orchestration** | 代理模式：OpenClaw 只调 n8n webhook，凭证留在 n8n；集成可视化、可锁定、可加审批/限流 | Docker 起 n8n 或 `openclaw-n8n-stack` → AGENTS.md 写规则：密钥不进 Agent、workflow 命名 `openclaw-{service}-{action}` → 先 build→test→lock |
| **Self-Healing Home Server** | 常驻基础设施 Agent：SSH 管理 homelab、cron 健康检查、自愈（重启 pod/修配置）、Terraform/Ansible/K8s、晨间简报、Obsidian 知识沉淀 | 写 AGENTS.md 人设+权限 → HEARTBEAT.md 设 15min–weekly cron → 安全清单（pre-push hook、禁止直推 main）→ 按你的机器/监控栈定制 |

---

## 四、生产力（20）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **Autonomous Project Management** | 去中心化 PM：子 Agent 通过共享 `STATE.yaml`（任务/负责人/阻塞/下一步）并行协作，主会话只做「CEO」 | AGENTS.md 定义 PM 委派模式 → 新项目 spawn PM sub-agent → PM 更新 STATE.yaml 并可再 spawn 子任务 → schema 与 label 约定可自定义 |
| **Multi-Channel AI Customer Service** | 统一收件箱：WhatsApp/Instagram/Gmail/Google 评论，AI 自动回复+人工接管+测试模式+业务 KB | 接各渠道 API → 建 KB（服务/营业时间/FAQ/升级规则）→ AGENTS.md 路由（意图分类、语言匹配）→ 30min heartbeat 查队列积压 |
| **Phone-Based Personal Assistant** | ClawdTalk + Telnyx：任意手机打电话/SMS 访问 OpenClaw，免提查日历、Jira、网页搜索 | 配置 ClawdTalk → Prompt 设定问候语+语音可访问的集成列表 → 适合开车/双手占用场景 |
| **Inbox De-clutter** | 每日 cron 读过去 24h newsletter，摘要+链接，收集反馈并更新 memory；可专用 Gmail 收 newsletter | 安装 Gmail OAuth → 可选专用邮箱集中订阅 → 8pm cron digest Prompt → 定制摘要长度和主题过滤 |
| **Personal CRM** | 6:00 扫 Gmail/Calendar 更新 SQLite 联系人；NL 查询「我对 X 了解什么」；7:00 外部会议 prep 简报 | gog CLI + SQLite → Telegram topic `personal-crm` → 每日 scan + briefing Prompt → 自定义字段和扫描范围 |
| **Health & Symptom Tracker** | Telegram topic 记录饮食/症状到 `health-log.md`；3 次用餐提醒；周日做食物-症状关联分析 | 建 topic + log 文件 → logging/reminder/weekly analysis 三段 Prompt → 定制提醒时间和追踪项 |
| **Multi-Channel Personal Assistant** | 单 Assistant 路由 Telegram 多 topic、Slack、Google Workspace、Todoist/Asana；含垃圾日等生活提醒 | 配置 topic 与 OAuth → routing Prompt 映射意图到工具 → 先测单集成再测跨工作流 |
| **Project State Management** | 事件驱动替代 Kanban：对话更新写入 Postgres/SQLite 全历史；9:00 standup 从 git+事件生成；支持 sprint 规划 | 建 `projects/events/blockers` 表 → Discord 频道 → 对话式 event logging + standup Prompt |
| **Dynamic Dashboard** | 每 N 分钟并行 sub-agent 拉 GitHub/X/Polymarket/系统指标，聚合 Discord/HTML/Canvas，超阈值告警 | 建 metrics/alerts 表 → 15min cron 并行 fetch Prompt → 定制数据源和告警阈值 |
| **Todoist Task Manager** | 把 Agent 推理外化到 Todoist：任务含完整 PLAN、子步骤用 comment 记录进度、heartbeat 检测 stalled | 让 Agent 自建 `todoist_api.sh` 等脚本 → 设定 In Progress/Waiting/Done 分区 → 长任务强制 Todoist 可见 |
| **Family Calendar & Household Assistant** | 晨间聚合多日历；监控 iMessage 自动建事件（含车程 buffer）；`inventory.json` pantry OCR+购物清单；伴侣共享 Telegram | 日历聚合 Prompt → HEARTBEAT 15min 消息监控 → pantry Prompt；**建议先只读**再开写入 |
| **Multi-Agent Specialized Team** | 一人公司虚拟团队：策略/商业/营销/开发各 Agent，独立 SOUL.md+模型，共享 `GOALS.md`/`DECISIONS.md`，Telegram @tag 路由 | 建 `team/` 目录结构 → AGENTS.md @tag→session 路由 → HEARTBEAT 各 Agent 日程 → **先从 2 个 Agent 起步** |
| **OpenClaw as Desktop Cowork（AionUi）** | 桌面统一 UI 跑 OpenClaw+12 Agent，MCP 配一次；WebUI/Telegram/飞书/钉钉远程；内置部署专家可远程修 gateway | 装 AionUi → `openclaw onboard` → Cowork 会话 → 远程渠道作「救援通道」 |
| **Custom Morning Brief** | 定时（如 8:00）推送：兴趣新闻、**完整内容草稿**（非仅标题）、今日任务、Agent 可自主完成的任务建议 | 接消息渠道+任务管理器 → 四段式 morning brief Prompt → 聊天迭代（「加天气」「只看 AI」） |
| **Automated Meeting Notes & Action Items** | transcript→结构化摘要+action items（负责人/截止）→ 自动建 Jira/Linear/Todoist/Notion 任务→推 Slack | **先粘贴 transcript** 验证 → 再开文件夹监听/Otter API → 定制 assignee 映射和摘要模板 |
| **Habit Tracker & Accountability Coach** | Telegram/SMS 定时 proactive 打卡；streak；语气随进度变化；2h 无回复跟进；周日周报 | 定义 3–5 个习惯+时间 → tone 规则+3 天缺席升级 → `~/habits/log.json` → 可选 Google Sheets 看板 |
| **Second Brain** | 随手 texto 到 Telegram/iMessage 进 memory；自建 Next.js 仪表盘 Cmd+K 搜索，无文件夹/tag 负担 | 先开始 capture → 再 Prompt 建 Next.js UI → 定制捕获渠道和筛选字段 |
| **Event Guest Confirmation** | SuperCall 逐个外呼嘉宾确认出席、饮食/携伴；沙箱 persona（无 gateway 权限防注入）；汇总 confirmed/declined/no-answer | 装 SuperCall+Twilio+ngrok → 嘉宾名单+事件详情 Prompt → **先测 2–3 人**再批量；查 `supercall-logs` |
| **Phone Call Notifications** | 超 chat 阈值时通过 clawr.ing 外呼：晨间简报、股价异动、紧急邮件；双向对话；免 Twilio 自建 | 粘贴 clawr.ing setup Prompt → 定义「值得打电话 vs 只发消息」规则 → 配 heartbeat/cron；设阈值防骚扰 |
| **Local CRM Framework（DenchClaw）** | `npx denchclaw` 本地 CRM：DuckDB、localhost:3100 多视图 UI、浏览器自动化、NL 查询、CSV 导入、LinkedIn enrichment | 一行安装 → onboarding → NL 建对象/字段/视图 → 文件系统优先，Agent 可直接改数据；Telegram 可远程用 |

---

## 五、研究与学习（8）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **AI Earnings Tracker** | 周日预览本周 tech/AI 财报；你选公司；各财报后 one-shot cron 发 beat/miss、营收、EPS、AI 亮点 | Telegram topic `earnings` → 周日 preview+确认 → one-shot cron+摘要格式 Prompt → memory 记住常看名单 |
| **Personal Knowledge Base (RAG)** | 丢 URL/推文/YouTube/PDF 到 chat 入库；语义搜索；供视频 pipeline、会议 prep 等 workflow 复用 | 安装 `knowledge-base` → 建专用 topic → ingest+query Prompt → 指定哪些 workflow 自动 query KB |
| **Market Research & Product Factory** | Last 30 Days skill 挖 Reddit/X 痛点 → 排序机会 → OpenClaw 按痛点建 MVP web app；可周一 cron 扫 niche | 装 Last 30 Days → research Prompt（结构化输出）→ 「为 [痛点] 建 MVP，只要核心功能」 |
| **Pre-Build Idea Validator** | 编码前 `idea_check` 扫 GitHub/HN/npm/PyPI/PH → `reality_signal` 0–100；>70 停做，30–70 pivot，<30 Proceed | 配 `idea-reality-mcp`（`uvx idea-reality-mcp`）→ Agent 指令设阈值 → 大决策用 `depth="deep"` |
| **Semantic Memory Search** | memsearch 把 OpenClaw markdown memory 索引进 Milvus，混合向量+BM25，SHA-256 去重，watch 自动同步 | `pip install memsearch` → `index ~/memory/` → `search "query"` → 可选 `memsearch[local]` 无 API Key |
| **arXiv Paper Reader** | 按 ID 拉论文、列章节、比 abstract、分段 summarize/critique；本地缓存 | 装 arxiv-reader skill → workflow：先看 abstract，需要再全文；多 ID 对比；维护 reading list |
| **LaTeX Paper Writing** | 对话写 LaTeX，容器内即时 PDF 编译（article/IEEE/beamer/中文）；BibTeX；从 log 自动修错 | Prismer Docker 8080 → latex-compiler skill → Prompt：选模板、每次改完 compile、CJK 用 xelatex |
| **HF Papers Research Discovery** | 每日 HF trending（按 upvote）、关键词搜索、metadata+评论；deep-read 走 arxiv-source | `hf-papers` + `arxiv-source` → 每日 top-10 → triage → 选中 deep-read → 维护已读列表 |

---

## 六、金融与交易（1）

| 场景 Case | 具体做什么 | 怎么借鉴使用 |
|-----------|------------|--------------|
| **Polymarket Autopilot** | **仅模拟盘**：15min cron 拉 Polymarket 数据，跑 TAIL/BONDING/SPREAD 策略，$10k 虚拟组合；8:00 Discord 日报 P&L/胜率 | 建 `paper_trades`/`portfolio` 表 → Discord 频道 → autopilot Prompt 设策略阈值 → 文档明确 **禁止真钱** |

---

## 七、跨案例可复用的 6 种模式

| 模式 | 适用场景 | 借鉴要点 |
|------|----------|----------|
| **Cron + Memory 反馈** | Reddit/YouTube/收件箱/习惯/财报 | 定时跑 + 每日问满意度 → 规则写入 memory，越用越准 |
| **Topic/频道隔离** | CRM、KB、视频点子、earnings | 一个 Telegram topic 或 Discord 频道 = 一个 workflow，减少上下文污染 |
| **共享状态文件** | 多 Agent 协作 | `STATE.yaml` / `DECISIONS.md` / 只追加 `tasks-log.md`，避免并发写同一文件 |
| **链式 vs 并行 Agent** | Content Factory（顺序）vs Dashboard/游戏开发（并行） | 有依赖用 chain；独立数据源用 parallel sub-agents |
| **凭证隔离** | n8n webhook、SuperCall 沙箱、DenchClaw 本地、TweetClaw 托管 API | 敏感 key 不进 Agent 上下文 |
| **先简后繁** | 会议纪要、家庭日历、活动外呼 | 先手动 paste/只读/测 2–3 人，验证后再全自动化 |

---

## 八、安全提醒

- 多数 Skill/插件为社区构建，**未经列表维护者审计**。
- 使用前请：**读 Skill 源码、检查权限、勿硬编码 API Key**。
- 仓库**不接受 crypto 相关用例**；Polymarket 案例明确为 paper trading。

---

## 九、各用例原文链接

| # | 用例 | 文件 |
|---|------|------|
| 1 | Daily Reddit Digest | usecases/daily-reddit-digest.md |
| 2 | Daily YouTube Digest | usecases/daily-youtube-digest.md |
| 3 | X Account Analysis | usecases/x-account-analysis.md |
| 4 | Multi-Source Tech News Digest | usecases/multi-source-tech-news-digest.md |
| 5 | X/Twitter Automation | usecases/x-twitter-automation.md |
| 6 | Goal-Driven Autonomous Tasks | usecases/overnight-mini-app-builder.md |
| 7 | YouTube Content Pipeline | usecases/youtube-content-pipeline.md |
| 8 | Multi-Agent Content Factory | usecases/content-factory.md |
| 9 | Autonomous Game Dev Pipeline | usecases/autonomous-game-dev-pipeline.md |
| 10 | Podcast Production Pipeline | usecases/podcast-production-pipeline.md |
| 11 | AI Video Editing via Chat | usecases/ai-video-editing.md |
| 12 | n8n Workflow Orchestration | usecases/n8n-workflow-orchestration.md |
| 13 | Self-Healing Home Server | usecases/self-healing-home-server.md |
| 14 | Autonomous Project Management | usecases/autonomous-project-management.md |
| 15 | Multi-Channel AI Customer Service | usecases/multi-channel-customer-service.md |
| 16 | Phone-Based Personal Assistant | usecases/phone-based-personal-assistant.md |
| 17 | Inbox De-clutter | usecases/inbox-declutter.md |
| 18 | Personal CRM | usecases/personal-crm.md |
| 19 | Health & Symptom Tracker | usecases/health-symptom-tracker.md |
| 20 | Multi-Channel Personal Assistant | usecases/multi-channel-assistant.md |
| 21 | Project State Management | usecases/project-state-management.md |
| 22 | Dynamic Dashboard | usecases/dynamic-dashboard.md |
| 23 | Todoist Task Manager | usecases/todoist-task-manager.md |
| 24 | Family Calendar & Household Assistant | usecases/family-calendar-household-assistant.md |
| 25 | Multi-Agent Specialized Team | usecases/multi-agent-team.md |
| 26 | OpenClaw as Desktop Cowork (AionUi) | usecases/aionui-cowork-desktop.md |
| 27 | Custom Morning Brief | usecases/custom-morning-brief.md |
| 28 | Automated Meeting Notes & Action Items | usecases/meeting-notes-action-items.md |
| 29 | Habit Tracker & Accountability Coach | usecases/habit-tracker-accountability-coach.md |
| 30 | Second Brain | usecases/second-brain.md |
| 31 | Event Guest Confirmation | usecases/event-guest-confirmation.md |
| 32 | Phone Call Notifications | usecases/phone-call-notifications.md |
| 33 | Local CRM Framework (DenchClaw) | usecases/local-crm-framework.md |
| 34 | AI Earnings Tracker | usecases/earnings-tracker.md |
| 35 | Personal Knowledge Base (RAG) | usecases/knowledge-base-rag.md |
| 36 | Market Research & Product Factory | usecases/market-research-product-factory.md |
| 37 | Pre-Build Idea Validator | usecases/pre-build-idea-validator.md |
| 38 | Semantic Memory Search | usecases/semantic-memory-search.md |
| 39 | arXiv Paper Reader | usecases/arxiv-paper-reader.md |
| 40 | LaTeX Paper Writing | usecases/latex-paper-writing.md |
| 41 | HF Papers Research Discovery | usecases/hf-papers-research-discovery.md |
| 42 | Polymarket Autopilot | usecases/polymarket-autopilot.md |

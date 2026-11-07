# 现状评估：`Linux2010` 个人主页审计

> 审计日期：2026-11-02 · 全部数字为当日通过 `gh api` / GitHub 页面实测抓取，非估算
> 基准：[github-profile-research.md](./github-profile-research.md) 的 6 条准则 · 方案见 [github-profile-optimization-plan.md](./github-profile-optimization-plan.md)

---

## 1. 账号事实底账（审计依据）

### 1.1 档案区元数据（`GET /users/Linux2010`）

| 字段 | 当前值 | 问题 |
|---|---|---|
| `name` | `Andy Tien` | 与 README 自称 `Hope` **不一致**（同名主页上出现两个名字） |
| `bio` | `Java \| Python \| Bigdata \| Data Warehouse \| Data Analysis \| AI` | **严重过期**：与 README 的定位"AI 自动化与智能体生态建设者"直接冲突；主页上 bio 紧贴头像、优先级高于 README |
| `company` | `antgroup`（无组织关联，纯文本） | 未加入 GitHub Organization，因此不渲染成可点击的 org 卡片（`/users/Linux2010/orgs` 返回 200 但长度为 **0** = 无任何公开组织成员身份） |
| 组织成员身份 | 你是 `orion-agents` 的 **`role=admin, state=active`** 成员，但该 org 的 `public_members` 列表为**空** → **成员身份未 publicize** | 🔴 主页完全不显示你亲手创建的组织的 logo —— 见 P0-4 |
| `blog` | API 返回**空**（REST `blog=""`、GraphQL `websiteUrl: null`）—— **但 2026-11-03 浏览器实访发现侧栏公开显示 `https://blog.csdn.net/UncleTian`** | 两处字段读数与线上渲染不一致；以线上为准：站点其实已有，只是 API 取不到。README 原本漏了它，已补入（见 §4） |
| `twitter_username` | `null` | X 用户名未绑定 → 头像下方不显示社交图标 |
| `hireable` | `null`（未开启） | 与 README "当前在学习" 的求职/合作意图不匹配 |
| `location` | `Shanghai China` | OK |
| `followers / following` | **25 / 47** | 影响力外部背书薄弱，因此更要靠"可验证产出"而非粉丝数 |
| `public_repos` | 172 | 见 1.2，结构失衡 |
| `created_at` | 2018-01-06 | 8 年账号，资历可用 |

### 1.2 仓库构成（172 个公开仓库全量统计）

```
fork 仓库   : 118  (68.6%)
自建仓库    :  54  (31.4%)
全账号获得星数合计: 21★   （第 1 页 10★ + 第 2 页 11★）
```

星数最高的 5 个仓库（混列 fork，用以说明真实构成）：

| 仓库 | ★ | 自建? | 语言 | 最后 push | description |
|---|---|---|---|---|---|
| `ai-wiki` | 2 | ✅ 自建 | – | 2026-08-14 | **（空）** |
| `mall` | 1 | ❌ fork（父仓 `5kywa1ker/mall`） | – | 2019-03-21 | `基于SpringBoot的网上商城` |
| `hermes-agent` | 1 | ❌ fork（父仓 `NousResearch/hermes-agent`） | Python | 2026-07-27 | `The agent that grows with you` —— **与上游同名，极易误读** |
| `codex` | 1 | ❌ fork（父仓 `openai/codex`） | Rust | 2026-09-09 | `Lightweight coding agent that runs in your terminal` —— **与上游同名** |
| `awesome-coding-plan` | 1 | ✅ 自建 | JavaScript | 2026-07-14 | `awesome-coding-plan`（同义反复，无信息） |

> ⚠️ 逐仓核实后的更硬结论：**全账号 172 仓中，自建且星数 ≥1 的只有 2 个**（`ai-wiki` 2★、`awesome-coding-plan` 1★）。星数排名第 2–4 位的项目全部是**别人的 fork**，其中 `hermes-agent`、`codex` 与上游同名 —— 读者从主页档案区进入时，很可能把它们当成本人作品，而一旦发现是 fork，可信度反而倒扣。这是 P1-4 的直接证据。
| `itemCF` / `item_CF` / `hive-report` / `hdfs-operation` / `hadoop-doc` / `bigram` / `ITF_MR` / `email-ml` / `hadoop-hbase-doc` / `bigdata-code-report` … | 0 | Scala/Java/– | 全部 **2026-04-23 同一天** | 课程作业/实验报告（`含课设（非论文）`、`带操作截图和视频`） |

`Linux2010` 档案仓库自身：`description` 空、`homepageUrl` 空、0★、0 fork、README 最后提交 **2026-04-22**（距今 6 个月，而同期产出见 1.3）。

### 1.3 真实产出（与主页呈现完全脱节）

```
本人发起 PR 总数        : 478
其中已合并              : 267   (55.9% 合并率)
本人发起 Issue 总数     : 203
最近 30 天新建 PR       :  85
最近 30 天被合并 PR     :  74
```

按目标拆开（`is:pr is:merged`，作者=Linux2010）：

| 上游项目 | 我的合并 PR | 我发起 PR | 项目体量 | README 是否体现 |
|---|---|---|---|---|
| `orion-agents`（**org 由本人 2026-07-26 创建，公开成员仅 Linux2010 一人**） | **133** | 139 | orion-code 1★（TS，"goal-driven coding agent for the terminal"，push 2026-11-02）；orion-studio 为 fork（Rust） | ❌ **完全未提及** |
| `openclaw/openclaw` | **16** | 138 | 393,718★ | ✅ 标了"代码贡献者" —— **属实、可验证** |
| `a2aproject/*`（原 `google/a2a-python` 已迁移） | **10**（a2a-python 4 / A2A 5 / a2a-samples 1） | 36 | a2a-python 2,223★ | ⚠️ 标了"代码贡献者"（属实），但**链接指向已迁移的旧地址** |
| `NousResearch/hermes-agent` | **0** | 46 | 259,712★ | ⚠️ 列为"专注项目"但未标贡献者（诚实），然而读者会误读为本人项目 |

`orion-code` 的所有权证据（`GET /repos/orion-agents/orion-code/contributors`）：

```
fork = false        license = MIT        open_issues = 15        watchers = 0
Linux2010        261 commits      ← 绝对主体
AshSgDe29071999    1 commit
```

即：**这是你写的东西，261 次提交，MIT 协议，非 fork。** 而当前 README 对它只字未提，反而把一个 2★、description 为空的 `ai-note`（现已改名 `ai-wiki`）标为"创建者与维护者"的旗舰。

### 1.4 当前 README（`README.md`，1,964 B，4 个标题）

- 外部域名仅 2 个：`github.com`、`t.me`
- **图片 0 张、动态挂件 0 个、`alt` 文本 0 处、自动化标记块 0 个**
- 结构：`### 👋 Hi there` → `### 🚀 专注项目`（2×2 `<table>`）→ `### 🛠️ 技术栈`（5 行纯文本）→ `### 📫 联系方式` → 居中结语
- 全中文（除抬头），技术栈里 `OpenClaw / Claude Code / 阿里云百炼 / Qwen/GLM/Kimi` 与 `Docker / GitHub Actions / Telegram Bot API / SQLite / LanceDB / Ollama` 混排
- git 历史显示曾有过 stats 图（提交 `d2e7711 fix: … 修复 stats 图片错误`），后被移除 —— 现版本是纯文字

---

## 2. 问题清单（按严重度）

### 🔴 P0-1 主页漏掉了唯一真正稀缺的资产：贡献记录

267 个已合并 PR、其中 **133 个进入自己创建的 org、16 个进入 39 万星的 OpenClaw、10 个进入 A2A 协议仓库**，README 里**一个数字都没有**。调研准则 2（"每个论断都要是一个链接"）与准则 3（"自动化=证据"）双双落空。
对照：`yangshun` 用 3,320 B 纯文字、每条带 ★ 数的链接，就撑起了 13.5k 粉档案的说服力。当前 README 用 1,964 B 只写了 4 个**别人的**仓库名。

**这是本次审计最大的单点损失，也是收益最高的改动。**

### 🔴 P0-2 自建旗舰项目 `orion-agents/orion-code` 未出现在主页

`orion-code`（TypeScript，goal-driven terminal coding agent，org 唯一公开成员是你，133 个合并 PR 全部落在这里）才是"我造的东西"；README 却把 `AI-Note` 列为旗舰并标注"创建者与维护者"，而该链接 `Linux2010/ai-note` **现在重定向到 `Linux2010/ai-wiki`，description 为空、2★**。
后果：主页把读者的注意力导向自己最弱的一个仓库，同时把最强的一个藏起来了。

### 🔴 P0-3 档案区 bio 与 README 互相打脸

bio 写 `Java | Python | Bigdata | Data Warehouse | Data Analysis | AI`，README 写"AI 自动化与智能体生态建设者"。bio 在头像正下方、字数少、先被读到 —— 两个叙事并存等于告诉读者"其中一个是旧的"。同理 `name = Andy Tien` vs README 的 `Hope`。
且 bio/blog/X/hireable 全属**改字段即可生效、零代码**的主页组成部分。

### 🔴 P0-4 你亲手创建的组织，主页上看不到

```
GET /orgs/orion-agents/memberships/Linux2010 → { role: "admin", state: "active" }   # 你是管理员
GET /orgs/orion-agents/public_members        → []                                   # 公开成员列表为空
GET /users/Linux2010/orgs                    → 200, length 0                        # 主页不显示任何组织
```

调研结论明确写着：**组织成员身份由 GitHub 原生渲染在头像/姓名下方，是最强可信信号**（`§3 趋势 ⑤`）。而你现在是 `orion-agents` 的 admin —— 一个你 2026-07-26 创建、133 个合并 PR 落在其中的组织 —— 却因为成员身份未公开，主页上**一个像素都没有显示**。

修复成本：档案页 Organization 区块点一次眼睛图标（或 `PUT /orgs/orion-agents/memberships/Linux2010` 配 `visibility=public`，需 `admin:org` scope）。**这是全部改动中性价比最高的一条：零代码、零写作、立刻让主页出现一个"我拥有并组织化的东西"的原生徽章。**

**后续核实（同日补充）**：`GET /user/memberships/orgs` 显示你是 **6 个组织的 admin** —— `JDZWTS · open-horse · quick-hope · skills-repo · orion-agents · agi-wiki`，而 `users/Linux2010/orgs` 长度为 0 → **6 个成员身份全未 publicize，主页 0 曝光**。其中 `skills-repo`（技能组仓库，2026-07-26 创建）有 **37 个公开库、0 个 fork**，体量超过你个人账号下的全部自建仓库 —— 初版审计因只查 `/users/{u}/repos` 而漏掉了它，详见方案 §7.1。

（附带收益：org logo 一旦显示，读者从 README 读到 "Orion" 时能立刻在侧栏对上，形成互证。）

### 🟠 P1-4 68.6% 的仓库是 fork，且置顶仓库疑似未设置

172 个公开仓库里 118 个是 fork、全账号 21★，**档案区精选仓库状态存疑**（GraphQL `pinnedItems` 查询返回空；本环境抓取 `github.com/Linux2010` 返回 `bytes=0` 被拦截，故无法从 HTML 确认是 "Pinned" 还是回落的 "Popular repositories" —— **这一条请你在浏览器里目视确认**）。
读者从 README 点进档案区时，会先看到一批别人的高星项目（`hermes-agent`、`codex`、`a2a-python`、`linux`、`langgraph`、`halo`…）与课程作业报告，形成"fork 收藏者"印象 —— 这会**直接抵销** README 的努力。
补充：`openclaw`/`hermes`/`a2a` 的同名 fork 尤其危险，因为读者分不清 `Linux2010/a2a-python` 与 `a2aproject/a2a-python`。

### 🟠 P1-5 "专注项目"表格存在蹭上游观感 + 链接已失效

- 把 393,718★ 的 `openclaw/openclaw`、259,712★ 的 `NousResearch/hermes-agent` 与自研项并列放在同一个 2×2 网格里，标题统称"专注项目"，只以 `<em>代码贡献者</em>` 轻描淡写区分 —— 语义上把他人项目纳入了自己的叙事。
- `https://github.com/google/a2a-python` **仓库已迁移至 `a2aproject/a2a-python`**（旧地址靠重定向存活，且 `search` API 已查不到该 repo）。主页上任何依赖重定向的链接都会随上游改名而静默失真。
- `Linux2010/ai-note` 同样依赖重定向。

### 🟠 P1-6 档案仓库本身没有"保鲜"

`Linux2010/Linux2010` README 最后提交 2026-04-22；期间产生约 85 PR/30天 的强度，主页却停在 6 个月前。调研中 7/38 档案带自动化标记块，被认定为"真实维护者"最强信号；当前为 0。
注意约束：**不要**因此走公共 `github-readme-stats` 实例 —— 该实例 2025 年末起已被作者承认"无法可靠运行"（issue #4680），Vercel 已终止 OSS 赞助；`Ashutosh00710` activity-graph 的 action 也已无 `action.yml`/tag，网上教程里的 cron 配方是坏的。

### 🟡 P2-7 全中文，与贡献记录的语言环境不匹配

16 个 OpenClaw 合并 PR、10 个 A2A 合并 PR、orion-code 的英文 README —— 目标读者（国际 OSS 协作者、招聘方）读不到中文主页。抬头用英文、正文用中文的混搭，反而两边都不得到信息。

### 🟡 P2-8 深浅色/可访问性/尺寸余量

- 当前 0 图片 → 不存在裂图风险，这是**现有版本的真实优点**（9/38 头部档案也是零挂件），应保留这一取向。
- 但一旦引入图片：仅 1/38 档案做对了 `<picture> + prefers-color-scheme`，`alt` 全覆盖也属少数 → 这是**免费的差异化**，别浪费。
- 体积 1,964 B / 上限 500 KiB：余量巨大，**不构成限制**，但也不应为此堆卡片。
- 主页 README 图片**不走 camo 代理**（实测）→ 第三方服务故障会直接显示为裂图，佐证"优先静态自托管资源"。

### 🟢 现有版本的优点（保留）

1. 零第三方挂件、零死链风险、加载即完整 —— 符合准则 1。
2. 没有访客计数、没有奖杯、没有 snake、没有残留模板注释、没有写死的假数字 —— 干净度已超过样本中约 1/3 的档案。
3. 表格用 `<table>` 双列、`###` 层级生成大纲 —— 技术写法本身没问题。
4. "代码贡献者"的两处标注（OpenClaw、A2A）**经核验属实**，没有虚构履历。

---

## 3. 对照评分（基准 = 38 档案调研）

| 维度 | 头部档案典型做法 | Linux2010 现状 | 判定 |
|---|---|---|---|
| 视觉资产数量 | 0–2 个 | 0 | ✅ 已达最佳区间，勿增 |
| 可验证链接密度 | 每条一个链接 + 数字 | 4 个链接，0 个数字 | ❌ 主要短板 |
| 身份自述 | 一句话 + 具体产物 | 一句话 + 抽象定位 | ⚠️ 缺产物 |
| 自建项目展示 | 明确区分"我造的/我参与的" | 混在同一"专注项目"表格 | ❌ |
| 档案区元数据一致性 | bio/名字/站点一致 | bio 与 README 冲突、blog 空 | ❌ 且改动成本≈0 |
| 档案区精选仓库 | 固定 6–8 个 | 疑似未设置（需浏览器目视确认） | ❌ 且改动成本≈0 |
| 组织身份原生徽章 | admin/owner 徽章直接显示在姓名下方 | 是 orion-agents 的 admin，但**成员身份未 publicize** → 主页 0 显示 | ❌ 改动成本最低、收益最高 |
| 自动化保鲜 | 7/38 采用 | 0，且停更 6 个月 | ❌ |
| 深浅色/`alt` | 1/38 做对 | 无图片，暂不适用 | ➖ 改版时必须做对 |
| 语言与受众 | 匹配贡献环境 | 中文为主 | ⚠️ |
| 死链/掉价挂件 | — | 2 处依赖重定向；无掉价挂件 | ⚠️ |

**一句话结论**：主页不是"不够花哨"，而是**信息错配** —— 用 2 千字节展示了别人的 4 个仓库，却把自己 267 个合并 PR、1 个亲手创建的 org 和 1 个自研 Agent 项目全部藏起来了；同时档案区（bio/名字/精选仓库）在往反方向解释这个人。

---

## 4. 线上实测复核（2026-11-03，浏览器实访 + REST/GraphQL 交叉验证）

| 复核项 | 实测结果 | 对审计的影响 |
|---|---|---|
| 双语切换 | 主页渲染出的切换链接 href 已被 GitHub 改写为绝对地址 `…/Linux2010/blob/main/README.zh-CN.md` | ✅ 档案仓库里的相对链接可正常跳转，双语方案在主页场景成立 |
| 精选仓库 | GraphQL `pinnedItems.totalCount = 0`，页面区块标题实际为 **"Popular repositories"**（非 "Pinned"） | 🟠 **P1-4 由"疑似"升级为确认**：档案区确实在用 GitHub 自动挑选的结果 |
| 组织区块 | 侧栏**没有** Organizations 区块（匿名视图） | 🔴 **P0-4 确认仍未解除** |
| ⚠️ GraphQL 口径陷阱 | `user.organizations` 返回 6 个组织 —— 因为查询令牌就是本人账号，**含未公开成员身份** | 判"是否公开"必须用 `GET /users/{u}/orgs`（返回 0）+ 页面实访，**不能信 GraphQL 的条数** |
| 成就徽章 | 原生渲染且可见：**Pull Shark ×3 · Pair Extraordinaire ×3 · YOLO · Quickdraw · Arctic Code Vault Contributor** | 🟢 与主页"看补丁不看星数"的叙事互证；按调研结论**不需要也不应**把它们嵌进 README（GitHub 无官方嵌入 API，抓 PNG 属 ToS 灰区） |
| bio | 线上仍是 `Java \| Python \| Bigdata \| Data Warehouse \| Data Analysis \| AI`；`isHireable=false`、`twitterUsername=null` | 🔴 **P0-3 未解除**（需 `user` scope 或网页改） |
| 个人站点 | 侧栏公开显示 CSDN 博客，但 REST/GraphQL 均报空；链接实测 200、标题含 "Andy" | 🟡 §1.1 已更正；两份 README 已补入该链接（提交 `44d9887`） |
| 数字漂移 | 一天之内 `267 → 270`、`133 → 136`；`16 → OpenClaw`、`10 → A2A` 未变 | 🟠 实证了"写进主页的数字必须可核验或可刷新"—— 也是方案 §3.1 月度刷新工作流的理由 |

**方法教训**：`curl` 在本环境抓 `github.com` 返回 `bytes=0`（被拦截），一度让我把"精选仓库是否固定"写成推测句。改用浏览器自动化取 DOM 快照 + 双 API 交叉，才把三项悬而未决的判断（pinned / Organizations / 成就徽章）钉成事实。判读主页渲染时，**页面 DOM 是权威，REST 单字段不是**。

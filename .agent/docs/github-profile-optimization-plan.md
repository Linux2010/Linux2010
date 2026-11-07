# 优化方案：`Linux2010` 个人主页改版

> 依据：[调研](./github-profile-research.md) · [审计](./github-profile-audit.md) · 日期 2026-11-02
> 交付物：本方案 + 可直接替换的 [README 草案](./assets/profile-readme-v2.md) + [保鲜工作流草案](./assets/profile-metrics-workflow.yml)
> 目标不是"更好看"，而是**让主页陈述这个账号真实最强的一面**：267 个已合并 PR、亲手创建的 `orion-agents`、进入 OpenClaw/A2A 的贡献。

---

## 0. 策略定位（先定路线，再谈排版）

调研的核心发现是：**粉丝量与挂件数量负相关**，头部档案靠"每个论断都是一个可点击链接"取胜。同时 2026 年公共挂件实例已不可靠（`github-readme-stats` 作者公开承认公共实例无法可靠运行），主页图片还不走 camo 代理 —— 第三方一挂，主页就裂。

因此选定路线：

> **A 案 · 纯文字 + 证据链（采用）** —— 延续当前"零挂件"取向并把它做扎实：一句话身份 → 自建项目 → 上游贡献台账（带数字与 PR 链接）→ 精简技术栈 → 1 个 CTA。视觉资产 ≤2，且全部自托管/静态。
>
> B 案 · Actions 静态卡片（备选，不推荐现在做） —— 用 `readme-tools/github-readme-stats-action@v1` 每晚把 `stats` + `top-langs` 渲染成 SVG 提交进仓库。优点是数据自动更新，代价是每日 commit 噪音 + 需 `contents: write`，且"总星数 21"这类指标**对本账号是负资产**。等 `orion-code` 星数有意义后再考虑。
>
> C 案 · maximalist 卡片墙（否决） —— 样本中与低粉丝量强相关，且依赖已死的公共实例。

**关键取舍**：不展示 `github-readme-stats` 的 stars/contribs 卡。21★ 会自我削弱；改用**能放大优势的指标**（合并 PR 数、目标项目体量），这些数字走 Search API 自算并写死在表格里 + 每月刷新，比通用卡片更贴合"贡献者"叙事。

---

## 1. 信息架构（新版主页骨架）

```
# Andy "Hope" Tien                      ← 一次性解决名字双叙事（P0-3）
> 一句话身份：我造什么、我给谁贡献过      ← 具体产物 + 数字，不写抽象形容词

## 🔧 Building: Orion                    ← 自建项目置顶（P0-2）
或 Orion 是什么（1 行）· 语言 · 3 个仓库链接 · 「133 PRs merged」

## 🌐 Open-source contributions          ← 本方案的灵魂（P0-1）
一张三列小表：项目 | 我的合并 PR | 代表性 PR 链接
  OpenClaw (393k★)  16   → PR #… 
  A2A / a2a-python  10   → PR #1329, #1158
  （orion 自研另列，避免与上游混排）

## 🧰 Stack                              ← 6–10 项，用 <details> 收复杂度（P2-7）
Agent/AI · 语言 · 基础设施 三行；不摆徽章墙

## 📝 Writing / Latest                   ← 自动化标记块（P1-6），无博客则整段省略
<!-- BLOG-POST-LIST:START --> … <!-- BLOG-POST-LIST:END -->

## 📬 Contact                            ← 1 个主 CTA + 2 个次要
```

设计约束：

- 标题用 `##` 起（档案会依据标题自动生成大纲，`###` 起步会让层级失真）。
- **区分"我造的"与"我参与的"** —— 用两个独立章节而非同一张 2×2 网格，直接消除蹭上游观感（P1-5）。
- 每条项目行 = `名称（★数或 PR 数）— 一句话作用 — 链接`。数字用**当日实测值**，见审计 §1.3。
- 全篇英文为主（受众是上游 OSS 与招聘方），中文保留在 bio/一处说明（P2-7）。

---

## 2. 执行清单

### 阶段 1 · 零代码，30 分钟内可完成（收益最大）

| # | 动作 | 位置 | 消除的问题 |
|---|---|---|---|
| 1.0 | **publicize `orion-agents` 组织成员身份** —— 你是该 org 的 `admin`，但 `public_members` 为空，主页姓名下方不显示任何组织徽章。这是全部动作里唯一"零代码 + 零写作 + 立刻出现原生可信徽章"的一条 | 档案页右侧 Organization 区块 → 点亮眼睛图标；或 `gh api -X PUT orgs/orion-agents/memberships/Linux2010 -f visibility=public`（可逆：改回 `private` 即撤销） | 🔴 P0-4 |
| 1.1 | bio 改为与 README 同一叙事，删掉 `Java \| Python \| Bigdata \| Data Warehouse` 旧栈 | 档案 → Edit profile → Bio | 🔴 P0-3 |
| 1.2 | `name` 与 README 抬头统一（建议 `Andy Tien` 保留 + README 内注 `a.k.a. Hope`） | 档案 → Name | 🔴 P0-3 |
| 1.3 | 绑定 X 用户名 / 填 `blog`（若无站点，用 `orion-agents` 相关站点或 `ai-wiki` Pages） | 档案 → Blog / X | 🟠 P1 |
| 1.4 | 视意图开启 `Display current status` 与 `hiredable`（若接受机会） | 档案 | 🟡 |
| 1.5 | **固定 6–8 个精选仓库**：`orion-code`、`ai-wiki`、`awesome-coding-plan`、`master`、`Linux2010`（档案仓库）+ 2 个真正自研项 | 档案区 → Customize your profile pins | 🔴 P1-4 |
| 1.6 | 给 54 个自建仓库补 `description`（至少精选的 8 个），删掉同义反复（`awesome-coding-plan` → `awesome-coding-plan`） | 仓库 → About | 🟠 P1-4 |
| 1.7 | 清理/静默纯镜像的 fork（`linux`、`langgraph`、`halo`、`hive`、`kafka`、`mianshiya`…）：改私有或删除。**118 个 fork 是"fork 收藏者"印象的根因** | 仓库 → Settings | 🟠 P1-4 |

> ⚠️ 1.7 是**不可逆**操作（删除 fork 会丢 Issue/Star 记录，转私有会移出档案区计数）。建议先转私有、观察一周，再决定是否删除；批量操作前先出一份待处理清单给你确认。本方案不代替你执行。

### 阶段 2 · README 重写（一次提交）

| # | 动作 |
|---|---|
| 2.1 | 用 [assets/profile-readme-v2.md](./assets/profile-readme-v2.md) 替换 `README.md`，逐条核对里面的数字与 PR 链接 |
| 2.2 | 把 `google/a2a-python` 改为 `a2aproject/a2a-python`；`Linux2010/ai-note` 改为 `Linux2010/ai-wiki`（两处现依赖重定向） |
| 2.3 | 上游贡献表里为每条至少配 1 个真实 PR 链接（可点击验证 = 准则 2） |
| 2.4 | 若引入任何图片：必须 `<picture>` + `prefers-color-scheme` 双源 + 完整 `alt`（样本仅 1/38 做对，属免费差异化） |
| 2.5 | 删除 `<em>代码贡献者</em>` 这种弱标注，改为"贡献台账"章节 —— 从形容词升级为数据 |

### 阶段 3 · 保鲜自动化（可选，1 个工作流）

| # | 动作 |
|---|---|
| 3.1 | 安装 [assets/profile-metrics-workflow.yml](./assets/profile-metrics-workflow.yml) 到 `.github/workflows/profile-refresh.yml`（每月刷新贡献台账里的合并 PR 数，写进标记块） |
| 3.2 | 若开始写技术文章/有 Dev.to、博客 RSS → 接 `gautamkrishnar/blog-post-workflow@v1`（**纯 RSS、零 token**，样本里读者真会点的挂件）。没有稳定输出源时**不要接**，空标记块比没有更糟 |
| 3.3 | 明确不接：`Ashutosh00710/github-readme-activity-graph` action（已无 `action.yml`/tag，教程配方是坏的，会让主页挂红叉）、访客计数、trophy、snake、streak |
| 3.4 | 工作流安全：token 走 Secrets（`GITHUB_TOKEN` 已够 Search API 用），**不落盘明文 PAT**；`permissions: contents: write` 只给这一个 workflow |

### 阶段 4 · 复核

见 §4 验收标准。

---

## 3. 明确不做（避免返工）

| 诱惑 | 不做的原因 |
|---|---|
| 访客计数器（`komarev`/`hits.sh`） | 8/38 用过，其中 2 人已注释掉；`visitor-badge.glitch.me`、`profile-counter.glitch.me`、`countapi.xyz`、`pufler`、`hits.dwyl` 已死。资深读者读作业余信号，且属隐私外泄 |
| `github-profile-trophy` 奖杯墙 | 3/38；招聘信号≈0；服务 README 自挂"可能因成本下线"横幅 |
| 贡献图贪吃蛇 `Platane/snk` | 2/38；cron 一停就**静默冻结**在旧数据；需额外 `output` 分支 |
| `typing-svg` 打字动画 | 3/38；信息量 0；且 `data-source`/`isMultiple` 已从公共实例移除 |
| 公共 `github-readme-stats` 实例 | 作者承认公共实例不可靠（#4680），Vercel 终止 OSS 赞助；图片不走 camo → 故障直接显示裂图 |
| `lowlighter/metrics` 全家桶 | 事实冻结于 2023-12；重（需 PAT + 每次渲染数十秒）；输出是 2021 审美 |
| **写死数字的静态徽章** | 样本 `HwangTaehyun` 用 shields 硬写 `Code Time 1,342 hrs`、`Profile Views-52` —— 过期即自我打脸。要用数字就必须可追源（PR 链接）或能自动刷新 |
| Spotify / Last.fm / Strava | 需 OAuth，令牌会腐烂；职业档案价值低；Strava 已无在维护的嵌入方案 |
| 预填 Issue 实现的"小游戏/交互主页" | 技术上唯一可行的交互路径，但会给档案仓库灌 Issue，与"专业贡献者"定位冲突 |
| `<center>` / Unicode 数学字体标题 | 实测被渲染器剥离 / 导致标题不可搜索 |
| 把 `openclaw`、`hermes-agent` 与自研并列为"专注项目" | 39 万星项目与 2★ 自研同格陈列，语义上纳他人成果入己叙事；改为"上游贡献台账" |

---

## 4. 验收标准

改版后逐条自检（可直接跑）：

```bash
# 1. 无死链 / 无重定向依赖（应全部 200 且无 redirect）
grep -oE 'https://[^ )"]+' README.md | sort -u | while read u; do
  printf '%s -> ' "$u"; curl -s -o /dev/null -w '%{http_code}\n' -L "$u"; done

# 2. 每条项目论断都带数字（Building / Contributions 两节内应出现 [0-9])
# 3. 若含图片，检查 alt 与深浅色双源
grep -c 'alt="' README.md; grep -c 'prefers-color-scheme' README.md

# 4. 体积远低于 500 KiB 上限
wc -c README.md

# 5. 与档案区叙事一致：bio 里不应再出现 Bigdata/Data Warehouse
gh api users/Linux2010 --jq .bio

# 6. 主页实际渲染（确认未出现裂图，图片直连不经 camo）
curl -s https://github.com/Linux2010 | grep -c 'unreadable-img\|js-unreadable'
```

**内容层面 4 条硬标准**：

1. 读者在**首屏 5 行内**能答出：他造了什么 + 他给哪些大项目合并过代码。
2. 每个论断可点击验证（至少 4 个 PR 级链接）。
3. 自建项目与上游贡献**在版面上物理分离**。
4. 主页不再出现任何自己无法解释的徽章/数字；6 个月后回看不会过期。

---

## 5. 风险与依赖

| 风险 | 影响 | 处置 |
|---|---|---|
| 数字随时间过期（16/10/133 是 2026-11-02 快照） | 主页失真 | 阶段 3 每月刷新，或在文案里用"since 2025"相对表述 |
| Search API 计数含已关闭未合并 PR 的口径差异 | 数字对不上 | 统一口径为 `is:pr is:merged author:Linux2010`，本方案所有数字均此口径 |
| 组织改名再次发生（`google/a2a-python → a2aproject`，`ChungZH → Zirnc` 是同类案例） | 链接静默漂移 | 每季度跑 §4 第 1 项检查；优先用 org 级 `org:a2aproject` 叙事 |
| 转私有/删 fork 属不可逆 | 丢失历史与 Issue | **需你确认**后才批量执行；先转私有观察 |
| `orion-agents` 目前 1★、org 1 follower | 展示自建项目≠有背书 | 措辞用"我创建并维护"而非暗示采用度；这也正是阶段 3 自动刷新的价值：把叙事从"星数"移到"活动量" |
| 工作流 commit 噪音 | 主页提交记录变乱 | 若启用 B 案，把 SVG 提交到独立分支或用 `--no-verify` 之外的常规 squash；月度频率优于每日 |

---

## 6. 优先级速览

| 优先级 | 动作 | 成本 | 收益 |
|---|---|---|---|
| **P0** | **1.0 publicize `orion-agents` 成员身份** | **1 次点击** | 主页立刻出现原生组织徽章；当前你创建的 org 在主页上 0 曝光 |
| P0 | 1.1 bio 与 README 统一叙事 | 2 分钟 | 消除自相矛盾 |
| P0 | 2.1 README 替换为 v2 草案（含贡献台账 + Orion 章节） | 20 分钟 | 主页从"介绍别人的仓库"变成"陈述自己的产出" |
| P0 | 1.5 固定精选仓库（先在浏览器确认当前是 Pinned 还是回落的 Popular） | 3 分钟 | 挡住"fork 收藏者"第一印象 |
| P1 | 2.2 修正两处重定向链接 | 2 分钟 | 消除静默失真 |
| P1 | 1.6 自建仓库补 description | 15 分钟 | 档案区可信度 |
| P1 | 1.3 绑定 blog / X | 5 分钟 | 可信锚点 |
| P2 | 3.1 月度刷新工作流 | 10 分钟 | 保鲜 = 真实维护者信号 |
| P2 | 1.7 fork 清理 | 需决策 | 长期档案区质量（**待你确认**） |

---

## 7. 执行状态（2026-11-02 起）

| 动作 | 状态 | 说明 |
|---|---|---|
| 2.1 / 2.2 / 2.3 / 2.5 README 重写 | ✅ **已上线** | 提交 `c6bc293`，`origin/main` 已推送；经 `GET /repos/Linux2010/Linux2010/readme` 解码比对，线上内容与本地**字节一致**。含 9 个 PR 级链接与 4 个头条数字（267/133/16/10）；两处重定向链接已换成规范地址；标题逐条取自 PR API 原文（草案阶段的意译已修正）。落地版本 **0 图片**，故 `alt`/`prefers-color-scheme` 项暂不适用。 |
| 2.4 图片可访问性 | ➖ 未触发 | 因未引入图片。后续若加，按 §1 约束执行。 |
| **1.0 publicize `orion-agents`** | ❌ **受阻（非权限不足，是 token scope）** | `PUT /orgs/orion-agents/memberships/Linux2010 -f visibility=public` 返回 403 `You must be an admin…` + CLI 提示 `This API operation needs the "admin:org" scope`。当前 `gh auth status` 实际 scopes = `read:org, repo, workflow, write:packages` —— 你是该 org 的 `admin`（`role=admin, state=active` 已确认），缺的只是令牌作用域。执行前后 `public_members` 均为 0，即**主页目前仍不显示组织徽章**。 |
| 3.1 保鲜工作流 | ⏸ **未安装** | 有意未装。线上 README 因此把标记块文案落为 `Counts verified 2026-11-02 against the GitHub Search API` —— 不宣称"每月自动刷新"，避免 README 陈述一件尚未成立的事实。标记对 `<!--START/END_SECTION:contributions-->` 已预留，装工作流时无需再改 README。 |
| **补充：两个自建组织上主页** | ✅ **已上线**（`0f45221`） | 抬头同时点名 `orion-agents` 与 `skills-repo`，并新增 `### 📦 Skills — skills-repo` 章节。所引 12 个技能仓库逐个经 API 核实为 public / `archived=false` / `fork=false`。 |
| **2026-11-03：双语化** | ✅ **已上线**（`93345bc` + `44d9887`） | 按 OSS 通行做法（参照 `ant-design/ant-design` 的 `English · [中文]` 互指模式）拆为 `README.md`（英文默认）+ `README.zh-CN.md`（中文镜像）。两份章节 1:1、链接集完全相同、数字一致，PR 标题保留上游英文原文以维持可验证性。浏览器实访确认切换链接在主页渲染为绝对 blob 地址。 |

### 7.1 审计自查：初版评估漏算了一个更大的隐藏资产

初版审计只看 `GET /users/Linux2010/repos`（个人仓库），因此**没发现 `skills-repo` 这个 37 个公开库、0 fork 的组织** —— 而它和 `orion-agents` 同日创建于 2026-07-26。经 `/user/memberships/orgs` 核实，你是 **6 个组织的 admin**：`JDZWTS · open-horse · quick-hope · skills-repo · orion-agents · agi-wiki`。

**这同时加重了 P0-4**：`GET /users/Linux2010/orgs` 长度为 0 → **6 个成员身份全部未 publicize**，主页上一个组织徽章都不显示。你在 UI 里可以逐个点亮（`github.com/settings/organizations`），一次动作能让 6 个原生徽章同时出现在姓名下方 —— 比 README 里任何一行文字都更有背书力，且当前令牌缺 `admin:org` scope，只能走网页或 `gh auth refresh -h github.com -s admin:org -s user`。

> 教训（写进方法论）：评估 GitHub 档案时，`/users/{u}/repos` 只覆盖个人仓库，**必须再查组织成员身份**，否则会系统性低估产出归属 —— 尤其在 agent 时代，作品常放在自建 org 下而非个人账号。
| 1.1 bio / 1.2 name / 1.3 blog·X / 1.4 hiredable | ⏳ 待办 | 同属账号元数据写入，需要 `user` scope（当前令牌也没有）→ 与 1.0 一并处理。 |
| 1.5 精选仓库 / 1.6 description / 1.7 fork 清理 | ⏳ 待办 | 1.7 不可逆，仍需你确认范围。 |

### 1.0 + 1.1–1.4 的两条可行路径

1. **浏览器操作（推荐，零 scope 变更）**
   - publicize 组织：`https://github.com/settings/organizations` → `orion-agents` → 点 *Publicize*。（档案页右侧 Organizations 区块的眼睛图标是同一开关的另一入口。）
   - bio / 名字 / Blog / X / 状态：`https://github.com/settings/profile` 与档案页 *Edit profile*。
2. **补 scope 后由我继续执行**
   - `gh auth refresh -h github.com -s admin:org -s user`（交互式设备授权，需你在浏览器确认），之后我可自动完成 1.0 并逐条改 bio/name/blog。

> ⚠️ **关于 `.agent/` 目录本身**：本仓库是**公开档案仓库**，其文件列表任何人可见。这些文档含雇主、fork 结构评价、"蹭上游观感"等内部判断，以及一个 Telegram 之外的私人邮箱口径。建议保持**不提交**（当前仅为本地未跟踪文件），或改放到私有仓库/`~/.config`。若你希望公开一部分（如调研结论），建议单独拆出不含个人评价的版本。

# GitHub 个人主页（Profile README）调研

> 调研日期：2026-11-02 · 方法：实抓 38 个真实档案仓库的 README 源码 + GitHub 官方文档 + 渲染器白名单源码 + 12 个生态工具的仓库现状
> 结论用途：为 `Linux2010/Linux2010` 主页改版提供依据。评估与方案见 [github-profile-audit.md](./github-profile-audit.md) / [github-profile-optimization-plan.md](./github-profile-optimization-plan.md)

---

## 0. 主页是怎么渲染出来的（机制）

官方文档（[Managing your profile README](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme)）给出的显示条件只有 4 条，全部可核验：

1. 仓库名 = 用户名（本项目 `Linux2010/Linux2010` 满足，API 字段 `isUserConfigurationRepository: true`）
2. 仓库为 public
3. 根目录存在 `README.md`
4. `README.md` 有非空内容

两个容易踩的坑：

- **2020-07 之前创建的同名仓库不会自动展示**，需手动点 *Share to profile*。
- **移除条件**：README 被删空 / 仓库转私有 / 改名 → 主页整块消失。
- **内容超过 500 KiB 会被截断**（官方数字是 500 KiB，网上流传的 "400 KB" 是错的）。
- 文档明确支持 GitHub Flavored Markdown，并会依据标题自动生成大纲/TOC —— **这意味着标题层级本身就是主页的导航**。
- 官方文档从未提及用 GitHub Actions 维护 README。所谓"自动化"本质都是**一个 Action 往 README 里 commit**，所以所有工具都靠成对标记注释定位插入点（`<!--START_SECTION:xxx-->`）。

---

## 1. 官方渲染器的能力边界（决定"什么能做、什么是自欺"）

来源：渲染管线白名单源码 [html-pipeline `SanitizationFilter`](https://github.com/gjtorikian/html-pipeline/blob/main/lib/html_pipeline/sanitization_filter.rb) + 对线上页面的实测抓包。

**允许（实测存活）**：`h1-h6 br b i strong em a pre code img tt div ins del sup sub p picture ol ul table thead tbody tfoot blockquote dl dt dd kbd q samp var hr ruby rt rp li tr td th s strike summary details caption figure figcaption abbr bdo cite dfn mark small source span time wbr`；属性白名单含 `align width height valign title id border colspan rowspan`（`img` 上是 `src longdesc loading alt`）。

**被静默丢弃（写了等于没写，且不会报错）**：`script`（无 JS）、`style`、`iframe`、`form`、`input`、`button`、`select`、`textarea`、`object`、`embed`、`video`、`audio`、`svg`/`path`（不能内联 SVG）、`canvas`、**`center`**、**`font`**、`link`、`meta`、`noscript`。

实测补充（比 gem 默认白名单更宽/更严之处）：

| 现象 | 实测结论 |
|---|---|
| `img` 上的内联 `style` | **存活**（生产环境比文档白名单宽松，但属未承诺行为，不建议依赖） |
| 用户自写的 `class="..."` | **被 GitHub 自己的 class 覆盖丢弃** |
| `<center>` | **确证被剥离**：`PrincessAkira` 线上页面中 `<center>` 出现 0 次，排版静默塌陷 |
| 超大图片 | GitHub 自动追加 `style="max-width:100%; height:auto; max-height:Npx"` |
| 图片代理 | 主页 README 的 `img src` **直连** shields.io / vercel.app / demolab，**不走 camo**（仅混合内容才 camo）→ 第三方服务故障会直接在主页上显示成裂图 |
| `data:image;base64` | **不允许**（`img src` 协议限定 http/https/相对路径）→ 不能内联 base64 图片 |
| `javascript:` 链接 | 被剥离 |

推论：**"交互型主页"只能靠 URL 本身实现** —— 唯一成熟手法是预填 Issue 链接（`issues/new?body=...`）做井字棋/四子棋，见 `JonathanGin52`、`DoubleGremlin181`。代价是会给仓库灌 Issue，职业档案不建议。

---

## 2. 38 个真实档案的模式频率（N=38）

抓取方式：`raw.githubusercontent.com/<login>/<login>/HEAD/README.md`，失败回退 `gh api repos/<login>/<login>/readme`（若干账号默认分支导致 raw 404）。样本池来自任务指定账号 + `abhisheknaiidu/awesome-github-profile-readme` 里的 218 个 login。

> 附带发现：那份"权威合集"里 **约 199 个链接已 404，仅约 19 个还活着** —— 网上教程的默认推荐基本建立在死链上，不要照着抄。

### 2.1 头部账号的真实做法（最重要的反直觉结论）

| 账号 | 粉丝 | README 体积 | 第三方挂件数 | 结构 |
|---|---|---|---|---|
| `sindresorhus` | 85,157 | 610 B | **0** | 无标题，纯文字 + 15 张自托管搞笑 GIF，CTA = 自己的 App + 博客 |
| `rauchg`（Guillermo Rauch） | 17,491 | **308 B** | **0** | `# rauchg` + 一个 GIF + `## Credits`；用 `bash`/`js` 代码块代替"联系方式" |
| `anuraghazra` | 14,979 | 2,820 B | 1 个服务（自己的产品） | 横幅 → About → 技术图标 → stats+top-langs 双列表格 → Top Repositories 置顶卡 |
| `yangshun`（Yangshun Shek） | 13,549 | 3,320 B | 1 个（`ghchart`） | **纯文字**：Building / Writing / Past projects / GitHub activity / Try GreatFrontEnd |
| `holman`（GitHub 前工程总监） | 6,516 | **117 B** | 0 | 9 行 `# holman` 循环冷笑话，反档案即人格 |
| `ascoders`（廖雪峰式作者，《React 系列》） | 3,923 | 819 B | 1 | 两句话 → 1 张 stats 卡 → 把流量导向 `weekly` 仓库 |
| `xcatliu` | 4,070 | 573 B | 0 | 官方默认模板 5 条要点，但每条都填了**可验证的具体产物** |
| `jayli` | 1,844 | 457 B | 0 | 整份档案 = 一个 ` ```javascript ` 类声明，用代码当身份 |
| `frostming`（HTTPie/PDM 维护者） | 1,719 | 1,539 B | 3 | typing-svg + 社交徽章 + **自建** stats 实例 + 技术徽章，无访客计数、无奖杯 |
| `jamesgeorge007` | 1,300 | 1,728 B | 1 | `### :zap: Recent Activity` + `<!--START_SECTION:activity-->` 自动更新 |
| `DenverCoder1`（挂件作者） | 5,109 | **30,902 B** | 7 |  maximalist：赞助阶梯表格 + YouTube 卡片 + 7 个 `<details>` + streak + activity graph；仍给 131 张图写了 `alt` |

**结论 1：粉丝量与挂件数量呈负相关。** 顶部账号 0–2 个视觉资产；把主页堆满卡片的多是低粉丝量账号（`HwangTaehyun` 223 粉、`subhadipbhowmik` 45 粉、`biezhi` 18 粉、`vinayakshukla` 1 粉）。`anuraghazra`/`DenverCoder1` 的密集排版是**卖工具的广告位**，不是职业档案范式。

**结论 2：可信度来自"每条都是一个可点击的链接"。** `yangshun` 每个列表项都是 `仓库名（★数）+ 一句上下文`；`martonlederer` 直接写创办了什么、在哪工作；`xcatliu` 把模板槽位换成真产物。中低档案则写成一排 emoji + 技术徽章墙，**没有任何可验证的东西**。

### 2.2 模式频率统计

| 模式 | 采用数 | 判断 |
|---|---|---|
| `align="center"` 容器 | 19/38 | 标准做法，成本不可见 |
| `img.shields.io` 徽章 | 15/38 | 技能/社交可用，`flat-square` + `logo=` 是通行风格 |
| **`github-readme-stats`** | 17/38 | 采用率最高的挂件 |
| ├ `/api/top-langs` | 7/38 | **比 stats 卡信息密度更高** |
| └ `/api/pin` 仓库卡 | 4/38 | ≤3 张才有价值 |
| **自动化标记块**（waka / activity / BLOG-POST-LIST / YOUTUBE-CARDS / QUOTE） | 7/38 | **"真实维护者"的最强信号**：它要求你真的有工作流文件 |
| WakaTime | 5/38 | 诚实、易保鲜、高信号 |
| `<details>` 折叠 | 5/38 | **被严重低估**，是处理技能清单膨胀的最佳工具 |
| 访客计数器（komarev 4 / profile-counter 1 / visitor-badge 2 / pufler 1 / hits 1） | 8/38（**其中 2 人已把它注释掉**） | **掉价 + 服务已死** |
| `<table>` 排版 | 4/38 | 只在需要双列卡片时用 |
| 奖杯 trophy | 3/38 | 过时，`rank=` 过滤版略好 |
| typing-svg | 3/38 | 纯装饰，信息量 0 |
| 贡献图 snake | 2/38 | 噱头，噪 |
| `skillicons.dev` | 3/38 | 手写徽章墙的现代替代 |
| activity-graph 折线 | 2/38 | 看起来像虚荣指标 |
| streak-stats | 3/38 | 虚荣指标 |
| **`<picture>` + `prefers-color-scheme`**（官方深浅色方案） | **1/38**（`wayou`） | 几乎无人做；做了就进实测前 3% |
| `#gh_dark_mode` / `#gh-light-mode-only` 锚点 | **0/38** | 无文档承诺，无人依赖 |
| Spotify / last.fm / Twitch | 3/38 | 有个性，职业档案价值低 |
| **零第三方挂件**（纯文字/自托管资源） | 9/38 | 与名气正相关 |
| 残留模板注释 / 挂件 `username=` 指向别人 | ≥2/38 | **一眼看穿"生成器复制"** |

CTA 分布：Twitter/X 15、LinkedIn 10、博客 9、mailto 7、YouTube 7、BuyMeACoffee/ko-fi/sponsors 5。**社交链接近乎必备，变现 CTA 并非必备**（头部账号通常只留 1 个 CTA，且带追踪参数，如 `yangshun` 的 `?gnrs=yangshun`）。

### 2.3 明确的"抄作业反面教材"

- `biezhi`（18 粉）：所有挂件 `username=` 仍是模板默认值 `hellokaton`，`<h1>` 用 `</h3>` 闭合，打赏链接指向别人 → **从未配置过生成器**。
- `vinayakshukla`（1 粉）：GitHub 初始模板那段 `<!-- **xxx** is a ✨ special ✨ repository … Fun fact: … -->` 注释还留在文件里。
- `HwangTaehyun`（223 粉）：用静态 shields 徽章**写死假指标** —— `Code Time 1,342 hrs`、`AI Code Time 62 hrs 39 mins`、`Profile Views-52`。硬编码数字一旦过期就是自我打脸。
- `PrincessAkira`（2 粉）：`<center>` + Unicode 数学字母标题 `𝓦𝓮𝓵𝓬𝓸𝓶𝓮` → 排版被剥离、标题不可搜索。
- `ChungZH`（已改名 `Zirnc`）：仅 445 B、约 200 字散文 + 1 个 `dynamic/json` 实时徽章（Telegram 频道订阅数）→ **单位体积信号量全样本最高**。

---

## 3. 2026 年生态工具现状（可用性是硬约束）

| 工具 | 状态 | 关键事实与风险 |
|---|---|---|
| `anuraghazra/github-readme-stats`（79.9k★） | 仓库活跃（2026-10-01），**主题新增已官方"暂停"** | ⚠️ **公共实例已不可靠**：Vercel 终止 OSS 赞助（#3851），#4680（2025-12-27 关闭）原文"we can no longer reliably operate the public instance"。`count_private` 已从文档移除，私有数据需自建 + PAT。 |
| **`readme-tools/github-readme-stats-action@v1`**（325★，2026-10-26） | ✅ **官方推荐的替代路径** | cron 生成**静态 SVG 提交进你的仓库**（`card: stats\|top-langs\|pin\|wakatime\|gist`）。无 502、无第三方依赖。代价：需 `contents: write`，每日 commit 噪音。 |
| `ryo-ma/github-profile-trophy`（6.7k★） | 活跃（v1.0 首发 2026-01-27） | README 自己列了 ~17 个志愿者镜像并挂"**服务可能因成本下线**"横幅；主页实例压力极大。社区共识：装饰性，招聘信号≈0。 |
| `Ashutosh00710/github-readme-activity-graph`（2.4k★） | ⚠️ **最后 push 2026-05-17，已无 `action.yml`、无 tag** | **网上疯传的 `uses: Ashutosh00710/...@vX` cron 工作流已失效**，大量主页上挂着红叉。可用替代：`jamesgeorge007/github-activity-readme@master`（2026-08-05，写 `<!--START_SECTION:activity-->`）、`maurodesouza/github-readme-activity-graph-action@v1`（2026-07，生成静态 SVG）。`guilhermnevesdias` 分叉与 ArcOS 图仓库**已 404**。 |
| `Platane/snk`（6.2k★，v3.5.0 2026-04-25） | 活跃 | 贪吃蛇 SVG 写入 `output`/`github-actions` 分支。故障模式：分支冲突、GIF 数 MB、cron 一停就**静默冻结**在旧数据上。 |
| `lowlighter/metrics`（17.4k★） | ❄️ **事实冻结：最后代码提交 2023-12-18**，最后 release v3.34（2023-09），此后只有 dependabot | 功能最全（isocalendar/lines/stars/achievements/traffic…），但 2023 年产物 + 渲染重 + 需 PAT。仍可用，属"重投入"。 |
| `DenverCoder1/readme-typing-svg`（9.5k★，v0.10.0 2026-08-31） | 活跃 | ⚠️ `isMultiple` 和 `data-source`（Dev.to/SO/StackOverflow 拉取）**已被移除**，现在只是静态打字动画。 |
| `DenverCoder1/github-readme-streak-stats`（7.2k★，v1.8.1 2026-10-24） | 活跃，实测可用 | `https://streak-stats.demolab.com/?user={u}&theme=…&mode=weekly`。指标本质是"连续打卡"，虚荣。 |
| `gautamkrishnar/blog-post-workflow`（3.4k★，2026-10-07） | ✅ 活跃 | **纯 RSS、零 token**，写进 `<!-- BLOG-POST-LIST:START/END -->`，支持 `$title/$url/$date/$emojiKey` 模板。样本里被点击率最好的挂件类型。 |
| `rahuldkjain/github-profile-readme-generator` | 活跃（V2 于 2025-10 发布） | 生成器本身没问题，问题是用户不编辑 → 参见 2.3 的 `biezhi` 现象。 |
| `kyechan99/capsule-render`、`vn7n24fzkq/github-profile-summary-cards`、`yoshi389111/github-profile-3d-contrib`、`silentsoft/hits`（`hits.sh`）、`antonkomarev/ghpvc` | 代码仍在维护 | 视觉上属 2021 年审美；`hits.sh`/`ghpvc` 是访客计数，**资深读者读作业余信号**。 |
| **已死/别用** | | `komacms/github-visits`(404)、`artem-shipilev/github-hit-counter`(404)、`countapi.xyz`(2021 停服)、`omidnikrah/github-readme-medium`(2023)、`Qunff/lastfm-readme-stats`(404)、Vibrating badge（查无服务）、`visitor-badge.glitch.me`（glitch.me 已停）、`badges.pufler.dev/visits`、`hits.dwyl.com`（http + 死）、`github-trophies.vercel.app`（旧域名，现为 `github-profile-trophy`）。 |
| 音乐/运动类 | 需 OAuth，令牌会腐烂 | Spotify `kittinan/spotify-github-profile`（2026-10-13 活跃，需一次性 OAuth）；Last.fm `JeffreyCA`（需 API key）；**Strava 无在维护的嵌入方案**；YouTube 卡片需 Google API key 且 `dhyeythumar/youtube-stats-card` 已休眠（2025-01）。 |
| 高信号非虚荣项 | ✅ | `https://ghchart.rshah.org/<login>`（`yangshun` 在用）；`img.shields.io/github/followers/<login>?style=social`；`shields.io/badge/dynamic/json` 挂真实订阅数（`Zirnc`）；Kaggle `road-to-kaggle-grandmaster.vercel.app/api/badges/{user}/competition/light`（2026-01 活跃）；LeetCode `leetcard.jacoblin.cool/{user}`。 |
| 成就徽章（Pull Shark / Starstruck / Galaxy Brain / Pair Extraordinaire） | ⚠️ | **GitHub 无官方嵌入 API**，主页原生渲染。可选：`metrics` 的 `plugin_achievements`；抓 PNG 属 ToS 灰区（`drknzz/GitHub-Achievements` 2024 后停更）。 |

**2025–2026 趋势**：① AI 工程师普遍用 shields 静态徽章摆出模型/Agent 栈 + 置顶 dotfiles/agent 配置仓库；② 贡献真实性优先于花哨 —— `show=prs_merged,prs_merged_percentage,reviews,discussions_answered` 正好是 AI 时代审阅者想看的指标；③ GitHub 2026 年新增官方文档 [Using your GitHub profile to enhance your resume](https://docs.github.com/en/account-and-profile/tutorials/using-your-github-profile-to-enhance-your-resume)（甚至建议用 Copilot 写 README，立场是"文字 + 置顶仓库 > 卡片堆砌"）；④ 组织成员身份由 GitHub 原生渲染，是最强可信信号；⑤ 浏览型目录：`rzashakeri/beautify-github-profile`（12.5k★，2026-09 更新）。

---

## 4. 可直接复用的 URL 模板（已剔除死服务）

```
# —— 统计卡（优先 Actions 静态化，公共实例已不可靠）——
https://github-readme-stats.vercel.app/api?username=<u>&show_icons=true&include_all_commits=true&theme=<t>&hide_border=true&locale=cn&hide=issues,contribs&show=prs_merged,prs_merged_percentage,reviews
https://github-readme-stats.vercel.app/api/top-langs/?username=<u>&layout=compact&langs_count=6&theme=<t>&hide_border=true&hide=jupyter%20notebook,tex,css&exclude_repo=<r>&bg_color=00000000
https://github-readme-stats.vercel.app/api/pin/?username=<u>&repo=<r>&theme=<t>&show_owner=true
#   locale 用 cn（不是 zh_cn）；bg_color=00000000 = 透明背景；hide=/exclude_repo= 是" curated"与"生成器默认"的分界线

# —— 高信号、低维护 ——
https://ghchart.rshah.org/<login>                     # 贡献热力，yangshun 在用，无鉴权
https://img.shields.io/github/followers/<login>?label=Follow&style=social
https://skillicons.dev/icons?i=ts,python,react,docker,kubernetes
https://gautamkrishnar/blog-post-workflow  <!-- BLOG-POST-LIST:START --> ... <!-- END -->   # 纯 RSS

# —— 深浅色官方唯一方案（38 个样本里只有 1 人做对）——
<picture>
  <source media="(prefers-color-scheme: dark)"  srcset="<dark-asset>">
  <source media="(prefers-color-scheme: light)" srcset="<light-asset>">
  <img alt="<必要，屏幕阅读器>" src="<fallback>" width="...">
</picture>
```

不要用：访客计数、trophy、snake、streak、activity-graph 公共实例、typing-svg、capsule 波浪横幅、giphy 挥手 GIF（以及从 `TheDudeThatCode`/`ABSphreak` 等他人仓库盗链的 `Hi.gif`）、Unicode 数学字体标题、写死数字的静态徽章。

---

## 5. 调研得到的 6 条设计准则（方案的依据）

1. **少即是多，且可量化**：粉丝量与挂件数负相关；目标控制在 ≤2 个视觉资产。
2. **每个论断都要是一个链接**：项目名 + ★数 + 一句上下文；不给无法验证的形容词。
3. **自动化 = 证据，不是装饰**：能证明"我持续产出"的标记块（RSS / activity）才有价值；纯打字动画没有。
4. **公共第三方实例在 2026 已不可靠**：要卡片就走 Actions 静态化或自建，否则宁可用纯文字。
5. **可访问性与深浅色正确性是免费的差异化**：实测仅 1/38 做对 `<picture>`；`alt` 全覆盖也属少数。
6. **主页 ≠ 只有 README**：档案区的 bio / blog / 社交 / hireable 字段与**置顶仓库**是主页渲染的一部分，且不写代码就能改 —— 见评估文档，这是本项目最大的即时收益点。

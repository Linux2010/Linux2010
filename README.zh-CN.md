## Andy Tien <sub>(又名 Hope)</sub>

[English](./README.md) · **简体中文**

**我做编程 Agent，也把修复真正合并进上游。**

我创建了两个组织：**[Orion](https://github.com/orion-agents)** —— 面向终端的目标驱动编程 Agent；
**[skills-repo](https://github.com/skills-repo)** —— 精选 AI Agent 技能库。
同时是 **[OpenClaw](https://github.com/openclaw/openclaw)** 与 **[A2A 协议](https://github.com/a2aproject)** 的上游代码贡献者。

`270 个已合并 PR` · `136 → Orion` · `16 → OpenClaw` · `10 → A2A`

---

### 🔧 自建项目 — Orion

运行在终端里的目标驱动编程 Agent。TypeScript 编写，面向长任务与可评审的差异，而不是"一次生成完事"。
自 2026 年 7 月起，我在这里发起并合并了 **136 个 PR**。

| 仓库 | 内容 |
| --- | --- |
| [orion-code](https://github.com/orion-agents/orion-code) | Agent 本体 —— 目标循环、工具编排、终端界面（TypeScript，MIT） |
| [orion-code-docs](https://github.com/orion-agents/orion-code-docs) | 文档 |
| [orion-agents-www](https://github.com/orion-agents/orion-agents-www) | 项目站点 |

---

### 📦 技能库 — skills-repo

**[skills-repo](https://github.com/skills-repo)**（技能组仓库）—— 我在 2026 年 7 月创建的另一个组织。
**37 个已发布技能库，全部自研（0 个 fork）**：一个仓库对应一个领域，每个库都同时包含成文方法论，
以及 Agent 可以直接运行的质量门禁脚本。

| 领域 | 代表技能库 |
| --- | --- |
| 工程 | [security-guardian](https://github.com/skills-repo/security-guardian) · [database-engineer](https://github.com/skills-repo/database-engineer) · [devops-engineer](https://github.com/skills-repo/devops-engineer) · [accessibility-engineer](https://github.com/skills-repo/accessibility-engineer) · [machine-learning-engineer](https://github.com/skills-repo/machine-learning-engineer) |
| 产品与内容 | [product-manager](https://github.com/skills-repo/product-manager) · [docs-writer](https://github.com/skills-repo/docs-writer) · [content-creator](https://github.com/skills-repo/content-creator) · [visual-designer](https://github.com/skills-repo/visual-designer) |
| 运营与增长 | [startup-founder](https://github.com/skills-repo/startup-founder) · [marketing-master](https://github.com/skills-repo/marketing-master) · [career-coach](https://github.com/skills-repo/career-coach) |

收录原则：**一个领域、一组技能、一套方法论。** 宁缺毋滥 —— 当一个库的内容被并入后继库时，它会被归档，
而不是为了凑数量继续挂着。

---

### 🌐 上游贡献

不看星数，看补丁。下面每一行都可以点开验证（PR 标题保持上游英文原文，未翻译）。

**[OpenClaw](https://github.com/openclaw/openclaw)** · 个人 AI Agent 基础设施 · 合并 16 个 PR

- [#189210](https://github.com/openclaw/openclaw/pull/189210) — fix: claude-cli history shows synthetic lifecycle rows as assistant replies
- [#183165](https://github.com/openclaw/openclaw/pull/183165) — fix(agents): recover deferred tool calls through native correction
- [#175058](https://github.com/openclaw/openclaw/pull/175058) — fix(ai): recover DeepSeek DSML tool calls when routed through OpenRouter
- [#177272](https://github.com/openclaw/openclaw/pull/177272) — fix(matrix): stop seeding new thread sessions from the room transcript
- [#165310](https://github.com/openclaw/openclaw/pull/165310) — fix: compaction repeatedly fails when a session-bound cron job targets the same session

**[A2A / Agent2Agent](https://github.com/a2aproject)** · 智能体互操作协议 · 合并 10 个 PR

- [#1329](https://github.com/a2aproject/a2a-python/pull/1329) — fix(server): isolate push notification failures from the task lifecycle
- [#1158](https://github.com/a2aproject/a2a-python/pull/1158) — fix: replace deprecated `FieldDescriptor.label` with `is_repeated` in proto_utils
- [#1077](https://github.com/a2aproject/a2a-python/pull/1077) — fix: raise on `append=True` for unknown `artifact_id`
- [#534](https://github.com/a2aproject/A2A/pull/534) — fix(spec): Optimize the camel case naming

<!--START_SECTION:contributions-->
*以上数字于 2026-11-03 通过 GitHub Search API 核验，口径为 `is:pr is:merged author:Linux2010`。*
<!--END_SECTION:contributions-->

---

### 🧰 技术栈

<details>
<summary><b>Agent 与 AI</b> — OpenClaw · Claude Code · A2A 协议 · Qwen / GLM / Kimi / DeepSeek（阿里云百炼）</summary>

工具调用恢复、会话与压缩（compaction）语义、多模型路由、Agent 编排与定时任务循环。
我的时间大多花在失败模式上：工具调用被推迟、历史行是合成出来的、定时任务活得比它所绑定的会话还久，
这些情况下应该发生什么。
</details>

<details>
<summary><b>开发语言</b> — TypeScript · Python · Java · Bash</summary>

TypeScript 写 Agent 运行时，Python 写协议与 SDK，Java 处理数据链路。
</details>

<details>
<summary><b>基础设施</b> — Docker · GitHub Actions · Telegram Bot API · Matrix · SQLite · LanceDB</summary>

多容器 Agent 部署、由 CI 驱动的贡献流水线、基于 embedding 的笔记检索。
</details>

---

### 📖 笔记

[ai-wiki](https://github.com/Linux2010/ai-wiki) —— 关于 Agent 基础设施与贡献玩法的工作笔记。
刻意保持小体量：这里是我记录"什么东西坏了"的地方。

---

### 📬 联系我

- ✉️ [linuxmorebetter@gmail.com](mailto:linuxmorebetter@gmail.com) ← 主要渠道
- 💬 Telegram [@worldhello321](https://t.me/worldhello321)
- ✍️ [blog.csdn.net/UncleTian](https://blog.csdn.net/UncleTian) —— 中文技术博客
- 📍 上海 · 中文沟通请直接邮件或 Telegram

<sub>当前在做：让 Orion 处理长程重构时不丢失主线。</sub>

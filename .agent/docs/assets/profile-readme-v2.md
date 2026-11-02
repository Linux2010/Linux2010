> **这是可直接替换 `README.md` 的草案**（方案 A：纯文字 + 证据链）。
> 用法：核对下方 ⚠️ 标注处（个人意图类字段），然后整体复制进 `Linux2010/README.md`。
> 所有数字与 PR 链接均为 2026-11-02 经 `gh api` 实测，口径统一为 `is:pr is:merged author:Linux2010`。

```markdown
## Andy Tien <sub>(a.k.a. Hope)</sub>

**I build coding agents, and I land the fixes.**
Creator of **[Orion](https://github.com/orion-agents)** — a goal-driven coding agent for the terminal.
Upstream contributor to **OpenClaw** and the **A2A (Agent2Agent) Protocol**.

`267 pull requests merged` · `16 → OpenClaw` · `10 → A2A` · `133 → Orion`

---

### 🔧 Building — Orion

A goal-driven coding agent that runs in your terminal. TypeScript, built for long-horizon tasks and
reviewable diffs rather than one-shot completions.

| Repo | What it is |
|---|---|
| [orion-agents/orion-code](https://github.com/orion-agents/orion-code) | The agent — goal loop, tool orchestration, terminal UI (TypeScript) |
| [orion-agents/orion-code-docs](https://github.com/orion-agents/orion-code-docs) | Documentation |
| [orion-agents/orion-agents-www](https://github.com/orion-agents/orion-agents-www) | Project site |

I opened and merged **133 PRs** here since Jul 2026. ⚠️ *如需，补一句"当前阶段/是否欢迎试用"*

---

### 🌐 Upstream contributions

Not stars — patches. Every row is verifiable by clicking.

**[OpenClaw](https://github.com/openclaw/openclaw)** — personal AI agent infrastructure · 16 PRs merged

- [#189210](https://github.com/openclaw/openclaw/pull/189210) — fix: Claude CLI history showed synthetic lifecycle rows as assistant messages
- [#183165](https://github.com/openclaw/openclaw/pull/183165) — fix(agents): recover deferred tool calls through native corrections
- [#175058](https://github.com/openclaw/openclaw/pull/175058) — fix(ai): recover DeepSeek DSML tool calls when routed through OpenAI-compatible providers
- [#177272](https://github.com/openclaw/openclaw/pull/177272) — fix(matrix): stop seeding new thread sessions from the room transcript
- [#165310](https://github.com/openclaw/openclaw/pull/165310) — fix: compaction repeatedly failed when a session-bound cron job was active

**[A2A / Agent2Agent](https://github.com/a2aproject)** — agent-to-agent interop protocol · 10 PRs merged

- [#1329](https://github.com/a2aproject/a2a-python/pull/1329) — fix(server): isolate push-notification failures from the task handler
- [#1158](https://github.com/a2aproject/a2a-python/pull/1158) — fix: replace deprecated `FieldDescriptor.label` with `is_repeated`
- [#534](https://github.com/a2aproject/A2A/pull/534) — fix(spec): camelCase naming consistency
- [#1077](https://github.com/a2aproject/a2a-python/pull/1077) — fix: raise on `append=True` for unknown `artifact_id`

<!--START_SECTION:contributions-->
*Last verified: 2026-11-02 · refreshed monthly*
<!--END_SECTION:contributions-->

---

### 🧰 Stack

<details>
<summary><b>Agents & AI</b> — OpenClaw · Claude Code · A2A protocol · Qwen / GLM / Kimi / DeepSeek via 阿里云百炼</summary>

Tool-call recovery, session/compaction semantics, multi-provider routing, agent orchestration and
scheduled task loops. I spend most of my time on the failure modes: what happens when a tool call
deferred, a history row is synthetic, or a cron job outlives its session.
</details>

<details>
<summary><b>Languages</b> — TypeScript · Python · Java · Bash</summary>

TypeScript for the agent runtime, Python for protocol/SDK work, Java for data plumbing.
</details>

<details>
<summary><b>Infrastructure</b> — Docker · GitHub Actions · Telegram Bot API · Matrix · SQLite · LanceDB</summary>

Multi-container agent deployments, CI-driven contribution loops, embedding-backed note retrieval.
</details>

---

### 📖 Notes

[Linux2010/ai-wiki](https://github.com/Linux2010/ai-wiki) — working notes on agent infrastructure and
contribution playbooks. Small on purpose; it's where I write down what broke.

---

### 📬 Reach me

- ✉️ [linuxmorebetter@gmail.com](mailto:linuxmorebetter@gmail.com) ← **primary**
- 💬 Telegram [@worldhello321](https://t.me/worldhello321)
- 🇨🇳 中文沟通请直接邮件或 Telegram；上海 · 蚂蚁集团

<sub>Currently: making Orion handle long-horizon refactors without losing the thread.</sub>
```

---

## 替换前必读

| 项 | 说明 |
|---|---|
| ⚠️ 先 publicize 组织身份 | 在档案页点亮 `orion-agents` 成员身份（方案 1.0）。否则读者读到 "Creator of Orion" 时，侧栏没有任何组织徽章与之互证 —— 文案与主页呈现会脱节。 |
| ⚠️ `company` / 在职表述 | 草案把"蚂蚁集团"放在 Contact 一行。若不希望主页出现雇主，删掉即可 —— 但档案区 `company=antgroup` 仍会显示，需一并改（方案 1.1）。 |
| ⚠️ `hiredable` / 求职意图 | 草案末尾"Currently:"是中性的在做事项。若你确实在看机会，改成明确的 *Open to …* 并开启档案区 hiredable；若非，保持现状。 |
| ⚠️ orion-code 成熟度 | 该项目当前 1★、org 1 follower。草案刻意用"我 opened and merged 133 PRs here"叙述**活动量**而非采用度，不要改成暗示用户量的措辞。 |
| 数字口径 | `267 merged / 16 / 10 / 133` 为 2026-11-02 快照。过期表现已由 `<!--START_SECTION:contributions-->` 标记 + 月度工作流覆盖（见 [profile-metrics-workflow.yml](./profile-metrics-workflow.yml)）。 |
| 抬头 `##` 而非 `#` | GitHub 会按标题自动生成大纲；用 `##` 起可让后续 `###` 层级正确。档案仓库不额外显示 H1 页面标题，因此 `#` 也可，但层级会整体上移一级。 |
| 无图片 | 有意为之：零裂图、零第三方依赖、零隐私外泄。若要加卡片，必须走 `<picture>` + `prefers-color-scheme` + `alt`，且优先 Actions 静态 SVG（方案 B 案），**不要**用公共 `github-readme-stats` 实例。 |
| 两处链接已修正 | `google/a2a-python` → `a2aproject/*`（旧地址仅靠重定向存活）；`Linux2010/ai-note` → `Linux2010/ai-wiki`（已改名）。 |

## Andy Tien <sub>(a.k.a. Hope)</sub>

**I build coding agents, and I land the fixes.**

Creator of **[Orion](https://github.com/orion-agents)** — a goal-driven coding agent for the terminal —
and of **[skills-repo](https://github.com/skills-repo)**, a library of curated AI-agent skills.
Upstream contributor to **[OpenClaw](https://github.com/openclaw/openclaw)** and the **[A2A Protocol](https://github.com/a2aproject)**.

`267 PRs merged` · `133 → Orion` · `16 → OpenClaw` · `10 → A2A`

---

### 🔧 Building — Orion

A goal-driven coding agent that runs in your terminal. TypeScript, built for long-horizon tasks and
reviewable diffs rather than one-shot completions. I opened and merged **133 PRs** here since Jul 2026.

| Repo | What it is |
| --- | --- |
| [orion-code](https://github.com/orion-agents/orion-code) | The agent — goal loop, tool orchestration, terminal UI (TypeScript, MIT) |
| [orion-code-docs](https://github.com/orion-agents/orion-code-docs) | Documentation |
| [orion-agents-www](https://github.com/orion-agents/orion-agents-www) | Project site |

---

### 📦 Skills — skills-repo

**[skills-repo](https://github.com/skills-repo)** (技能组仓库) — the other org I created, in Jul 2026.
**37 published skill libraries, all self-authored (0 forks)**: one domain per repo, each pairing a written
methodology with runnable quality-gate scripts an agent can execute directly.

| Domain | Example libraries |
| --- | --- |
| Engineering | [security-guardian](https://github.com/skills-repo/security-guardian) · [database-engineer](https://github.com/skills-repo/database-engineer) · [devops-engineer](https://github.com/skills-repo/devops-engineer) · [accessibility-engineer](https://github.com/skills-repo/accessibility-engineer) · [machine-learning-engineer](https://github.com/skills-repo/machine-learning-engineer) |
| Product & content | [product-manager](https://github.com/skills-repo/product-manager) · [docs-writer](https://github.com/skills-repo/docs-writer) · [content-creator](https://github.com/skills-repo/content-creator) · [visual-designer](https://github.com/skills-repo/visual-designer) |
| Ops & growth | [startup-founder](https://github.com/skills-repo/startup-founder) · [marketing-master](https://github.com/skills-repo/marketing-master) · [career-coach](https://github.com/skills-repo/career-coach) |

Stated rule for the catalogue: one domain, one skill group, one methodology — quality over breadth, so a repo
gets archived when its content folds into a successor rather than kept alive for the count.

---

### 🌐 Upstream contributions

Not stars — patches. Every line below is clickable and verifiable.

**[OpenClaw](https://github.com/openclaw/openclaw)** · personal AI agent infrastructure · 16 PRs merged

- [#189210](https://github.com/openclaw/openclaw/pull/189210) — fix: claude-cli history shows synthetic lifecycle rows as assistant replies
- [#183165](https://github.com/openclaw/openclaw/pull/183165) — fix(agents): recover deferred tool calls through native correction
- [#175058](https://github.com/openclaw/openclaw/pull/175058) — fix(ai): recover DeepSeek DSML tool calls when routed through OpenRouter
- [#177272](https://github.com/openclaw/openclaw/pull/177272) — fix(matrix): stop seeding new thread sessions from the room transcript
- [#165310](https://github.com/openclaw/openclaw/pull/165310) — fix: compaction repeatedly fails when a session-bound cron job targets the same session

**[A2A / Agent2Agent](https://github.com/a2aproject)** · agent-to-agent interop protocol · 10 PRs merged

- [#1329](https://github.com/a2aproject/a2a-python/pull/1329) — fix(server): isolate push notification failures from the task lifecycle
- [#1158](https://github.com/a2aproject/a2a-python/pull/1158) — fix: replace deprecated `FieldDescriptor.label` with `is_repeated` in proto_utils
- [#1077](https://github.com/a2aproject/a2a-python/pull/1077) — fix: raise on `append=True` for unknown `artifact_id`
- [#534](https://github.com/a2aproject/A2A/pull/534) — fix(spec): Optimize the camel case naming

<!--START_SECTION:contributions-->
*Counts verified 2026-11-02 against the GitHub Search API (`is:pr is:merged author:Linux2010`).*
<!--END_SECTION:contributions-->

---

### 🧰 Stack

<details>
<summary><b>Agents & AI</b> — OpenClaw · Claude Code · A2A protocol · Qwen / GLM / Kimi / DeepSeek via 阿里云百炼</summary>

Tool-call recovery, session and compaction semantics, multi-provider routing, agent orchestration and
scheduled task loops. Most of my time goes to the failure modes: what happens when a tool call defers,
a history row is synthetic, or a cron job outlives the session it was bound to.
</details>

<details>
<summary><b>Languages</b> — TypeScript · Python · Java · Bash</summary>

TypeScript for the agent runtime, Python for protocol and SDK work, Java for data plumbing.
</details>

<details>
<summary><b>Infrastructure</b> — Docker · GitHub Actions · Telegram Bot API · Matrix · SQLite · LanceDB</summary>

Multi-container agent deployments, CI-driven contribution loops, embedding-backed note retrieval.
</details>

---

### 📖 Notes

[ai-wiki](https://github.com/Linux2010/ai-wiki) — working notes on agent infrastructure and contribution
playbooks. Small on purpose; it's where I write down what broke.

---

### 📬 Reach me

- ✉️ [linuxmorebetter@gmail.com](mailto:linuxmorebetter@gmail.com) ← primary
- 💬 Telegram [@worldhello321](https://t.me/worldhello321)
- 中文沟通请直接邮件或 Telegram · 上海

<sub>Currently: making Orion handle long-horizon refactors without losing the thread.</sub>

# `.agent/docs` 索引

| 文档 | 作用 |
|---|---|
| [github-profile-research.md](./github-profile-research.md) | 调研：38 个真实大V 档案的模式频率与反面教材 · 官方渲染器能力边界（哪些标签会被静默剥离）· 2026 生态工具可用性现状 · 可复用 URL 模板 · 6 条设计准则 |
| [github-profile-audit.md](./github-profile-audit.md) | 现状评估：账号事实底账（全部为 2026-11-02 API 实测）· 9 条按严重度排序的问题清单（P0×4 / P1×3 / P2×2）· 对照评分表 |
| [github-profile-optimization-plan.md](./github-profile-optimization-plan.md) | 优化方案：路线选择（A/B/C 取舍）· 新版信息架构 · 4 阶段执行清单 · 明确不做清单 · 可执行验收标准 · 风险表 |
| [assets/profile-readme-v2.md](./assets/profile-readme-v2.md) | 可直接替换 `README.md` 的草案（含实测 PR 链接与数字口径说明） |
| [assets/profile-metrics-workflow.yml](./assets/profile-metrics-workflow.yml) | 阶段 3 保鲜工作流草案（月度重算贡献台账，仅用内置 `GITHUB_TOKEN`） |

一句话结论：**主页的问题不是不够花哨，而是信息错配** —— 现有 1,964 字节在介绍别人的 4 个仓库，而 267 个已合并 PR、亲手创建的 `orion-agents`、自研的 `orion-code` 全部没有出现在主页上；同时档案区 bio 还在用三年前的 `Java | Python | Bigdata` 叙事往反方向解释这个人。

> 数据快照日期 2026-11-02。PR 计数会随时间变化，口径统一为 `is:pr is:merged author:Linux2010`。

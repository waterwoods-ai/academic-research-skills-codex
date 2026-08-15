# Security Track — 安装与使用（Codex 版）

> 日常使用手册(全流程场景速查 + 研究闭环 S0–S8):[`HowTo.md`](../../HowTo.md)

> 本 fork 在上游 ARS-Codex（`academic-research-suite`）之上增加第二个 skill
> `security-track`：面向安全顶会（四大 + tier-2）研究的覆盖层，校准方向为
> CPS / IoT / AI 安全。上游文件零改动（codex 插件清单 `"skills": "./skills/"`
> 为目录自动发现，新 skill 放入即注册）。`main` 保持 upstream 纯净镜像，
> 全部定制在 `dev` 分支（GitHub 默认分支已设为 `dev`）。

## 这个 skill 包含什么

| 文件 | 内容 |
|---|---|
| `SKILL.md` | 触发条件 + 对 stock 套件的覆盖规则（引用格式、论文结构、审稿模拟、双盲、截稿铁律） |
| `references/big4_venue_profiles.md` | 四大会档案（经官方 CFP 逐条核实，2023–2027 各版） |
| `references/security_paper_conventions.md` | 安全论文行文规范：Threat Model 章、评估门槛、责任披露、匿名化清单 |
| `references/security_reviewer_personas.md` | 5 人安全审稿面板 + 八条标准拒稿锚点 |
| `references/major_revision_playbook.md` | 四大会多轮评审实战手册：rebuttal / revision / re-review + 模式映射 |
| `references/perspective_retrieval_protocol.md` | 视角驱动检索协议（STORM 检索侧机制改造，opt-in） |
| `references/conference_ranking_2025.json` | 22 会 CIF 排名快照（每年更新） |
| `references/deadlines_current.md` | 截稿日历（**生成文件，勿手改**） |
| `scripts/fetch_deadlines.py` | 截稿日历拉取脚本（每次 `git sync-upstream` 自动执行，同步到 plugins/ 镜像） |
| `contracts/reviewer/security_full.json` | 安全顶会版 sprint contract（盲态预提交标尺；Schema 13.2 验证通过） |
| `references/research_loop_protocol.md` | 研究闭环协议 S0–S8：gap→课题→方法（novelty+contribution）→证伪实验→执行→有界改进→论文 |

本 skill 在仓库中有两份拷贝：`skills/security-track/`（源，直装路径）与
`plugins/ars-codex/skills/security-track/`（插件物化镜像）。修改源后由
`git sync-upstream` 别名同步日历；其余文件改动需手动 `cp` 到镜像。

## 安装（Codex）

插件方式（推荐，一次装齐主套件 + security-track）：

```bash
# 若装过 upstream 的 ars-codex，先移除
codex plugin marketplace add waterwoods-ai/academic-research-skills-codex --ref dev
codex plugin add ars-codex@ars-codex
```

Codex Desktop 图形界面：Plugins → 添加仓库
`https://github.com/waterwoods-ai/academic-research-skills-codex.git`
（默认分支即 `dev`）→ 安装 **ARS-Codex**。

直装 skill 方式（注意要装两个）：

```bash
python3 "$HOME/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo waterwoods-ai/academic-research-skills-codex --ref dev \
  --path skills/academic-research-suite --method git

python3 "$HOME/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py" \
  --repo waterwoods-ai/academic-research-skills-codex --ref dev \
  --path skills/security-track --method git
```

装完**开新 Codex 会话**生效。更新：`codex plugin marketplace upgrade ars-codex`
后重新 `codex plugin add`；直装方式先 `rm -rf` 旧目录再重跑 installer。

验证：问一句「NDSS 的 Major Revision 流程是什么」，agent 应去读
`major_revision_playbook.md` 而非凭记忆作答。

## 使用

> **关于斜杠命令**：Codex CLI 0.147+ 已移除自定义斜杠命令通道（custom
> prompts 机制被废弃且不再加载，插件也不携带命令），`/ars-*` 是 Claude Code
> 专属形态。在 Codex 中用下面两种等价方式调用：
>
> 1. `$academic-research-suite ars-reviewer <论文>` —— 输入 `$` 后自动补全
> 2. 直接发裸别名文本：`ars-reviewer <论文>`（skill 触发描述已注册全部
>    ars-* 别名，无斜杠、不经过命令弹窗，最省事）

**自动激活**：论文任务涉及安全会议即触发（会议名、threat model、CPS/IoT/
AI security 等触发词）。也可显式调用 `$security-track`。用户为安全研究方向
时，所有论文任务默认按安全会议处理；明确说明非安全研究则回退 stock 行为。

| 你要做的事 | overlay 提供 |
|---|---|
| 选会 / 投稿规划 | venue 档案 + 实时截稿日历 |
| 文献综述（说「确保覆盖全面」触发视角检索协议） | 6 透镜查询扩展 + 未用检索追问 |
| 写作 / 大纲 | 安全论文结构、numeric 引用、双盲清单 |
| 模拟审稿 | 5 人安全面板 + 目标会议的准确判定词汇 |
| 审稿意见响应 / rebuttal 检查 | 按 venue+判定档的 Roadmap、响应包结构与硬规则审计 |

**截稿日期铁律**：日期只从 `references/deadlines_current.md` 引用；超过 7 天
先运行 `python3 skills/security-track/scripts/fetch_deadlines.py`；拉取失败时
明确说明日历过期，绝不凭模型记忆报日期。

## 维护（fork 工作流）

```bash
git sync-upstream   # 拉 upstream → ff-only 更新 main → 推送 → 合并进 dev → 刷新日历（含 plugins/ 镜像）
```

定制永远走加法：新文件放本目录 + 同步到 plugins/ 镜像；不改上游文件。
venue 档案钉结构性事实；页数、轮次会漂移，临近投稿以当年 CFP 为准。

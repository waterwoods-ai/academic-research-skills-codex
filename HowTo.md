# HowTo — 在 Codex 中使用(安全顶会研究全流程)

> 安装见 [`skills/security-track/README.md`](skills/security-track/README.md)。本文假设
> 插件已装好(`codex plugin add ars-codex@ars-codex`)、已开新会话。`/skills` 里应有
> 两个条目:**ARS-Codex**(主套件)+ **Security Track**(安全覆盖层)。
> 全程默认:你的论文任务面向安全顶会(四大 + tier-2),覆盖层自动叠加。

## Codex 的调用形态(与 Claude Code 的关键差异)

Codex 0.147+ 没有第三方斜杠命令。两种等价调用,**都不要加斜杠**:

```text
ars-reviewer — target venue: NDSS 2027 ...        ← 裸别名,最省事(推荐)
$academic-research-suite ars-reviewer ...          ← $ 调用,输入 $ 后自动补全
```

输入 `/ars-...` 会撞上 Codex 的命令弹窗然后匹配失败——这是平台机制,不是安装问题。

## 核心习惯(先记这三条)

1. **涉及投稿/审稿的请求,永远写明目标会议**:`target venue: NDSS 2027`。
   判定词汇、Phase-0 检查项、页数预算都依赖它。
2. **截稿日期永远不要接受凭记忆的回答**——agent 被规定只从
   `references/deadlines_current.md` 引用,超 7 天会先刷新。
3. **审稿第二轮起用 re-review + 冻结清单**,不要重新跑全量评审(防死循环,见下)。

## 日常场景速查(全部为裸别名/自然语言)

| 场景 | 输入 | 背后生效的机制 |
|---|---|---|
| 选会/投稿规划 | 直接问「帮我选会并做投稿规划:<课题>」 | venue 档案 + 实时截稿日历 |
| 文献综述 | `ars-lit-review <主题>,确保覆盖全面` | 6 透镜视角检索 + 未用检索追问 |
| 大纲 | `ars-outline — target venue: ...` | 安全论文结构(Threat Model / Ethics 章) |
| 全文写作 | `ars-full — target venue: ...` | numeric 引用、双盲写法、页数预算 |
| 模拟审稿 | `ars-reviewer — target venue: ...` + 论文(粘贴或给文件路径) | Phase-0 合规 → 安全合同预提交 → 5 人安全面板 → 会议判定词汇 |
| 修改后复审 | `ars-reviewer re-review` + **粘贴上轮判定原文** | 只判任务清单、禁止新异议、二元收敛 |
| 收到真实审稿意见 | `ars-revision-coach` + decision letter | 按 venue+判定档的 Roadmap + 响应包 + 倒排时间表 |
| 检查 rebuttal 草稿 | `ars-rebuttal-audit` + 意见 + 草稿 | venue 硬规则审计(S&P 500 词等) |
| 实验计划/统计解读 | 「为这个方法设计实验/解读这组结果」 | experiment-agent 工作流 + 闭环协议 S4/S5 |

提示:在论文/代码所在目录启动 `codex`,直接给文件路径(`./paper.tex`),
不必粘贴全文。

## 完整研究闭环(S0–S8)

协议:`skills/security-track/references/research_loop_protocol.md`。按所处阶段直接说:

```text
S0  「验证这个课题是否可做:<主题>」            → go/no-go(饱和/错框/可行)
S1  ars-lit-review <主题>,确保覆盖全面         → gap 登记(每个 gap 带检索证据)
S2  「基于这些 gap 延伸课题」                   → RQ 卡片(威胁模型草图+会议适配+可行性)
S3  「评估我的方法的 novelty 和 contribution」  → Contribution Card(search-bounded 判定)
    或「为 RQ-2 提出新方法」
S4  「为这个方法设计验证实验」                  → 按论文类型的反驳表 + 冻结成功标准
S5  「实现并运行实验」                          → Codex 写代码+跑实验,溯源台账记录
S6  「结果不达标,改进方法」                    → 有界循环:每轮一个针对性修改,≤3 轮强制人工检查点
S7  ars-reviewer(方法+台账包)                 → 投稿前对抗压测
S8  ars-full → Phase-0 → ars-reviewer → 投稿    → 多轮评审生命周期按 playbook
```

Codex 的独有优势在 **S5**:写代码、跑实验、读日志本来就是它的主场——
台账规则(数字只来自执行日志、负结果必须记录)在这里落地最自然。

## 审稿死循环的正确解法(重要)

第一轮 `ars-reviewer` 拿到判定后,**保存输出**。此后每一轮:

```text
ars-reviewer re-review — target venue: NDSS 2027
上轮判定与任务清单(原文):<粘贴>
修改说明:T1 → §4.2 增加 adaptive 实验;T2 → ...
修改稿:<全文或文件路径>
```

规则已由机制冻结:只对清单逐项判 RESOLVED/NOT、禁止对旧文本提新异议、
判定收敛即终止。如果它违规冒出新问题,引用规则让它重来。

## 首次使用的四点验证

跑一次 `ars-reviewer — target venue: NDSS 2027` + 任一篇安全论文,确认:

1. 质量评审**之前**出现 Phase-0 合规表(P0-1~P0-7 PASS/FAIL)
2. 预提交阶段出现 `threat_model_soundness` 等安全维度名(出现 `methodology_rigor` = 合同没加载)
3. 面板 = PC Chair + CPS + IoT + Adversarial-ML + 威胁模型质疑者
4. 判定只用 NDSS 四档词汇(换 S&P 目标应只剩 Accept/Reject)

预期管理:Codex 版是单 skill 内联模拟多 agent 流程,预提交的"盲态"隔离弱于
Claude Code 版的多次独立调用。投稿前最后一轮高利害评审,建议用 Claude Code 版跑。

## 维护

```bash
codex plugin marketplace upgrade ars-codex && codex plugin add ars-codex@ars-codex   # 更新插件
git sync-upstream    # 同步上游 + 刷新截稿日历(在仓库目录内执行,含 plugins/ 镜像)
```

- 排名快照(`conference_ranking_2025.json`)每年随源更新一次
- 明确不是安全研究的任务,说一句即可回退 stock 行为

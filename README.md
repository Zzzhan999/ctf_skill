# CTF-AI-Skill-Pack

> 证据驱动的 CTF 解题 Skill 套件：一个统一「大脑」+ 八个专项 playbook。
> 不是 100 个 Skill 的堆叠，而是一条**可证伪的推理链**。

---

## 这是什么

CTF-AI-Skill-Pack 是面向 CTF 学习者与网络安全初学者的 Agent Skill 套件。它的核心不是「知识百科」，而是**结构化推理**：

```text
题目
  → Evidence（证据提取）
  → Unknown（缺失识别）
  → Hypotheses（多假设）
  → Priority（三维优先级排序）
  → Current Best Direction
  → Smart Next Step（目标 + 成功/失败/模糊信号）
  → 用户在授权环境中验证
  → Result Interpretation
  → Hypothesis Update / Elimination
  → 新的 Next Step → 解题 → Writeup → 复盘
```

**与「把安全知识整理一遍的 prompt」的根本区别**：每个判断都挂在可追溯的证据链上，每个方向都可被证伪，每次卡住都知道下一步该看什么。

---

## 架构：一个大脑 + 八个 playbook

```
                 ┌─────────────────────────────────────┐
                 │   00-core / CTF-Analyzer-V2.0（大脑）     │
                 │   证据驱动推理 · 假设生命周期         │
                 │   L1/L2/L3 渐进提示 · Session State  │
                 └───────────────┬─────────────────────┘
                                 │ 判定题型后调度
        ┌────────────┬───────────┼───────────┬────────────┐
        ▼            ▼           ▼           ▼            ▼
   01-web       02-crypto    03-pwn    04-reverse    05-forensics
        │            │           │           │            │
        └────────────┴───────────┴───────────┴────────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        ▼                        ▼                        ▼
   06-misc                 07-linux               08-writeup
```

**分工边界（严格）：**

| 职责 | 归属 |
|---|---|
| 题型识别、Evidence/Inference/Unknown 区分、假设与优先级、置信度、L1/L2/L3、Session State、反幻觉 | **CTF-Analyzer-V2.0（大脑）** |
| 具体漏洞/算法/取证/提权的技术细节、payload、工具命令、决策树 | **各专项 playbook** |
| 面向用户的最终输出与 Smart Next Step | **CTF-Analyzer-V2.0** |

**专项 Skill 不重新实现推理系统。** 它们被大脑调度，返回「专项分析结果」，由大脑汇总。这避免了多套并行推理逻辑互相冲突。

---

## 目录结构

```
ctf-skill/
│
├── skills/
│   ├── 00-core/
│   │   └── CTF-Analyzer-V2.0/SKILL.md      ← 大脑（V2.0，24 章核心机制）
│   │
│   ├── 01-web/
│   │   ├── Web-Analyzer/SKILL.md            ← Web 通用 playbook + 通用语料
│   │   ├── SQLi/SKILL.md                    ← SQL 注入专项
│   │   ├── XSS/SKILL.md                     ← 跨站脚本专项
│   │   └── SSRF/SKILL.md                    ← 服务端请求伪造专项
│   │
│   ├── 02-crypto/Crypto-Analyzer/SKILL.md
│   ├── 03-pwn/Pwn-Analyzer/SKILL.md
│   ├── 04-reverse/Reverse-Analyzer/SKILL.md
│   ├── 05-forensics/Forensics-Analyzer/SKILL.md
│   ├── 06-misc/Misc-Analyzer/SKILL.md
│   ├── 07-linux/Linux-Security/SKILL.md
│   │
│   └── 08-writeup/
│       ├── Writeup-Generator/SKILL.md       ← Writeup 生成
│       └── CTF-Review/SKILL.md              ← 复盘专项
│
├── templates/                           ← 状态 / 输出 / Writeup 模板
├── examples/                            ← 13 个完整场景（覆盖 10 项判据）
├── tests/                               ← 五层测试方案 + check_skills.py 检查器
├── README.md
└── LICENSES.md
```

**为什么 Web 拆成 4 个：** Web 的漏洞面最宽，单一 playbook 会让决策树过深。SQLi / XSS / SSRF 是 CTF 中最高频且技术体系自洽的三个方向，各自独立成 Skill；Web-Analyzer 保留通用流程（信任边界映射、fuzzer 工具链、认证 / 授权 / 上传 / 反序列化等其余方向）。

**为什么 Writeup 拆成 2 个：** Writeup 回答「怎么解的」，复盘回答「为什么没想到 / 学到了什么」。二者产物形态不同（题解文档 vs 认知提升文档），触发指令也不同（「写 Writeup」vs「复盘」）。

---

## 安装

```bash
# 推荐：自带展平与验证的安装器
python3 tests/install.py ~/.claude/skills

# 先模拟看会装什么，不写盘：
python3 tests/install.py ~/.claude/skills --dry-run
```

> **⚠️ 不要直接 `cp -r skills/* ~/.claude/skills/`。** 本 Pack 仓库是分类分组布局
> （`skills/01-web/SQLi/SKILL.md`），但 Skill 加载器只识别
> `<skills-root>/<技能名>/SKILL.md` **直接子目录**。直接复制会装成三层路径，
> 加载器一个技能都发现不了。`install.py` 把 13 个技能目录展平复制，并验证安装结果
> 恰好可被发现 13 个、无深层嵌套泄漏。

安装后重启 Claude Code 会话即可生效。

**发布前自检（必须通过）：**

```bash
python3 tests/run_checks.py
```

一键跑完三个自研检查器（无第三方依赖）：

| 检查器 | 层级 | 验证什么 |
|---|---|---|
| `check_skills.py` | L1 格式合规 | 恰好 13 个可发现技能 / 大脑 24 章完整 / 三章节齐备 / 无悬空引用 / trigger 非空 |
| `check_content.py` | L1 危险内容 | 无硬编码凭据（不可豁免）/ 破坏性技术必须带授权横幅 / 散文无未标注 eval·exec |
| `check_dispatch.py` | L2 调度契约 | 专项 Skill 只经大脑调度 / 大脑 trigger 类型中立 / **区分词无共享** / 路由覆盖分级 |
| `check_contract.py` | L4 协作契约 | 输出格式不含题型判断与 Level 宣布 / 不改 Session State / 不反向调用大脑 |

安装后 Skill 加载器应发现**恰好 13 个技能**。若发现更多，说明第三方语料的 `SKILL.md` 未正确降级为 `PLAYBOOK.md`——运行检查器定位。任一检查器退出码非 0 即不可发布。

> **仍未自动化的项**（见 `tests/test-plan.md`）：L2 语义路由准确性、
> L3 推理行为（13 场景）、L5 跨实例稳定性盲测。这些需人工/模型/多实例执行。

---

## 日常使用

安装完成后直接对话即可，无需记忆任何命令：

```text
用户：这道题给了 ZmxhZ3tjcnlwdG9faXNfZnVufQ==，提示 Decode me
→ 大脑判定 Crypto，调度 02-crypto/Crypto-Analyzer，输出 Level 1（方向，不给答案）

用户：继续
→ Level 2（补证据链 + Priority 三维明细 + 可执行命令）

用户：完整解析
→ Level 3（完整解法，仍标注哪些是已验证、哪些是推断）

用户：我输入单引号后响应毫无变化，而且我找到源码用的是 PDO 预处理
→ 大脑触发假设淘汰：H1 SQL Injection 淘汰，重新排序，生成新的 Next Step

用户：帮我写 Writeup
→ 08-writeup/Writeup-Generator（读 Session State，标注缺失证据）

用户：复盘
→ 08-writeup/CTF-Review（复盘的核心是「为什么没想到」，不是流水账）
```

**交互指令速查：** `继续`（等级 +1）｜ `Level 2/3`（直跳）｜ `为什么？`（补原理不升级）｜ `我卡住了`（卡点诊断）｜ `写 Writeup` ｜ `复盘` ｜ `重头分析`（重置状态）

---

## 调度链路

```text
用户输入
  → 00-core/CTF-Analyzer-V2.0（唯一的大脑）
      ├─ 识别题型；Evidence / Inference / Unknown 三分
      ├─ 建立假设、计算 Priority、选择 Current Best Direction
      ├─ 判定方向后，调度对应专项 Skill（Web 方向先经 Web-Analyzer 二次分发）
      │     → 专项 Skill 返回「专项分析结果」（技术判定 + 验证步骤，不含题型判断）
      └─ 汇总为面向用户的输出 + Smart Next Step
  → 用户在授权环境中执行验证
  → Result Interpretation → 假设淘汰 / 升降级 → 新的 Next Step
```

**关键纪律：** 专项 Skill 只被大脑调度，不反向调用大脑，不输出题型判断，不维护 Session State，不宣布 Level 等级。整套 Pack 只有一套推理系统，避免多套逻辑互相冲突。

---

## 质量与合规

- **发布门禁：** `tests/run_checks.py` 退出码 0（L1 格式 + L2 调度契约 + L4 协作契约，三个自研检查器）
- **测试方案：** `tests/test-plan.md`（L1 格式合规 / L2 调度准确性 / L3 推理行为 / L4 协作契约 / L5 跨实例稳定性）
- **行为基线：** `examples/examples.md`（13 个完整场景，覆盖 10 项判据，含授权边界场景）
- **授权边界：** 所有操作仅限 CTF / 靶场 / 本地实验 / 明确授权环境。未授权真实目标不调度专项 Skill，转为原理讲解与合法靶场替代方案
- **第三方语料：** 全部 MIT 授权，已降级为 `PLAYBOOK.md` 不参与技能发现，来源与归属见 `LICENSES.md`

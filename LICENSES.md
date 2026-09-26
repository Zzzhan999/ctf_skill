# LICENSES —— 第三方素材来源与许可声明

> 本文件严格记录 CTF-AI-Skill-Pack 中所有第三方素材的**来源、作者、许可证、使用方式与修改情况**。
> 所有第三方内容均为 **MIT 许可**，允许商业使用，但**必须保留原作者署名与许可声明**。
> 本 Pack 的整理产物（统一格式的 SKILL.md、README、模板、测试方案）为原创部分，同样以 MIT 许可发布。

---

## 一、许可总览

| 项目 | 作者 | 许可证 | 商业使用 | 本 Pack 中的处理 |
|---|---|---|---|---|
| ctf-skills | Lukasz Jagiello | MIT | ✅ 允许 | 参考语料原样保留 + 统一格式重写入口 |
| hack-skills | VillanCh | MIT | ✅ 允许 | 精选 44 个 playbook 原样保留 |
| cybersecurity-skills | Bri Russell | MIT | ✅ 允许 | 仅采纳 2 个（disk-forensics、secrets-audit） |
| skill-forge | Timur Zaynullin | MIT | ✅ 允许 | 仅复用工具设计思路，不复制代码 |
| claude-cybersecurity-skills | Paul Amerigo Jr. II Pajo | MIT | ✅ 允许 | **不采用**（见第三节） |
| security-skills | Theft Studio | MIT | ✅ 允许 | **不采用**（见第三节） |

**结论：本 Pack 不含任何「许可证不明确」或「不允许商业使用」的第三方内容。** 因此不存在单独的「非商业版本」分拆需求；下文第三节明确记录了主动排除的项目及理由。

---

## 二、采用的素材明细

### 2.1 ctf-skills（Lukasz Jagiello，MIT）

**原始项目：** https://github.com/ljagiello/ctf-skills（`npx skills add ljagiello/ctf-skills`）

**使用方式：** 参考语料 `.md` 文件**原样复制**到对应分类目录，文件名与内容未修改（仅在个别处为消解文件名冲突而重命名，见「修改情况」）。每个分类目录的 `SKILL.md` 由本 Pack **重新编写**，为原语料提供统一入口与索引。

**修改情况（目录调整后的最终路径）：**
- `ctf-web/*.md`（22 篇 + `scripts/`）→ `skills/01-web/Web-Analyzer/`，原样
- `ctf-web/sql-injection.md` → `skills/01-web/SQLi/`（随 SQLi 专项拆分）
- `ctf-web/client-side*.md`、`node-and-prototype.md` → `skills/01-web/XSS/`（随 XSS 专项拆分）
- `ctf-crypto/*.md`（19 篇）→ `skills/02-crypto/Crypto-Analyzer/`，原样
- `ctf-pwn/*.md`（17 篇 + `scripts/`）→ `skills/03-pwn/Pwn-Analyzer/`，原样
- `ctf-reverse/*.md`（19 篇）→ `skills/04-reverse/Reverse-Analyzer/`，原样
- `ctf-forensics/*.md`（14 篇）→ `skills/05-forensics/Forensics-Analyzer/`，原样
- `ctf-misc/*.md`（12 篇）→ `skills/06-misc/Misc-Analyzer/`，原样
- `ctf-osint/*.md`（3 篇）→ `skills/06-misc/Misc-Analyzer/osint/`，**移动到子目录**（避免 SKILL.md 命名冲突）
- `ctf-ai-ml/*.md`（3 篇）→ `skills/06-misc/Misc-Analyzer/ai-ml/`，**移动到子目录**（同上）
- `ctf-malware/*.md`（3 篇）→ `skills/04-reverse/Reverse-Analyzer/`，其中 `SKILL.md` **重命名为 `malware-analysis.md`**（避免与 reverse 的 SKILL.md 冲突）
- `ctf-writeup/SKILL.md` → `skills/08-writeup/Writeup-Generator/ctf-writeup-original.md`，**重命名**（作为参考保留，新入口由本 Pack 编写）
- `scripts/skill_security_auditor.py`、`scripts/install_ctf_tools.sh` 等：**未复制**，仅在 `tests/test-plan.md` 中记录为复用工具

**署名保留：** 原文件内未含逐文件署名头；本 Pack 通过本节记录、各分类 SKILL.md 的「参考语料索引」章节、以及 README 的贡献者列表完成署名。

### 2.2 hack-skills（VillanCh，MIT）

**原始项目：** VillanCh 的 hack-skills 技能库（103 个 playbook）

**使用方式：** 从 103 个中**精选 44 个**与 CTF 直接相关的 playbook，**原样复制**到对应分类目录（含目录结构）。未入选的 59 个（企业向、移动端、云、区块链、以及内部重复项）**未复制**。

**修改情况：** 文件内容与目录结构均未修改，**唯一例外是文件名**：每个 playbook 的 `SKILL.md` 被**重命名为 `PLAYBOOK.md`**。

**原因：** 这些是参考语料而非本 Pack 的可调用技能。若保留 `SKILL.md`，Skill 加载器会把它们当作独立技能发现（共 46 个），与本 Pack 的「13 个统一格式技能 + 统一调度」架构冲突，且会触发 L1 格式合规测试失败（它们没有 `version` / `trigger` 字段，不符合本 Pack 的统一格式）。重命名为 `PLAYBOOK.md` 保留全部内容与目录结构，同时使技能发现面干净。

**分类去向：**

| 最终路径 | 精选的 playbook（44） |
|---|---|
| `skills/01-web/Web-Analyzer/` | `sqli-sql-injection`、`xss-cross-site-scripting`、`ssrf-server-side-request-forgery`、`ssti-server-side-template-injection`、`idor-broken-object-authorization`、`xxe-xml-external-entity`、`cmdi-command-injection`、`deserialization-insecure`、`path-traversal-lfi`、`upload-insecure-files`、`prototype-pollution-advanced`、`waf-bypass-techniques`、`business-logic-vulnerabilities`、`race-condition`、`type-juggling`、`nosql-injection`、`expression-language-injection`、`jndi-injection`、`request-smuggling`（16，另有 3 个移入专项目录：`sqli-sql-injection`→`SQLi/`、`xss-cross-site-scripting`→`XSS/`、`ssrf-server-side-request-forgery`→`SSRF/`） |
| `skills/02-crypto/Crypto-Analyzer/` | `classical-cipher-analysis`、`rsa-attack-techniques`、`hash-attack-techniques`、`symmetric-cipher-attacks`、`lattice-crypto-attacks`（5） |
| `skills/03-pwn/Pwn-Analyzer/` | `stack-overflow-and-rop`、`format-string-exploitation`、`heap-exploitation`、`binary-protection-bypass`、`arbitrary-write-to-rce`、`kernel-exploitation`、`sandbox-escape-techniques`（7） |
| `skills/04-reverse/Reverse-Analyzer/` | `anti-debugging-techniques`、`code-obfuscation-deobfuscation`、`symbolic-execution-tools`、`vm-and-bytecode-reverse`（4） |
| `skills/05-forensics/Forensics-Analyzer/` | `memory-forensics-volatility`、`steganography-techniques`、`traffic-analysis-pcap`（3） |
| `skills/06-misc/Misc-Analyzer/` | `linux-security-bypass`、`tunneling-and-pivoting`、`reverse-shell-techniques`、`unauthorized-access-common-services`（4） |
| `skills/07-linux/Linux-Security/` | `linux-privilege-escalation`、`linux-lateral-movement`（2） |

**去重说明：** 未入选的 6 个内部路由桩（`hack`、`business-logic-vuln`、`auth-sec`、`api-sec`、`file-access-vuln`、`injection-checking`、`recon-for-sec`）与全量 playbook 重复，已排除；IDOR↔BOLA、JWT 三重交叉、sandbox↔container-escape 等重复对中，保留了更深入的一份。

### 2.3 cybersecurity-skills（Bri Russell，MIT）

**原始项目：** https://github.com/briiirussell/cybersecurity-skills（29 个 skill）

**使用方式：** 仅采纳 **2 个**，提取为单文件参考文档：

| 原始路径 | 本 Pack 路径 | 修改情况 |
|---|---|---|
| `skills/disk-forensics/SKILL.md` | `skills/05-forensics/Forensics-Analyzer/disk-forensics-governance.md` | 内容未改，**重命名**以区分命名空间 |
| `skills/secrets-audit/SKILL.md` | `skills/05-forensics/Forensics-Analyzer/secrets-and-credential-hunting.md` | 内容未改，**重命名** |

**未采纳的 27 个：** 13 个纯企业 GRC/合规（hipaa、pci、privacy、csf、ai-risk 等）与 CTF 无关；11 个 CTF 相关项（web-pentest、recon、osint-recon 等）与 ctf-skills / hack-skills **功能重复**，按「相同功能只保留质量更高的一份」原则排除。

### 2.4 skill-forge（Timur Zaynullin，MIT）

**原始项目：** https://github.com/zztimur/skill-forge（Skill 包 QA/审计框架）

**使用方式：** **不复制任何代码**。仅复用以下**设计思路**，记录于 `tests/test-plan.md`：

- `inspect_skill_package.py` 的静态检查思路 → L1 格式合规测试
- `package_skill.py` 的摘要验证分发思路 → 打包分发
- `check_outcomes.py` 的 precision/recall 基准设计 → L5 稳定性盲测
- `tests/` 的 frontmatter 与 discoverability 路由测试思路 → L1 / L2

**修改情况：** 无文件级复用，故无修改。

### 2.5 CTF-Analyzer-V2.0（本 Pack 用户原创）

**性质：** 由 Pack 所有者开发的原创 Skill，**不是第三方素材**。

**使用方式：** 原样置于 `skills/00-core/CTF-Analyzer-V2.0/SKILL.md`，作为整个 Pack 的统一「大脑」。**其 24 章核心机制在整合过程中未被任何开源内容覆盖、替换或削弱。**

---

## 三、主动排除的项目（明确记录，不强行加入）

### 3.1 security-skills（Theft Studio，MIT）—— 不采用

**理由：**
- 4 个 Skill（dependency-audit / sast-scan / secret-scan / security-preflight）全部是**防御性 SDLC 扫描 runbook**（gitleaks、semgrep、依赖审计），共约 270 行
- 与 CTF 解题场景**基本无关**——没有一个会被用于解 pwn/web/crypto/misc 题
- 唯一微弱重叠（git 历史凭据猎取）已由 cybersecurity-skills 的 `secrets-audit` 覆盖
- **许可证无问题，纯属场景不符**

### 3.2 claude-cybersecurity-skills（Paul Amerigo Jr. II Pajo，MIT）—— 不采用

**理由：**
- **它不是 Claude Skill。** `skills/` 下只有 2 个文档 YAML，没有 Claude Code 可加载的 `SKILL.md`（无 `name`/`description`/`allowed-tools` frontmatter，无 `.claude/skills/` 安装路径）。实际内容是一个 Python 原型包（`cybersec_skills/`）
- **README 与仓库严重不符**：README 称 5 个 production-ready skill，PROJECT_SUMMARY 称 4 个，实际代码只对得上 2 个；MCP 服务与 Claude Code 集成仍是未勾选的 TODO
- 其「授权模式（pentest/ctf/research/defensive）」设计思路有价值，但这一概念已由 CTF-Analyzer-V2.0 的「安全范围与授权边界」（第二十章）实现，无需引入不成熟的代码
- **许可证无问题，纯属质量与成熟度不符**

### 3.3 hack-skills 中未入选的 59 个 playbook —— 不采用

**理由：**
- ~20 个为企业/真实世界向（Active Directory、Windows/macOS 提权、移动端、Kubernetes、区块链、供应链），超出 CTF 场景
- 6 个内部路由桩与全量 playbook 重复
- 其余为重复对中被更深入版本取代者
- **排除目的是避免堆数量、降低维护负担与触发冲突**

---

## 四、原创部分声明

以下文件为本 Pack 整理过程中的**原创产物**（不源自任何第三方项目）：

- `README.md`
- `skills/00-core/CTF-Analyzer-V2.0/SKILL.md`（所有者原创的 CTF-Analyzer-V2.0）
- `skills/01-web` ～ `skills/08-writeup` 各 Skill 目录下**由本 Pack 编写的 `SKILL.md`**（统一格式入口，共 12 个：Web-Analyzer / SQLi / XSS / SSRF / Crypto-Analyzer / Pwn-Analyzer / Reverse-Analyzer / Forensics-Analyzer / Misc-Analyzer / Linux-Security / Writeup-Generator / CTF-Review）
- `templates/session-state-template.md`、`templates/analysis-output-template.md`、`templates/writeup-template.md`
- `tests/test-plan.md`
- `LICENSES.md`

**第三方素材以原样方式分发，未伪装为原创。** 任何二次分发本 Pack 的行为，须同时保留本文件中的署名与许可声明。

---

## 五、MIT 许可证全文

所有第三方素材与本 Pack 原创部分均适用以下许可：

```text
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

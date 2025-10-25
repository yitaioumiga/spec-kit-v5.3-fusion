# command: /speckit.plan
# short: Transcribe an Architecture Doc into the plan.md format
# description: >
# [V5.1 spec-md 劫持：架构转写]
# This command hijacks the default 'plan' behavior.
# It receives a COMPLETED Architecture Document (from Gemini), and transcribes it into the spec-kit native 'plan.md' format.
# @raycast.schemaVersion 1
# @raycast.title speckit.plan
# @raycast.mode fullOutput
# @raycast.argument1 { "type": "text", "placeholder": "粘贴您已完成的 架构设计文档 文本..." }
# @raycast.icon 📐
# @raycast.packageName spec-kit
# @raycast.author Den Delimarsky
# @raycast.authorURL https://github.com/localden
# @raycast.author John Lam
# @raycast.authorURL https://github.com/jflam

#Phase 1: 🧠 AI Task (Transcribing Architecture Doc to plan.md)
# (V5.1 备注：plan 命令不涉及 Shell 脚本，它是一个纯 AI 任务)

**致 AI 助手 (Copilot/Trae)：**

你的任务不是从零开始"创建"技术方案，而是执行一次"**转写 (Transcription)**"。

## 1. 审查 (Review)

你**必须**首先确认以下文件**已被成功创建**：

* `specs/$FEATURE_DIR/spec.md` (你的输入源之一，由上一步 `/specify` 创建)
* `specs/$FEATURE_DIR/plan.md` (此文件**不应存在**，你将要创建它)
* `.specify/templates/plan-template.md` (这是你的格式参考模板)

## 2. 转写 (Transcribe)

### 0. Dry-Run Mode Detection (--dry-run 标志检测)

**如果** 用户输入包含 `--dry-run` 标志, **那么**:

1. **跳过** 下方 Step 3 的文件写入操作 (不创建 `plan.md`)
2. **内部生成** plan 内容 (遵循 `plan-template.md` 结构,但不写入到文件系统)
3. **提取预览** 各章节的关键信息 (见下方 PlanPreview 报告格式)
4. **输出** PlanPreview 报告到控制台 (代替文件创建)
5. **退出** (报告生成后不再执行 Step 3-4)

**否则** (正常模式):
- 继续执行下方的 Step 1-4 (标准转写流程)

#### PlanPreview Report 格式

```markdown
# Dry Run Report: /speckit.plan

**Command**: `/speckit.plan --dry-run`  
**Timestamp**: [当前 ISO 8601 时间]  
**Source**: Architecture document (user input)  
**Target File**: `specs/$FEATURE_DIR/plan.md` (NOT created in dry-run mode)

## Planned Content Preview

### Summary Section
[从架构文档中提取的前3-5行摘要内容]
- **Feature**: [功能名称]
- **Technical Approach**: [技术方法简述]

### Technical Context Section
**Language/Version**: [e.g., Python 3.11 | NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, pytest | NEEDS CLARIFICATION]  
**Storage**: [e.g., PostgreSQL | N/A | NEEDS CLARIFICATION]  
**Testing**: [e.g., pytest | NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server | NEEDS CLARIFICATION]  
**Project Type**: [single/web/mobile | NEEDS CLARIFICATION]  
**Performance Goals**: [e.g., 1000 req/s | NEEDS CLARIFICATION]  
**Constraints**: [e.g., <200ms p95 | NEEDS CLARIFICATION]  
**Scale/Scope**: [e.g., 10k users | NEEDS CLARIFICATION]

[注: 如果架构文档中未明确某个参数,则显示 "NEEDS CLARIFICATION"]

### Constitution Check Section
**Status**: [✅ Compliant | ⚠️ Needs Review | ❌ Violations Detected]  
**Key Findings**:
- [e.g., "Aligns with V5.3 zero-modification constraint"]
- [e.g., "No complexity tracking violations"]
- [e.g., "All gates passed"]

[如果检测到冲突, 列出具体 violations]

### Project Structure Section
**Documentation Structure**:
```text
specs/[###-feature]/
├── plan.md              # Will be created
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/           # Phase 1 output
└── tasks.md             # Phase 2 output
```

**Source Code Structure**: [Single/Web/Mobile - detected from project type]
```text
[显示对应的目录树模板预览, 例如:
# Single project structure:
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/
]
```

## Summary
- **Total sections planned**: 4 (Summary, Technical Context, Constitution Check, Project Structure)
- **Constitution status**: [Compliant | Needs Review | Violations]
- **Estimated plan.md length**: ~X lines (基于架构文档输入预估)
- **Missing parameters**: X (Technical Context 中需要澄清的参数数量)

## Warnings
[Optional] 任何检测到的问题:
- "⚠️ Missing Technical Context parameter: Performance Goals"
- "⚠️ Constitution Check: Needs review for [specific gate]"

## Next Steps
If this preview looks correct, run `/speckit.plan` (without --dry-run) to create plan.md.
```

---

**[V5.1 spec-md 融合指令]**
我们 V5.1 计划的工作流是：战略 AI (Gemini) 生成高质量的"架构设计文档"，而你（Trae）负责将其转写为 `spec-kit` 需要的格式。

**请执行以下操作：**

1.  **读取高质量输入 (架构文档):**
    * 这是用户粘贴的、由 Gemini 生成的完整"架构设计文档"：
    ```
    {{.Argument1}}
    ```

2.  **读取格式模板 (plan-template.md):**
    * **路径:** `.specify/templates/plan-template.md`
    * **操作:** 完整读取此模板，**深刻理解**其所需的 Markdown 结构（例如：`## Tech Stack`, `## Data Model`, `## API Contracts`, `## File Structure` 等关键标题）。

3.  **分析并撰写 (转写核心):**
    * 你的**唯一任务**，是将"高质量输入 (架构文档)"中的信息，**重新组织并填充**到"格式模板 (`plan-template.md`)"所定义的结构中。
    * **禁止：** **严禁**添加任何 `plan-template.md` 中未定义的、你自己的标题或章节。
    * **禁止：** **严禁**对"架构文档"的核心内容进行任何"创造性"的修改、删减或添加。你是在"转写"，不是在"重写"。
    * **执行：** 将转写后的、格式正确的 Markdown 内容，**写入并创建**到 `specs/$FEATURE_DIR/plan.md` 文件中。

4.  **最终确认：**
    * 报告 `plan.md` 创建完成。
    * 声明："我已成功将'架构设计文档'内容转写为 `plan-template.md` 格式。"
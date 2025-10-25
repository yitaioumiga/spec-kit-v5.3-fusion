# command: /speckit.tasks
# short: Generate Autonomous Work Packages (AWP) from plan.md
# description: >
# [V5.1 spec-md 劫持：AWP 生成]
# This command hijacks the default 'tasks' behavior.
# It reads 'plan.md' and uses our custom 'AWP-template.md' to generate a single, comprehensive 'tasks.md' file formatted as a series of AWPs.
# @raycast.schemaVersion 1
# @raycast.title speckit.tasks
# @raycast.mode fullOutput
# @raycast.icon 🎯
# @raycast.packageName spec-kit
# @raycast.author Den Delimarsky
# @raycast.authorURL https://github.com/localden
# @raycast.author John Lam
# @raycast.authorURL https://github.com/jflam

#Phase 1: 🧠 AI Task (Generating AWPs from plan.md)
# (V5.1 备注：tasks 命令不涉及 Shell 脚本，它是一个纯 AI 任务)

**致 AI 助手 (Copilot/Trae)：**

你的任务是执行一次"**工作包分解 (Work Package Breakdown)**"。你将扮演"首席规划师"的角色，将高层级的技术方案（`plan.md`）分解为一系列详细的、可执行的、遵循我们 `spec-md` 体系的**自主工作包 (AWPs)**。

## 0. Dry-Run Mode Detection (--dry-run 标志检测)

**如果** 用户输入包含 `--dry-run` 标志, **那么**:

1. **跳过** 所有文件写入操作 (不要创建或修改 `tasks.md`)
2. **内部执行** AWP 生成逻辑 (分析 plan.md, 设计 AWPs, 按照 AWP-template.md 格式)
3. **输出** DryRunReport 到控制台 (见下方格式)
4. **退出** (报告生成后不再执行后续步骤)

**否则** (正常模式):
- 按常规流程继续, 执行下方的 "1. 审查" 和 "2. AWP 生成" 步骤

### DryRunReport 格式

```markdown
# Dry Run Report: /speckit.tasks

**Command**: `/speckit.tasks --dry-run`  
**Timestamp**: [当前 ISO 8601 时间]  
**Source**: `specs/$FEATURE_DIR/plan.md`  
**Target File**: `specs/$FEATURE_DIR/tasks.md` (NOT created in dry-run mode)

## Planned AWP Structure

### AWP-[PREFIX]-001: [AWP Title from plan.md]
- **Type**: [组件搭建类 | 测试与修复类 | 调查分析类 | 记忆沉淀类]
- **Priority**: [🔴 P1 | 🟡 P2 | 🟢 P3]
- **Description**: [Brief 1-2 sentence summary]
- **Dependencies**: [List prerequisites, or "None"]
- **Estimated Effort**: [e.g., "2-4 hours"]

[重复此结构for每个计划的AWP...]

## Summary
- **Total AWPs planned**: X
- **Breakdown by type**: Y 组件搭建, Z 测试修复, ...
- **Critical path**: AWP-XXX-001 → AWP-XXX-002 → ...
- **Estimated total effort**: [e.g., "1-2 days"]

## Warnings
[Optional] 任何检测到的问题, 例如:
- "Missing plan.md section: API Contracts"
- "AWP dependencies unclear"

## Next Steps
If this preview looks correct, run `/speckit.tasks` (without --dry-run) to create tasks.md.
```

---

## 1. 审查 (Review)

你**必须**首先确认以下文件**已被成功创建**：

* `specs/$FEATURE_DIR/plan.md` (你的**主要输入源**)
* `specs/$FEATURE_DIR/tasks.md` (此文件**不应存在**，你将要创建它)
* **(关键) `.specify/templates/AWP-template.md` (你的**唯一格式指南**)**
* **(忽略) `.specify/templates/tasks-template.md` (你**必须忽略**此文件)**

## 2. AWP 生成 (AWP Generation)

**[V5.1 spec-md 融合指令]**
这是我们 V5.1 计划的核心生成步骤。你必须严格遵循以下指令：

1.  **读取技术蓝图 (plan.md):**
    * **路径:** `specs/$FEATURE_DIR/plan.md`
    * **操作:** 完整读取此文件，理解所有的技术栈、数据模型、API 合约和文件结构。

2.  **(核心) 加载"黄金模板" (AWP-template.md):**
    * **路径:** `.specify/templates/AWP-template.md`
    * **操作:** 完整读取此"黄金模板"，**深刻理解**其复杂的结构，包括：
        * AWP 元数据 (ID, 类型, 模板版本)
        * 1. 战略意图与最高指示 (角色, 使命, 约束)
        * 2. RESEARCH 阶段 (分层记忆查阅, 调查任务)
        * 3. PLAN 阶段 (实施清单, 风险评估)
        * 4. EXECUTE 阶段 (日志配置, 自愈循环, 分类指南)
        * 5. REVIEW 阶段 (最高审查协议, 证据驱动)
    * **禁止：** **严禁**读取或参考 `.specify/templates/tasks-template.md`。它已**被废弃**。

3.  **分析并分解 (AWP 生成核心):**
    * 你的**唯一任务**，是将 `plan.md` 中的高层级任务（例如"实现登录 API"，"创建用户数据模型"），**分解**为一系列符合 `AWP-template.md` 格式的、**完整的**自主工作包。
    * **例如：** `plan.md` 里的"实现登录 API" -> 应该被扩展成一个**完整**的 AWP（包含 R-P-E-R 四个阶段、审查协议、日志记录要求等）。
    * 你需要为每个 AWP 分配一个唯一的 ID（例如 `AWP-DEV-01`, `AWP-DEV-02`...）。

4.  **(关键) 遵守 V4 源码约束 (单文件输出):**
    * **约束：** `spec-kit` 的下游命令 (`/implement`) 期望只读取**一个** `tasks.md` 文件。
    * **执行：** 你**必须**将你生成的所有 AWP（例如 `AWP-DEV-01`, `AWP-DEV-02`...）**全部串联起来，写入并创建到单个** `specs/$FEATURE_DIR/tasks.md` 文件中。
    * **格式：** 使用 Markdown 的 `---` 分隔符来区分文件中的每一个 AWP。

    *(tasks.md 文件内容示例)*
    ```markdown
    <!--
    AWP ID: AWP-DEV-01
    ... (AWP-01 的完整 R-P-E-R 内容) ...
    -->

    ---

    <!--
    AWP ID: AWP-DEV-02
    ... (AWP-02 的完整 R-P-E-R 内容) ...
    -->
    ```

5.  **最终确认：**
    * 报告 `tasks.md` 创建完成。
    * 声明："我已成功将 `plan.md` 分解，并**唯一使用** `AWP-template.md` 格式，生成了包含 [N] 个 AWP 的**单个** `tasks.md` 文件。"
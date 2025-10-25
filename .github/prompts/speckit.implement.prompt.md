# command: /speckit.implement
# short: Execute all AWPs in tasks.md sequentially
# description: >
# [V5.1 spec-md 劫持：AWP 执行器 + 记忆闭环]
# This command hijacks the default 'implement' behavior.
# It reads the AWP-formatted 'tasks.md' file, injects the 'memory-runtime-prompt.md' for each AWP,
# executes them sequentially, and runs a final 'Memory Precipitation' AWP to close the loop.
# @raycast.schemaVersion 1
# @raycast.title speckit.implement
# @raycast.mode fullOutput
# @raycast.icon 🚀
# @raycast.packageName spec-kit
# @raycast.author Den Delimarsky
# @raycast.authorURL https://github.com/localden
# @raycast.author John Lam
# @raycast.authorURL https://github.com/jflam

# (V5.1 备注：implement 命令不涉及 Shell 脚本，它是一个纯 AI 任务)

**致 AI 助手 (Copilot/Trae)：**

你的任务是执行一次"**自主工作包执行 (AWP Execution)**"。你将扮演"**自主工作包执行引擎 (AWP Execution Engine)**"的角色。

你的工作流分为三个阶段：**1. 审查** -> **2. AWP 执行循环** -> **3. 记忆沉淀闭环**。

---

## Phase 1: 审查 (Review)

你**必须**首先确认你的所有"灵魂"文件均已就位：

1.  **`specs/$FEATURE_DIR/tasks.md` (你的"任务列表")**
    * **操作:** 确认此文件存在。你将读取它。

2.  **`.specify/templates/AWP-template.md` (你的"操作手册")**
    * **操作:** 确认此文件存在。你将在"记忆沉淀"阶段使用它。

3.  **`.specify/templates/memory-runtime-prompt.md` (你的"运行时指南")**
    * **操作:** 确认此文件存在。你将**立刻加载它**，并将其作为你执行 AWP 时的"内心独白"和行为准则。

4.  **`.specify/templates/memory-system-readme.md` (你的"记忆库蓝图")**
    * **操作:** 确认此文件存在。你将在"记忆沉淀"阶段使用它。

---

## Phase 2: AWP 执行循环 (AWP Execution Loop)

### 0. Dry-Run Mode Detection (--dry-run 标志检测)

**如果** 用户输入包含 `--dry-run` 标志, **那么**:

1. **跳过** Phase 2 的整个 AWP 执行循环 (不要执行步骤1-5中的任何文件操作)
2. **加载并分析** tasks.md 文件 (读取所有AWPs)
3. **预测** 每个AWP会执行的文件操作 (基于AWP的描述和类型)
4. **输出** FileOperation Report 到控制台 (见下方格式)
5. **跳过** Phase 3 (记忆沉淀,因为没有实际执行代码)
6. **退出** (报告生成后不再执行后续步骤)

**否则** (正常模式):
- 按常规流程继续, 执行下方的步骤1-5 (AWP执行循环)

#### FileOperation Report 格式

```markdown
# Dry Run Report: /speckit.implement

**Command**: `/speckit.implement --dry-run`  
**Timestamp**: [当前 ISO 8601 时间]  
**Source**: `specs/$FEATURE_DIR/tasks.md`  
**Total AWPs**: X  
**Target Files**: (NOT modified in dry-run mode)

## Planned File Operations

### CREATE Operations
- `[CREATE]` [文件路径]
  - **AWP**: [AWP-ID] - [AWP标题]
  - **Description**: [操作描述]
  - **Estimated Lines**: [预估行数]
  - **Dependencies**: [依赖的前置AWPs或文件]

[重复此结构for每个CREATE操作...]

### MODIFY Operations
- `[MODIFY]` [文件路径]
  - **AWP**: [AWP-ID] - [AWP标题]
  - **Description**: [修改内容描述]
  - **Target Section**: [修改的具体部分]
  - **Estimated Changes**: [+X lines | ~Y% of file]

[重复此结构for每个MODIFY操作...]

### DELETE Operations
- `[DELETE]` [文件路径]
  - **AWP**: [AWP-ID] - [AWP标题]
  - **Reason**: [删除原因]

[如果无DELETE操作,写 "- None"]

## Summary
- **Total operations**: X (Y creates, Z modifies, W deletes)
- **Files affected**: N unique files
- **Estimated execution time** (normal mode): ~M minutes
- **Risk level**: [Low | Medium | High] based on operations
  - Low: Only CREATE operations
  - Medium: Some MODIFY operations on non-critical files
  - High: DELETE operations or MODIFY on critical files (config, database)

## Warnings
[Optional] 任何检测到的高风险操作:
- "⚠️ MODIFY operation on critical file: config/production.json"
- "⚠️ DELETE operation: data/user-cache.db"

## Next Steps
If this preview looks correct, run `/speckit.implement` (without --dry-run) to execute all AWPs.
```

---

**[V5.1 spec-md 融合指令]**
你将按顺序执行 `tasks.md` 中的所有 AWP。

1.  **加载"运行时指南" (Load Runtime Guide):**
    * **操作:** **立即完整读取 `.specify/templates/memory-runtime-prompt.md` 的全部内容**。
    * **指令:** 在本 `/implement` 命令的剩余生命周期中，你**必须**严格遵循该"混合记忆模型指令"中的所有协议（三阶段执行、V9 模板、a-h 循环）来执行**每一个** AWP。

2.  **加载"任务文件" (Load Task File):**
    * **操作:** **完整读取 `specs/$FEATURE_DIR/tasks.md` 的全部内容**。

3.  **解析 AWP (Parse AWPs):**
    * **操作:** 按 Markdown 分隔符 `---` 拆分 `tasks.md` 的内容，得到一个 AWP 列表：`[AWP-DEV-01, AWP-DEV-02, AWP-FIX-01, ...]`。

4.  **(核心) 顺序执行循环 (Sequential Execution Loop):**
    * **操作:** 你现在必须**按顺序、逐一**执行列表中的**每一个** AWP。
    * **循环逻辑:**
        ```
        FOR awp in awp_list:
            1. 通知我："即将开始执行 AWP: [awp.ID]..."
            
            2. (关键) 执行 AWP:
               - 你必须严格遵循你已加载的 "memory-runtime-prompt.md" (运行时指南)。
               - 你必须遵循当前 AWP 内部定义的 R-P-E-R 流程。
               - 你必须使用 "memory-runtime-prompt.md" 中的 V9 模板来创建和更新短期记忆文件 (例如 `ai_assistant_memory/work_package_archives/context_[awp.ID].md`)。
               - 你必须严格执行 AWP 内部 "REVIEW" 阶段定义的"最高审查协议"。
               
            3. (关键) 自愈型修复循环:
               - 如果 AWP 执行失败 (未通过其"最高审查协议")，你**必须**遵循 `memory-runtime-prompt.md` 中的"d. 严格自我审查与决策" -> "如果审查失败"的逻辑，**停留在当前 AWP 内**，直到它被修复并通过。
               
            4. 报告 AWP 完成:
               - 当且仅当一个 AWP 通过了它自己的"最高审查协议"后，你才能向我报告：
                 "✅ AWP: [awp.ID] 执行成功。"
               - 然后，你才能进入列表中的**下一个** AWP。
        
        // 循环结束
        ```

5.  **报告开发完成:**
    * **操作:** 当列表中的**所有** AWP 都已成功执行完毕后，向我报告："所有 [N] 个开发/修复 AWP 已全部执行成功。"

---

## Phase 3: (关键) 强制收尾动作 - 记忆沉淀 (Forced Finalizer - Memory Precipitation)

**[V5.1 记忆闭环指令]**
你不能在此停止。开发完成不代表工作流结束。你**必须**立即执行最后一次、自动的"记忆沉淀"任务，以将本次 `implement` 的所有成果（代码、日志、短期记忆）更新到"四位一体"AI 辅助记忆库中。

1.  **通知:**
    * "即将开始执行强制的'记忆沉淀'收尾工作..."

2.  **加载"操作手册" (Load Guides):**
    * **操作:** 你将使用 `AWP-template.md` (作为格式) 和 `memory-system-readme.md` (作为蓝图)。

3.  **(关键) 生成并执行"记忆沉淀 AWP" (Generate & Execute Memory AWP):**
    * **操作:** 你**必须**现在：
        1.  **回顾**你在 `Phase 2` 中执行的所有 AWP 的产出物（最终代码、所有 `logs/` 文件、所有 `work_package_archives/` 中的短期记忆）。
        2.  遵循 `AWP-template.md` 中"**记忆沉淀类 (Memory Synchronization)**"的最佳实践。
        3.  **执行**沉淀任务，即**更新** `ai_assistant_memory/` 目录中的文件（例如：更新 `docs/components/` 下的新组件文档，更新 `logs/README.md` 索引，更新 `.context/progress.md` 等）。

4.  **最终确认 (V5.1 闭环):**
    * **操作:** 报告：
      "✅ '记忆沉淀' AWP 执行完毕。AI 辅助记忆库已与最新代码同步。"
      "✅ V5.1 融合工作流 (`/implement`) 完整闭环。"
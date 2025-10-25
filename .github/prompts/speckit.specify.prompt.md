# command: /speckit.specify
# short: Transcribe a PRD into the spec.md format
# description: >
#   [V5.1 spec-md 劫持：PRD 转写]
#   This command hijacks the default 'specify' behavior.
#   It receives a COMPLETED PRD/MRD (from Gemini), and transcribes it into the spec-kit native 'spec.md' format, based on 'spec-template.md'.
# @raycast.schemaVersion 1
# @raycast.title speckit.specify
# @raycast.mode fullOutput
# @raycast.argument1 { "type": "text", "placeholder": "粘贴您已完成的 PRD/MRD 文本..." }
# @raycast.icon 📝
# @raycast.packageName spec-kit
# @raycast.author Den Delimarsky
# @raycast.authorURL https://github.com/localden
# @raycast.author John Lam
# @raycast.authorURL https://github.com/jflam

# Phase 1: 📜 Shell Script Execution (Initializing Feature Branch)
# This script will:
# 1. Run the standard spec-kit script to create the feature branch.
# 2. Set up the specs/$FEATURE_DIR directory.
# 3. Create the initial specs/$FEATURE_DIR/spec.md file.

#!/bin/bash
set -e

# Load common functions and variables
source "$(dirname "$0")/../../.specify/scripts/common.sh"

# 1. --- [原始逻辑保留] Standard spec-kit script ---
# This script creates the feature branch, sets up $FEATURE_DIR,
# and creates the initial specs/$FEATURE_DIR/spec.md file.
bash "$(dirname "$0")/../../.specify/scripts/create-new-feature.sh" \
  --feature-name "specify" \
  --summary "Define feature specification from PRD" \
  --skip-pr \
  --auto-checkout \
  --allow-empty-description \
  --description "$1"

echo "Feature branch for 'specify' created."
echo "FEATURE_DIR=$FEATURE_DIR"

# Phase 2: 🧠 AI Task (Transcribing PRD to spec.md)

**致 AI 助手 (Copilot/Trae)：**

你的任务不是从零开始"创建"需求，而是执行一次"**转写 (Transcription)**"。

## 1. 审查 (Review)

Shell 脚本已执行完毕。你**必须**首先确认以下文件**已被成功创建**：

* `specs/$FEATURE_DIR/spec.md` (这是你的目标文件)
* `.specify/templates/spec-template.md` (这是你的格式参考模板)

## 2. 转写 (Transcribe)

**[V5.1 spec-md 融合指令]**
我们 V5.1 计划的工作流是：战略 AI (Gemini) 生成高质量的 PRD/MRD，而你（Trae）负责将其转写为 `spec-kit` 需要的格式。

**请执行以下操作：**

1. **读取高质量输入 (PRD/MRD):**
   * 这是用户粘贴的、由 Gemini 生成的完整 PRD/MRD：
   ```
   {{.Argument1}}
   ```

2. **读取格式模板 (spec-template.md):**
   * **路径:** `.specify/templates/spec-template.md`
   * **操作:** 完整读取此模板，**深刻理解**其所需的 Markdown 结构（例如：`## User Stories`, `## Requirements`, `## Out of Scope` 等关键标题）。

3. **分析并撰写 (转写核心):**
   * 你的**唯一任务**，是将"高质量输入 (PRD/MRD)"中的信息，**重新组织并填充**到"格式模板 (`spec-template.md`)"所定义的结构中。
   * **禁止：** **严禁**添加任何 `spec-template.md` 中未定义的、你自己的标题或章节。
   * **禁止：** **严禁**对 PRD/MRD 的核心内容进行任何"创造性"的修改、删减或添加。你是在"转写"，不是在"重写"。
   * **执行：** 将转写后的、格式正确的 Markdown 内容，**写入并覆盖**到 `specs/$FEATURE_DIR/spec.md` 文件中。

4. **最终确认：**
   * 报告 `spec.md` 填充完成。
   * 声明："我已成功将 PRD/MRD 内容转写为 `spec-template.md` 格式。"
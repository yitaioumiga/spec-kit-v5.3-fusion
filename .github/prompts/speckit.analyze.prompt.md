---
description: Perform a non-destructive cross-artifact consistency and quality analysis across spec.md, plan.md, and tasks.md after task generation.
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Goal

Identify inconsistencies, duplications, ambiguities, and underspecified items across the three core artifacts (`spec.md`, `plan.md`, `tasks.md`) before implementation. This command MUST run only after `/speckit.tasks` has successfully produced a complete `tasks.md`.

## Operating Constraints

**STRICTLY READ-ONLY**: Do **not** modify any files. Output a structured analysis report. Offer an optional remediation plan (user must explicitly approve before any follow-up editing commands would be invoked manually).

**Constitution Authority**: The project constitution (`.specify/memory/constitution.md`) is **non-negotiable** within this analysis scope. Constitution conflicts are automatically CRITICAL and require adjustment of the spec, plan, or tasks—not dilution, reinterpretation, or silent ignoring of the principle. If a principle itself needs to change, that must occur in a separate, explicit constitution update outside `/speckit.analyze`.

## Execution Steps

### 1. Initialize Analysis Context

Run `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` once from repo root and parse JSON for FEATURE_DIR and AVAILABLE_DOCS. Derive absolute paths:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Abort with an error message if any required file is missing (instruct the user to run missing prerequisite command).
For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

### 1.5. AWP Format Adapter (Spec-Kit V5.3 Compatibility Layer)

**【新增步骤 - 修复 Conflict #1 from AUDIT_REPORT.md】**

**Purpose**: Enable `/speckit.analyze` to work with V5.3's AWP (Autonomous Work Package) format by transcribing AWP structure into a temporary in-memory checklist before applying original consistency checks.

**Execution Logic**:

**Step A: Detect File Format**

After loading TASKS file path, perform format detection:

1. **Read first 50 lines** of TASKS file
2. **Check for AWP markers**:
   - Presence of `## 元数据 (Metadata)` section
   - Presence of `## 战略意图 (Strategic Intent)` section  
   - Presence of `## PHASE 1: RESEARCH` or similar phase headers
   - Presence of `### Task X.Y:` pattern (e.g., `### Task 1.1:`, `### Task 2.3:`)

**If AWP markers detected** (>=3 markers present):
- Set `FORMAT = "AWP"`
- Proceed to **Step B: AWP-to-Checklist Transcription**

**If AWP markers NOT detected**:
- Set `FORMAT = "CHECKLIST"`  
- Skip to original **Section 2: Load Artifacts** (no adaptation needed)

**Step B: AWP-to-Checklist Transcription (Only if FORMAT = "AWP")**

**Goal**: Extract task information from AWP's multi-phase structure and convert to flat checklist format for compatibility with original analysis logic.

**Transcription Rules**:

1. **Ignore metadata sections**: Skip `元数据`, `战略意图`, etc. (not task content)

2. **Extract tasks from PHASE sections**:
   - Search for `## PHASE N:` headers (where N = 1, 2, 3, 4)
   - Under each PHASE, find `### Task X.Y:` subsections
   - Example AWP task:
     ```markdown
     ### Task 2.1: 设计数据库 schema
     - 设计 users 表结构
     - 定义密码哈希策略
     - 产出物: `.context/db-schema.md`
     ```

3. **Convert to checklist format**:
   - Transform `### Task X.Y: [Description]` → `- [ ] TX.Y: [Description]`
   - Preserve task description verbatim
   - Extract deliverables from `产出物:` or similar patterns
   - Example transcription:
     ```markdown
     - [ ] T2.1: 设计数据库 schema (Deliverable: .context/db-schema.md)
     ```

4. **Preserve phase grouping** (optional for phase-level analysis):
   - Add phase comments to transcribed checklist:
     ```markdown
     # Phase 1: RESEARCH
     - [ ] T1.1: 技术调研 (Deliverable: logs/auth-research.md)
     - [ ] T1.2: 分析现有代码 (Deliverable: logs/code-analysis.md)
     
     # Phase 2: PLAN
     - [ ] T2.1: 设计数据库 schema (Deliverable: .context/db-schema.md)
     ```

5. **Handle parallel markers**:
   - If AWP task description contains keywords like "可并行" or "parallel", add `[P]` marker
   - Example: `- [ ] T3.1: 创建 User 模型 [P]`

**Step C: Store Transcribed Checklist in Memory**

- Store transcribed checklist as `TASKS_TRANSCRIBED` (in-memory variable)
- Use `TASKS_TRANSCRIBED` instead of raw TASKS file for all subsequent analysis steps
- **DO NOT write transcribed checklist to disk** (this is a read-only analysis)

**Step D: Log Adapter Activation**

Include in analysis report:
```markdown
## AWP Format Adapter

**Detected Format**: AWP (Autonomous Work Package)  
**Transcription Status**: ✅ SUCCESS  
**Transcribed Tasks**: N tasks across M phases  
**Note**: Analysis performed on transcribed checklist; original AWP structure preserved on disk.
```

**Error Handling**:

If AWP format detected but transcription fails:
- Log error in report: `⚠️ AWP transcription failed: [reason]`
- Fallback: Attempt analysis on raw AWP content (expect degraded accuracy)
- Recommendation: Suggest manual checklist generation or AWP format correction

**Preservation of Original Logic**:

After transcription (or if CHECKLIST format detected):
- Continue to **Section 2: Load Artifacts** using `TASKS_TRANSCRIBED` (or original TASKS if no adaptation needed)
- All downstream analysis (duplication, ambiguity, coverage, etc.) operates on transcribed format
- Original AWP file remains unmodified

**Expected Outcomes**:

1. ✅ `/speckit.analyze` can now process V5.3 AWP-formatted tasks
2. ✅ Consistency checks (spec ↔ tasks mapping) work correctly  
3. ✅ No breaking changes to original checklist format support
4. ✅ Clear reporting of adapter activation in analysis output

### 2. Load Artifacts (Progressive Disclosure)

Load only the minimal necessary context from each artifact:

**From spec.md:**

- Overview/Context
- Functional Requirements
- Non-Functional Requirements
- User Stories
- Edge Cases (if present)

**From plan.md:**

- Architecture/stack choices
- Data Model references
- Phases
- Technical constraints

**From tasks.md:**

- Task IDs
- Descriptions
- Phase grouping
- Parallel markers [P]
- Referenced file paths

**From constitution:**

- Load `.specify/memory/constitution.md` for principle validation

### 3. Build Semantic Models

Create internal representations (do not include raw artifacts in output):

- **Requirements inventory**: Each functional + non-functional requirement with a stable key (derive slug based on imperative phrase; e.g., "User can upload file" → `user-can-upload-file`)
- **User story/action inventory**: Discrete user actions with acceptance criteria
- **Task coverage mapping**: Map each task to one or more requirements or stories (inference by keyword / explicit reference patterns like IDs or key phrases)
- **Constitution rule set**: Extract principle names and MUST/SHOULD normative statements

### 4. Detection Passes (Token-Efficient Analysis)

Focus on high-signal findings. Limit to 50 findings total; aggregate remainder in overflow summary.

#### A. Duplication Detection

- Identify near-duplicate requirements
- Mark lower-quality phrasing for consolidation

#### B. Ambiguity Detection

- Flag vague adjectives (fast, scalable, secure, intuitive, robust) lacking measurable criteria
- Flag unresolved placeholders (TODO, TKTK, ???, `<placeholder>`, etc.)

#### C. Underspecification

- Requirements with verbs but missing object or measurable outcome
- User stories missing acceptance criteria alignment
- Tasks referencing files or components not defined in spec/plan

#### D. Constitution Alignment

- Any requirement or plan element conflicting with a MUST principle
- Missing mandated sections or quality gates from constitution

#### E. Coverage Gaps

- Requirements with zero associated tasks
- Tasks with no mapped requirement/story
- Non-functional requirements not reflected in tasks (e.g., performance, security)

#### F. Inconsistency

- Terminology drift (same concept named differently across files)
- Data entities referenced in plan but absent in spec (or vice versa)
- Task ordering contradictions (e.g., integration tasks before foundational setup tasks without dependency note)
- Conflicting requirements (e.g., one requires Next.js while other specifies Vue)

### 5. Severity Assignment

Use this heuristic to prioritize findings:

- **CRITICAL**: Violates constitution MUST, missing core spec artifact, or requirement with zero coverage that blocks baseline functionality
- **HIGH**: Duplicate or conflicting requirement, ambiguous security/performance attribute, untestable acceptance criterion
- **MEDIUM**: Terminology drift, missing non-functional task coverage, underspecified edge case
- **LOW**: Style/wording improvements, minor redundancy not affecting execution order

### 6. Produce Compact Analysis Report

Output a Markdown report (no file writes) with the following structure:

## Specification Analysis Report

| ID | Category | Severity | Location(s) | Summary | Recommendation |
|----|----------|----------|-------------|---------|----------------|
| A1 | Duplication | HIGH | spec.md:L120-134 | Two similar requirements ... | Merge phrasing; keep clearer version |

(Add one row per finding; generate stable IDs prefixed by category initial.)

**Coverage Summary Table:**

| Requirement Key | Has Task? | Task IDs | Notes |
|-----------------|-----------|----------|-------|

**Constitution Alignment Issues:** (if any)

**Unmapped Tasks:** (if any)

**Metrics:**

- Total Requirements
- Total Tasks
- Coverage % (requirements with >=1 task)
- Ambiguity Count
- Duplication Count
- Critical Issues Count

### 7. Provide Next Actions

At end of report, output a concise Next Actions block:

- If CRITICAL issues exist: Recommend resolving before `/speckit.implement`
- If only LOW/MEDIUM: User may proceed, but provide improvement suggestions
- Provide explicit command suggestions: e.g., "Run /speckit.specify with refinement", "Run /speckit.plan to adjust architecture", "Manually edit tasks.md to add coverage for 'performance-metrics'"

### 8. Offer Remediation

Ask the user: "Would you like me to suggest concrete remediation edits for the top N issues?" (Do NOT apply them automatically.)

## Operating Principles

### Context Efficiency

- **Minimal high-signal tokens**: Focus on actionable findings, not exhaustive documentation
- **Progressive disclosure**: Load artifacts incrementally; don't dump all content into analysis
- **Token-efficient output**: Limit findings table to 50 rows; summarize overflow
- **Deterministic results**: Rerunning without changes should produce consistent IDs and counts

### Analysis Guidelines

- **NEVER modify files** (this is read-only analysis)
- **NEVER hallucinate missing sections** (if absent, report them accurately)
- **Prioritize constitution violations** (these are always CRITICAL)
- **Use examples over exhaustive rules** (cite specific instances, not generic patterns)
- **Report zero issues gracefully** (emit success report with coverage statistics)

## Context

$ARGUMENTS

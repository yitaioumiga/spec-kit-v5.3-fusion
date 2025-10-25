# spec-kit V5.3 Fusion

> **A strategic evolution of [spec-kit](https://github.com/mikeyobrien/spec-kit)**: Embedding autonomous work package execution + four-tier memory architecture into specification-driven development.

**spec-kit V5.3 Fusion** is an enhanced version of the original spec-kit framework, designed for AI-augmented software teams. It preserves the core philosophy of specification-first development while introducing **AWP (Autonomous Work Package)** execution engine and a **persistent memory system** for long-term project intelligence.

---

## ⚠️ Important Notice: Known Limitations

**Before using V5.3 Fusion, please be aware:**

| Feature | Status | Details |
|---------|--------|---------|
| `/speckit.analyze` | 🔴 **Incompatible** | The analyze command expects simple checklist format (`tasks.md`), but V5.3 generates AWP format. **Workaround**: Manual review or use AI assistants for consistency checking. |
| `/speckit.clarify` | 🟡 **Untested** | Compatibility with V5.3's "transcription" workflow not verified. May work normally, but requires testing. |
| `/speckit.checklist` | 🟡 **Untested** | May miss AWP-specific quality items. Requires testing with V5.3 projects. |
| Windows Compatibility | 🟡 **Partial** | Some shell commands may need adjustment for Windows (cmd.exe/PowerShell). |

**Why these trade-offs?**  
V5.3 "hijacks" 5 core commands to implement the AWP execution model. We believe the gains (autonomous execution + memory persistence) outweigh the loss of the `analyze` command. See [AUDIT_REPORT.md](docs/AUDIT_REPORT.md) for full conflict analysis.

---

## ✨ What's New in V5.3 Fusion

### 🤖 1. Autonomous Work Package (AWP) System

**Before (Original spec-kit):**
```
User: /speckit.tasks
AI: Generates tasks.md checklist
User: Manually executes each task
```

**After (V5.3 Fusion):**
```
User: /speckit.tasks
AI: Generates AWP with 4-phase workflow (Research-Plan-Execute-Review)
AI: Autonomously executes the entire work package with self-healing loops
```

**Key Features:**
- **Self-contained execution**: Each AWP includes strategic intent, execution plan, and review protocols
- **Phase-gated workflow**: Research → Plan → Execute → Review (must pass gates to proceed)
- **Interactive feedback loop**: AI must submit progress via `mcp-feedback-enhanced` tool for human approval
- **Self-healing**: Built-in error recovery and consistency checks

### 🧠 2. Four-Tier Memory Architecture

V5.3 introduces a persistent memory system to preserve project intelligence across sessions:

```
docs/                      # 📚 Macro Memory (vision, principles)
.context/                  # 🎯 Core Memory (active specs, plans)
logs/                      # 📝 Evidence Memory (execution logs, decisions)
work_package_archives/     # 📦 Historical Memory (completed AWPs)
```

**Benefits:**
- New AI collaborators can onboard by reading memory files
- Avoid repeating past mistakes (captured in logs)
- Trace decisions back to original requirements

### 🔄 3. "Transcription" Workflow (vs. "Creation")

**Original spec-kit**: AI directly creates specs from user input  
**V5.3 Fusion**: Strategic AI receives PRD/architecture → Execution AI transcribes to spec-kit format

**Why?** Separates strategic thinking (what to build) from systematic specification (how to document). Reduces risk of AI "improvising" requirements.

---

## 🚀 Quick Start

### Prerequisites

- VS Code with GitHub Copilot or similar AI assistant
- [Desktop Commander MCP](https://github.com/your-link) (for autonomous execution)
- Basic understanding of specification-driven development

### Installation

1. **Clone this repository:**
   ```bash
   git clone https://github.com/your-username/spec-kit-v5.3-fusion.git
   cd spec-kit-v5.3-fusion
   ```

2. **Copy spec-kit files to your project:**
   ```bash
   # Copy prompts (defines /speckit commands)
   cp -r .github/prompts /path/to/your/project/.github/

   # Copy templates (AWP, memory system)
   cp -r .specify/templates /path/to/your/project/.specify/
   ```

3. **Initialize memory structure:**
   ```bash
   cd /path/to/your/project
   mkdir -p docs .context logs work_package_archives
   ```

4. **Test the installation:**
   In your project's chat interface (e.g., Copilot Chat):
   ```
   /speckit.constitution
   ```
   The AI should respond with constitution initialization.

### Basic Usage

**Typical workflow:**

```
1. /speckit.constitution    # Initialize project principles
2. /speckit.specify          # Transcribe PRD into spec.md
3. /speckit.plan             # Generate architecture plan (plan.md)
4. /speckit.tasks            # Generate AWP for implementation
5. Let AI execute AWP        # AI autonomously executes R-P-E-R phases
6. /speckit.implement        # (Optional) Precipitation loop for memory
```

**Example: Creating a feature spec**

```
You: /speckit.specify

AI: I'm ready to transcribe your PRD/MRD into a spec-kit specification.
Please provide:
1. The PRD document or user stories
2. Key requirements and constraints
3. Any architecture context

You: [Paste your PRD]
Create a user authentication feature with email/password login.

AI: [Generates FEATURE_DIR/spec.md following spec-template.md]
```

---

## 📖 Command Reference

### Available Commands

| Command | Status | Purpose |
|---------|--------|---------|
| `/speckit.constitution` | ✅ **Enhanced** | Initialize project constitution + memory system |
| `/speckit.specify` | ✅ **Enhanced** | Transcribe PRD into spec.md (V5.3 transcription mode) |
| `/speckit.plan` | ✅ **Enhanced** | Generate architecture plan with dry-run validation |
| `/speckit.tasks` | ✅ **Enhanced** | Generate AWP (replaces simple checklist) |
| `/speckit.implement` | ✅ **Enhanced** | Execute implementation + memory precipitation |
| `/speckit.analyze` | 🔴 **Broken** | Consistency checker (incompatible with AWP format) |
| `/speckit.clarify` | 🟡 **Untested** | Interactive requirements clarification |
| `/speckit.checklist` | 🟡 **Untested** | Quality checklist generation |

### Command Details

#### `/speckit.constitution`

**Purpose**: Initialize project principles and memory structure.

**What it does:**
- Creates `CONSTITUTION.md` if not exists
- Initializes memory directories (docs/, .context/, logs/)
- Sets up V5.3 execution protocols

**Example:**
```
/speckit.constitution

AI generates:
- CONSTITUTION.md with project vision and constraints
- docs/MEMORY_SYSTEM.md with memory architecture
- .context/PROJECT_CONTEXT.md (placeholder)
```

#### `/speckit.tasks` (V5.3 Behavior)

**Purpose**: Generate Autonomous Work Package for implementation.

**What it does:**
- Creates `FEATURE_DIR/work_packages/AWP-{ID}.md`
- Includes 4-phase execution plan (R-P-E-R)
- Defines success criteria and review protocols

**Key difference from original:**
- Original: Simple checklist → Manual execution
- V5.3: Full AWP → Autonomous AI execution

**Example AWP structure:**
```markdown
# AWP-FEAT-001: User Authentication

## 元数据 (Metadata)
- 工作包ID: AWP-FEAT-001
- 优先级: P0
- 预计工时: 4-6 hours

## PHASE 1: RESEARCH
- Task 1.1: Review authentication best practices
- Task 1.2: Analyze existing codebase structure

## PHASE 2: PLAN
- Task 2.1: Design database schema
- Task 2.2: Define API endpoints

## PHASE 3: EXECUTE
- Task 3.1: Implement User model
- Task 3.2: Create authentication service
- Task 3.3: Build login/register endpoints

## PHASE 4: REVIEW
- 最高审查协议: All endpoints return correct status codes
- 自我审查清单: [12 items]
```

#### `/speckit.implement` (V5.3 Behavior)

**Purpose**: Execute implementation + capture learnings.

**What it does:**
1. Executes implementation tasks
2. **Memory Precipitation** (Phase 3): Extracts learnings into memory files
3. Closes the knowledge loop

**Example memory precipitation:**
```
AI writes to:
- logs/YYYY-MM-DD-authentication-implementation.md
- .context/AUTH_DECISIONS.md (if new patterns emerge)
```

---

## 🏗️ Project Structure

```
spec-kit-v5.3-fusion/
├── .github/
│   ├── prompts/              # V5.3 Enhanced Commands
│   │   ├── speckit.constitution.prompt.md
│   │   ├── speckit.specify.prompt.md
│   │   ├── speckit.plan.prompt.md
│   │   ├── speckit.tasks.prompt.md
│   │   └── speckit.implement.prompt.md
│   └── ORIGINAL/prompts/     # Original spec-kit backups
│       ├── speckit.analyze.prompt.md (⚠️ incompatible)
│       ├── speckit.clarify.prompt.md
│       └── speckit.checklist.prompt.md
├── .specify/
│   └── templates/            # V5.3 Templates
│       ├── AWP-template.md           # Autonomous Work Package v2.0
│       ├── memory-runtime-prompt.md  # Runtime execution guidance
│       └── memory-system-readme.md   # Memory architecture docs
├── docs/                     # 📚 Macro Memory (your project docs)
├── .context/                 # 🎯 Core Memory (active specs)
├── logs/                     # 📝 Evidence Memory (execution logs)
├── work_package_archives/    # 📦 Historical Memory (completed AWPs)
└── README.md                 # This file
```

### Key Directories Explained

**`.github/prompts/`**: Defines the `/speckit.*` commands available in AI chat.

**`.specify/templates/`**: Template files that AI uses when generating specs, plans, and AWPs.

**Memory Directories** (created per project):
- `docs/`: Vision, architecture decisions, onboarding guides
- `.context/`: Active project state (current specs, plans)
- `logs/`: Execution logs, debugging notes, decision rationale
- `work_package_archives/`: Completed AWPs for historical reference

---

## 🧠 Memory System Deep Dive

V5.3's memory system is inspired by **long-term memory in LLMs** and **knowledge graphs**. It solves the problem of AI "forgetting" project context across sessions.

### Four Memory Tiers

| Tier | Directory | Lifespan | Purpose | Example Files |
|------|-----------|----------|---------|---------------|
| **Macro** | `docs/` | Permanent | Project vision, principles | `VISION.md`, `ARCHITECTURE.md` |
| **Core** | `.context/` | Semi-permanent | Active specs, plans | `spec.md`, `plan.md`, `tasks.md` |
| **Evidence** | `logs/` | Append-only | Execution logs, decisions | `2024-10-25-auth-impl.md` |
| **Historical** | `work_package_archives/` | Permanent | Completed work packages | `AWP-FEAT-001-COMPLETED.md` |

### Memory Lifecycle

```
1. Constitution Phase
   → AI writes project vision to docs/VISION.md (Macro Memory)

2. Specify/Plan Phase
   → AI writes spec.md, plan.md to .context/ (Core Memory)

3. Execute Phase
   → AI logs decisions to logs/YYYY-MM-DD.md (Evidence Memory)

4. Completion Phase
   → AI archives AWP to work_package_archives/ (Historical Memory)
```

### Using Memory in AI Prompts

**Example: Onboarding a new AI agent**

```
You: Please read the project context before we start.

AI: [Automatically reads]
- docs/VISION.md
- .context/PROJECT_CONTEXT.md
- Latest logs in logs/
```

**Example: Debugging a past decision**

```
You: Why did we choose JWT over sessions for authentication?

AI: [Searches logs/] 
Found in logs/2024-10-20-auth-design.md:
"Chose JWT because the frontend is a separate SPA and needs stateless auth..."
```

---

## 🔧 Configuration & Customization

### Adjusting AWP Template

Edit `.specify/templates/AWP-template.md` to customize:
- Phase structure (default: R-P-E-R)
- Review protocols
- Success criteria format

### Adding Custom Memory Tiers

You can extend the memory system:

```bash
mkdir .context/modules      # For modular context (e.g., per-feature)
mkdir logs/decisions        # For decision logs only
```

Update `memory-system-readme.md` to document your conventions.

### Windows Compatibility Notes

If you encounter shell command issues on Windows:

1. **Change default shell in Desktop Commander:**
   ```
   Use PowerShell instead of bash for better Windows support
   ```

2. **Adjust file paths in templates:**
   ```
   Replace Unix paths (/) with Windows paths (\) where needed
   ```

---

## 🤝 Contributing

We welcome contributions to spec-kit V5.3 Fusion!

### How to Contribute

1. **Report Issues**: Use GitHub Issues to report bugs or incompatibilities
2. **Fix Known Limitations**: See `docs/AUDIT_REPORT.md` for improvement opportunities
3. **Enhance Templates**: Submit PRs to improve AWP-template.md or memory system
4. **Test Unverified Features**: Help us test `/speckit.clarify` and `/speckit.checklist`

### Development Guidelines

- **Test before PR**: Ensure your changes don't break existing commands
- **Update docs**: Update README.md and AUDIT_REPORT.md if needed
- **Preserve backups**: Keep `.github_ORIGINAL/` and `.specify_ORIGINAL/` intact

### Priority Improvements (from Audit Report)

1. **High Priority**: Fix or replace `/speckit.analyze` command
2. **Medium Priority**: Add dependency visualization to AWP-template.md
3. **Medium Priority**: Integrate checklist into AWP REVIEW phase
4. **Low Priority**: Verify compatibility of clarify/checklist commands

---

## 📋 Roadmap

### v5.3.1 (Planned)

- [ ] Implement dependency graph visualization in AWP
- [ ] Verify and document `/speckit.clarify` compatibility
- [ ] Verify and document `/speckit.checklist` compatibility
- [ ] Add Windows-specific command templates

### v5.4 (Future)

- [ ] Fix or replace `/speckit.analyze` with AWP-compatible analyzer
- [ ] Integrate checklist into AWP REVIEW phase
- [ ] Add memory search/query tool (RAG-based)
- [ ] Support multiple memory profiles (e.g., per-feature)

### v6.0 (Vision)

- [ ] Full MCP (Model Context Protocol) integration
- [ ] Visual memory graph UI
- [ ] Automated AWP generation from natural language
- [ ] Multi-agent orchestration for complex projects

---

## 📚 Documentation

- **[AUDIT_REPORT.md](docs/AUDIT_REPORT.md)**: Full conflict analysis of V5.3 vs original spec-kit
- **[MEMORY_SYSTEM.md](.specify/templates/memory-system-readme.md)**: Detailed memory architecture
- **[AWP_GUIDE.md](docs/AWP_GUIDE.md)**: How to write and execute autonomous work packages *(coming soon)*

---

## 📄 License

MIT License - Same as [original spec-kit](https://github.com/mikeyobrien/spec-kit)

Copyright (c) 2024 spec-kit V5.3 Fusion Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.

---

## 🙏 Acknowledgments

- **Original spec-kit**: Thank you to [Mikey O'Brien](https://github.com/mikeyobrien) for creating the foundational spec-kit framework
- **AWP Inspiration**: Inspired by autonomous agent frameworks (AutoGPT, BabyAGI)
- **Memory System**: Influenced by Mem0 and LangChain's memory modules

---

## 📞 Support & Contact

- **Issues**: [GitHub Issues](https://github.com/your-username/spec-kit-v5.3-fusion/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-username/spec-kit-v5.3-fusion/discussions)
- **Email**: your-email@example.com

---

**Happy Spec-ing! 🚀**

*Remember: Specification-first development is not about bureaucracy—it's about thinking before coding.*

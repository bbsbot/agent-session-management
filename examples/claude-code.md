# Claude Code Integration

Add the following to your project's `CLAUDE.md`:

---

## Session Management

Read and follow all instructions in `SKILL.md` for session management throughout this entire session.

### Configuration
Session parameters are defined in `config/session-defaults.yaml`. Key overrides for this project:

- Budget: 80 tool calls per 5-hour window
- Sprint size: 20 tool calls
- Rest duration: 5 minutes between sprints
- Extended rest: 15 minutes after 4 sprints

### Sleep Execution
When SKILL.md instructs you to rest, use the Bash tool to run:
```bash
bash scripts/session-timer.sh [MINUTES]
```

### Multi-Agent Note
If using AGENTS.md with sub-agents, each sub-agent inherits session management
from SKILL.md. The orchestrator manages budget allocation per Section 5 of the skill.

### Permission Prompts
Note: Claude Code permission prompts add cognitive overhead but do not count
as tool calls for budget tracking purposes. To minimize prompt fatigue,
pre-approve common operations:
```bash
claude config set permissions.allow "Edit,Read,Write,Bash(git *)"
```

### Recommended CLAUDE.md Structure
```markdown
# Project: My App

## Required Skills
Read and follow all instructions in SKILL.md for session management.

## Project Context
[your project description here]

## Coding Standards
[your standards here]

## Architecture
[your architecture notes here]
```

This ensures the agent loads session management awareness before beginning any work.
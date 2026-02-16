# Cursor Integration

Add the following to your project's `.cursorrules`:

---

## Session Management Skill

The file `SKILL.md` in the project root contains session management instructions.
Follow all instructions in that file throughout this session.

Key points for Cursor:
- Map each Cursor "edit" or "apply" action as one tool call for budget tracking
- Background agents (if used) should use a reduced sprint size of 10 tool calls
- Use the integrated terminal to execute sleep commands from SKILL.md
- Cursor's Composer counts as the orchestrator in multi-agent scenarios

When executing rest periods, run in the terminal:
```bash
bash scripts/session-timer.sh 5
```

### Cursor-Specific Budget Mapping

| Cursor Action        | Tool Call Count |
|---------------------|----------------|
| File edit/apply     | 1              |
| File read           | 1              |
| Terminal command     | 1              |
| Search/lookup        | 1              |
| Multi-file edit     | Count each file |
| Chat response only  | 0 (no tool use) |

### Composer Integration
When using Cursor Composer for multi-file operations:
- Each file touched in a Composer action counts as one tool call
- A single Composer "apply all" with 5 files = 5 tool calls
- Plan Composer batches carefully in YELLOW zone

### Background Agent Notes
Cursor background agents operate asynchronously. For session management:
- Each background task gets a sub-budget of 10 tool calls
- Background agents should report completion with call count
- If a background task exceeds 10 calls, flag it to the user
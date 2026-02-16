# Aider Integration

Add the following to your `.aider.conf.yml`:

```yaml
read:
  - SKILL.md
  - config/session-defaults.yaml
```

### Mapping Aider Concepts to Session Skill

Aider operates differently from IDE-based agents. Here is how to map concepts:

| Aider Concept          | Session Skill Equivalent     |
|-----------------------|------------------------------|
| Edit cycle (prompt→change) | ~3-5 tool calls          |
| /run command           | 1 tool call                  |
| /add file              | 1 tool call                  |
| /commit                | Sprint checkpoint            |
| /diff review           | 1 tool call                  |

### Recommended Workflow

1. **Start session:** `/read SKILL.md` to load session awareness
2. **Work in sprint-sized batches** of related edits (4-5 edit cycles per sprint)
3. **After each batch:** `/commit` then `/run bash scripts/session-timer.sh 5`
4. **Check budget zone** and adjust pace accordingly
5. **End session:** `/run bash scripts/session-timer.sh 0` (just to log final status)

### Aider-Specific Tips

- Aider's `/tokens` command shows actual token usage — use it to calibrate your budget
- In YELLOW zone: use `/ask` for planning instead of `/code` to conserve budget
- In RED zone: use only `/ask` for creating outlines and TODO lists
- Aider's auto-commit feature naturally creates good sprint checkpoints
- Use `/map` sparingly in YELLOW zone — it consumes tokens for repo mapping

### Chat Mode Budget
When using aider in chat-only mode (`/ask`):
- Chat responses consume tokens but not tool calls
- However, long conversations still deplete the token budget
- In YELLOW zone, keep /ask responses focused and brief
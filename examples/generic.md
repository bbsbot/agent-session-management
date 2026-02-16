# Generic Integration Guide

For any AI coding agent or custom agent system:

## Option 1: System Prompt Include
Copy the full contents of `SKILL.md` into your agent's system prompt or
instructions file. This works with any LLM-based agent that accepts custom
system prompts.

## Option 2: File Reference
If your agent can read project files, place `SKILL.md` in the project root
and instruct the agent:

> "Read and follow SKILL.md for session management."

## Option 3: Partial Include
If token budget for instructions is limited, include at minimum:
1. Section 1 (Budget Awareness) — the zone system
2. Section 2 (Pomodoro Mode) — sprint/rest cadence
3. Section 3 (Rate Limit Detection) — backoff protocol

These three sections provide the core functionality. Sections 4-7 are
enhancements that improve the experience but are not strictly required.

## Adapting Sleep Commands

If your agent **can** execute shell commands:
- Use the sleep commands from SKILL.md Section 2 directly
- Or use `bash scripts/session-timer.sh N` for countdown display

If your agent **cannot** execute shell commands:
- Skip automated sleep — rely on checkpoint reporting instead
- The agent should report "REST NEEDED — [N] minutes" and pause
- Wait for the user to manually continue after the specified duration
- User simply waits the specified time before sending the next message

If your agent has a **different shell mechanism**:
- Adapt the sleep command to your platform's equivalent
- The key requirement is synchronous blocking — the agent must not
  proceed until the rest period completes

## Adapting Tool Call Counting

Different agents have different concepts of "tool calls." General mapping:

| Operation              | Tool Call Count |
|-----------------------|----------------|
| File read              | 1              |
| File write/create      | 1              |
| File edit/patch        | 1              |
| Shell/terminal command | 1              |
| Code search/grep       | 1              |
| Web search/lookup      | 1              |
| Multi-file operation   | Count each file |
| Chat-only response     | 0              |

Adjust `sprint.tool_calls` in `config/session-defaults.yaml` to match
your agent's typical throughput per sprint.

## Custom Agent Frameworks

If you are building a custom agent framework:

### LangChain / LangGraph
- Add SKILL.md content to the system message
- Instrument tool calls with a counter callback
- Use the counter to trigger zone changes automatically

### AutoGen
- Include SKILL.md in the agent's system message
- Map AutoGen "turns" to sprint boundaries
- Use GroupChat manager as the orchestrator for multi-agent budgeting

### CrewAI
- Add SKILL.md to each agent's backstory or goal
- Map CrewAI "tasks" to sprints
- The Crew manager acts as the orchestrator for budget splitting

### Custom Python Agents
```python
# Minimal session management integration
class SessionManager:
    def __init__(self, max_calls=80, sprint_size=20):
        self.max_calls = max_calls
        self.sprint_size = sprint_size
        self.call_count = 0
        self.sprint_count = 0

    def track_call(self):
        self.call_count += 1
        if self.call_count % self.sprint_size == 0:
            self.sprint_count += 1
            return "REST"  # Signal rest needed
        return self.get_zone()

    def get_zone(self):
        pct = (self.call_count / self.max_calls) * 100
        if pct < 50: return "GREEN"
        if pct < 75: return "YELLOW"
        if pct < 90: return "RED"
        return "CRITICAL"
```
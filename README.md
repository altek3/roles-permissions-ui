# AI Support Agent (with tool use + CI)

A Claude-powered support agent that doesn't just *answer* customer
questions — it **investigates** them, using tools to look up account
details and ticket history, and can escalate to a human when needed.

This is the "agentic" pattern behind real AI support tools: the model
decides which tools to call, in what order, based on the conversation —
it isn't a hardcoded if/else flowchart.

## Why this is a step up from a single API call

- **Multi-step reasoning**: the agent can call multiple tools in sequence
  (e.g. check the account, then check tickets, then decide whether to
  escalate) before giving a final answer
- **Real tool-use / function-calling**: uses Claude's tool-use API, the
  same pattern used to build agents that take real actions, not just
  generate text
- **Automated tests**: the tool layer has a real test suite (no API calls
  needed — fast, free, deterministic)
- **CI/CD**: a GitHub Actions workflow (`.github/workflows/tests.yml`)
  runs the test suite automatically on every push, so broken code can't
  silently sit in `main`

## How it works

1. A customer message + email comes in
2. Claude decides whether it needs more info (account status? ticket
   history?) and calls the relevant tool
3. The tool result is fed back to Claude, which can call more tools or
   give a final answer
4. If the situation warrants it (repeated failures, billing issues,
   frustration), Claude calls `escalate_to_human`

### Example

```bash
python agent.py "mike@resort.com" "My card keeps failing and I've reported this before, this is really frustrating"
```

Because `mike@resort.com` has two open billing tickets in the mock data,
the agent should look up his account, see the repeated open tickets, and
escalate to a human rather than trying to solve it again itself.

## Setup

```bash
git clone <this-repo>
cd support-agent-tools
pip install -r requirements.txt
export ANTHROPIC_API_KEY=your_key_here
python agent.py "jane@hotel.com" "Did my CSV export issue ever get fixed?"
```

## Running the tests

```bash
pytest test_tools.py -v
```

These test the tool layer directly (account lookup, ticket lookup,
escalation logging) — no API key or network calls needed, so they run in
under a second and are what CI runs on every push.

## Project structure

```
agent.py       — the agent loop (calls Claude, executes tools, loops)
tools.py       — tool implementations + tool schemas
mock_data.py   — fake accounts/tickets (stand-in for a real database)
test_tools.py  — automated tests for the tool layer
.github/workflows/tests.yml — CI: runs tests on every push
```

## Possible next steps

- Swap `mock_data.py` for real database queries
- Add a tool for creating/updating tickets, not just reading them
- Add conversation memory across multiple messages from the same customer
- Wrap this in the Flask webhook service from the other project so it's
  reachable over HTTP

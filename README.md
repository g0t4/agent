This repo is an experiment with setting up my agent harness with ACP to allow any ACP client to execute an agent with it.
- Also to help wrap my mind around what my harness might be missing.

## Permissions

Ideas around tool calling permisisons. How to keep it simple and yet in a tiny fraction of cases, ask the user to decide...

i.e. instead of blanket approval/denial... perhaps a layered approach:

- `auto-approve`: resourceless tool calls, almost always safe:
  - `run_command` tool: `echo hello`, `date`
- `auto-approve based on resource permissions`: no side effects, but not always safe:
  - ok: `cat pyproject.toml`
  - ehhh: `cat .env`, `cat /etc/passwd`
  - knowing which paths are involved can help (as well as operation: read/write/stat/etc)
  - could deny-list which paths are off limits and auto approve the rest
  - could allow-list specific directories (i.e. current only) and require approval for any outside of current
- `llm-scoring` complex commands (hard to parse):
  - ask an LLM to score risk
    - or ask LLM to parse type of command(s) involved and feed into above risk scoring (resourceless, no side effects, path(s) involved)
  - could approve based on configurable risk score (level)
- `always-ask` some tools could require always asking the user


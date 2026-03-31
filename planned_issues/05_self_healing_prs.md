# [Feature] Self-Healing PRs: Agent Fixes Its Own Tools

## Description
Currently, when an agent encounters a persistent blocker (e.g., a broken API endpoint or an outdated scraper script), it fails and waits for a human to fix the tool. In a true autonomous system, the agent should attempt to fix the tool itself.

## Proposed Changes
1. **Blocker Detection**: Enhance `harness_heartbeat.py` to detect when the same `Blocking Reason` occurs for more than 3 consecutive runs.
2. **Self-Healing Prompt**: When a persistent blocker is detected, inject a new sub-mission into the playbook: "Analyze the tool failure, write a fix, and submit a Pull Request to the repository."
3. **PR Integration**: Provide the agent with a `git_pr` tool (via GitHub CLI) to automatically push its fix to a new branch and open a PR for human review.

## Priority
**Low** (Future roadmap) - Advanced feature for the "Continuous Autonomous Operations" stage.

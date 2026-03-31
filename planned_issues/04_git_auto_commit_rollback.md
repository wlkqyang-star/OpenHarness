# [Feature] Entropy Combat: Git Auto-Commit and Rollback Integration

## Description
Long-running agents can sometimes destroy their own workspace or corrupt their state files. To combat this entropy, we need to integrate version control directly into the heartbeat cycle.

## Proposed Changes
1. **Auto-Commit**: Modify `harness_heartbeat.py` to run `git add . && git commit -m "chore: state snapshot before run"` at the start of every execution cycle.
2. **Auto-Rollback**: If `harness_eval.py` detects a catastrophic failure (e.g., critical files deleted, playbook corrupted), automatically trigger `git reset --hard HEAD~1` to restore the workspace to the last known good state.

## Priority
**Medium** - Essential for long-term unsupervised operations.

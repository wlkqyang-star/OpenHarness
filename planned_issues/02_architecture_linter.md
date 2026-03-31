# [Feature] Implement Architecture Linter for Mechanical Constraints (Pillar 2)

## Description
Currently, architecture constraints (like "don't build the UI before the Service layer") rely on soft prompts and markdown guidelines. To increase reliability, we need to enforce these mechanically. 

## Proposed Changes
1. Fully implement the `harness_linter.py` skeleton.
2. **Directional Dependency Check**: Parse `playbook.md` and throw an error if UI-layer tools are invoked before Service-layer tools.
3. **Tool Whitelist Trimming**: Automatically trim the tool whitelist in `mission.md` to a maximum of 10 core tools to reduce context entropy.
4. **CI Integration**: Integrate the linter into `harness_boot.py` so it runs automatically at every boot cycle, acting as a CI pipeline for the agent.

## Example Concept
```python
def enforce_constraints(playbook_path, mission_path):
    with open(playbook_path) as f: 
        content = f.read()
    if "UI" in content and "Service" not in content[:content.find("UI")]:
        raise ValueError("Architecture violation: Service must precede UI")
    update_tool_whitelist(mission_path, max_tools=10)
```

## Priority
**Medium** (Week 2 target) - Essential for bounding the solution space and preventing agent hallucinations.

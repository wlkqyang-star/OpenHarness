# [Feature] Upgrade harness_eval.py to End-to-End (E2E) Evaluation Engine (Pillar 1)

## Description
Currently, `harness_eval.py` only performs basic file existence and JSON schema checks. To reach the 95%+ success rate seen in benchmarks like Anthropic's CORE-Bench, we must upgrade the evaluation loop to verify real-world outcomes using Playwright.

## Proposed Changes
1. Add a new `e2e_evaluate(workspace_path, eval_criteria)` function in `harness_eval.py`.
2. Integrate `playwright` to spin up a headless browser and verify actual UI states or external system states (e.g., checking if an email was actually received or a dashboard was updated).
3. Update `eval_criteria.md` template to include an "E2E Checks" section.
4. **Auto-Fix Generation**: If the E2E check fails, the evaluator should not just mark it as failed, but generate a concrete fix suggestion and append it to `progress.md`.

## Example Code Snippet
```python
from playwright.sync_api import sync_playwright

def e2e_evaluate(workspace_path, eval_criteria):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        score = run_user_defined_tests(browser, workspace_path, eval_criteria)
        browser.close()
        
    if score < 0.85:
        with open(f"{workspace_path}/progress.md", "a") as f:
            f.write(f"\n### Fix Suggestion\n{generate_fix_report(score)}\n")
        return False
    return True
```

## Priority
**High** (Week 1 target) - This is the highest ROI improvement for agent reliability.

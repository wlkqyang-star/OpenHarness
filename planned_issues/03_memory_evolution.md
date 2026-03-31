# [Feature] Implement Memory Evolution Engine (Pillar 3)

## Description
Currently, `heartbeat.md` and `progress.md` provide static cross-session recovery. To achieve long-term continuous operations, we need to transform these static logs into an evolutionary knowledge base. The agent should learn from its own successful trajectories.

## Proposed Changes
1. Fully implement the `memory_evolution.py` skeleton.
2. **Gene Extraction**: After a successful run (evaluator score > 0.9), use an LLM to extract the "success genes" (key insights, exact API parameters, precise CSS selectors) from the `progress.md` trajectory.
3. **Vector DB Storage**: Store these genes in a lightweight local SQLite + embeddings database (or integrate with Cognee).
4. **Auto-Update Playbook**: Automatically inject the top extracted genes back into the `playbook.md` as "Learned Best Practices" for the next run.

## Example Concept
```python
def evolve_memory(success_task, evaluator_score):
    if evaluator_score > 0.9:
        gene = extract_key_insight(success_task)
        save_to_vector_db(gene)
        update_playbook_with_gene(gene)
```

## Priority
**High** (Week 3 target) - This is the "killer feature" that allows a 3-line prompt to eventually evolve into a 200-line expert playbook automatically.

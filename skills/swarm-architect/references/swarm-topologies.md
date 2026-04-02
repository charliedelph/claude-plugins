# Swarm Topology Patterns

Reference diagrams and handoff contract templates for each topology type.

---

## Sequential (Pipeline)

```
[User Input]
     ↓
[Agent 1: Intake]
     ↓
[Agent 2: Analysis]
     ↓
[Agent 3: Synthesis]
     ↓
[Final Output]
```

**When to use:** Each agent's output is the required input for the next. Strong dependency chain.
**Risk:** Single point of failure — one bad agent corrupts everything downstream.
**Mitigation:** Add a validation step at each agent (self-evaluator + explicit fallback).
**Best for:** Document processing, stepwise reasoning, code generation pipelines.

---

## Parallel (Fan-Out / Fan-In)

```
              [User Input]
             /      |      \
    [Agent A]  [Agent B]  [Agent C]
             \      |      /
          [Synthesizer Agent]
                   ↓
            [Final Output]
```

**When to use:** Sub-tasks are independent and can be executed simultaneously.
**Risk:** Synthesizer is the bottleneck — it must handle inconsistent formats and potential conflicts.
**Mitigation:** Standardize all agent output formats before the Synthesizer receives them. Add conflict protocol.
**Best for:** Multi-domain research, parallel analysis (technical + fundamental + sentiment), competitive intelligence.

---

## Hierarchical (Manager / Worker)

```
         [Orchestrator Agent]
        /         |          \
[Worker A]  [Worker B]  [Worker C]
        \         |          /
         [Orchestrator Agent]  ← reviews and integrates
                  ↓
          [Final Output]
```

**When to use:** Tasks require active coordination, re-delegation, or quality review between rounds.
**Risk:** Orchestrator is a bottleneck; high token cost for the manager agent.
**Mitigation:** Use Opus for the Orchestrator; Haiku/Sonnet for workers. Cap the number of review rounds.
**Best for:** Complex research, iterative drafting, multi-step planning with review cycles.

---

## Mesh (Peer Review)

```
[Agent A] ←→ [Agent B]
    ↕              ↕
[Agent C] ←→ [Agent D]
              ↓
      [Synthesizer]
```

**When to use:** High-stakes outputs where multiple independent reviews dramatically improve quality.
**Risk:** Highest token cost of any topology; complex to orchestrate; can create feedback loops.
**Mitigation:** Define strict review scope for each agent (what they are and are not reviewing). Set maximum review rounds (2 max).
**Best for:** Legal review, medical decision support, financial risk analysis, security audits.

---

## Hybrid (Recommended for Complex Goals)

```
[User Input]
     ↓
[Intake / Structuring Agent]        ← Sequential
     ↓
/──────────────────────\
[Expert A] [Expert B] [Expert C]    ← Parallel fan-out
\──────────────────────/
     ↓
[Conflict Resolver]                 ← Sequential
     ↓
[Devil's Advocate]                  ← Sequential
     ↓
[Final Synthesizer]                 ← Sequential
     ↓
[Output]
```

**When to use:** Goals that require both breadth (parallel expert analysis) and depth (sequential refinement).
**This is the default recommendation** for goals with 4+ agents and any domain complexity.

---

## Handoff Contract Templates

### Standard Handoff (Sequential)
```
HANDOFF CONTRACT:
- Pass to: [NEXT_AGENT_NAME]
- Format: [Markdown / JSON / Plain text]
- Required fields:
    - [FIELD_1]: [description]
    - [FIELD_2]: [description]
- Flag for human review if: [CONDITION — e.g., "confidence score < 7" or "conflicting data sources"]
- If task cannot be completed: Output "BLOCKED: [reason]" and halt. Do not pass forward.
```

### Parallel Agent Handoff (Fan-In)
```
HANDOFF CONTRACT:
- Pass to: Synthesizer
- Format: JSON with the following schema:
    {
      "agent_name": "[YOUR AGENT NAME]",
      "confidence": [1-10],
      "primary_finding": "[one sentence]",
      "supporting_evidence": ["item1", "item2"],
      "flags": ["any concerns or caveats"],
      "conflicts_with": ["agent name if you believe another agent's output contradicts yours"]
    }
- If task cannot be completed: Return JSON with "primary_finding": "INCOMPLETE" and reason in "flags"
```

### Manager → Worker Delegation
```
DELEGATION CONTRACT (Orchestrator → Worker):
- Task: [specific sub-task]
- Scope: [what is IN scope]
- Out of scope: [what the worker must NOT do]
- Input: [exactly what data you are passing]
- Expected output: [format and fields]
- Quality bar: Rate your output 1–10. Return only if score ≥ 7. Revise silently otherwise.
- Deadline behavior: If you cannot complete in one pass, return your best partial output with "PARTIAL" flagged.
```

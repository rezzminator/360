# 360°

A Claude Code skill for exhaustive multi-angle analysis. Systematically generates ALL angles on a subject — questions, risks, edge cases, blind spots — organized by dimension.

This is a **thinking protocol**, not a task runner. It produces a comprehensive list of angles, not answers. The consumer decides what to act on — 360° just ensures nothing gets skipped.

## Why this exists

When you ask an LLM to "think about edge cases" or "what could go wrong," it generates 3-5 obvious concerns and moves on. The same blind spots keep slipping through because there's no systematic coverage guarantee.

360° forces a walk through every dimension of concern — inputs, state, boundaries, timing, auth, regressions (for code), or assumptions, ambiguities, contradictions, dependencies, scope gaps (for designs). Each dimension must produce at least one concrete angle or an explicit "N/A: reason" — no silent skips.

## Two domains

| Domain | When to use | Dimensions |
|--------|------------|------------|
| **test** | Analyzing code, implementations, changes — things that will be tested | Inputs, State, Boundaries, Sequences, Timing, Error paths, Data shapes, Environment, Auth/Authz, Regressions |
| **inquiry** | Analyzing requirements, proposals, designs — things that need questioning | Assumptions, Ambiguities, Contradictions, Missing info, Dependencies, Scope gaps, Stakeholder conflicts, Feasibility, Precedent |

## Installation

### As a Claude Code skill

```bash
# From your project root
mkdir -p .claude/skills/360
cp SKILL.md .claude/skills/360/SKILL.md
```

Then use it in Claude Code:

```
360 <subject>           — run a full 360° analysis
360 test <subject>      — force the test domain
360 inquiry <subject>   — force the inquiry domain
```

### Embedded in agents

Agents can reference the protocol as a thinking step before their main work:

```
Before diving in, run the 360° protocol (test domain) from
.claude/skills/360/SKILL.md against the subject. Walk every
dimension internally as a thinking step.
```

The 360° output is implicit in the agent's work — it guides the analysis without producing a separate artifact.

## Example output

```
## 360° — user deletion endpoint (test)

### 1. Inputs
- What if the user ID is valid but already deleted?
- What if the ID format is correct but doesn't exist in any table?

### 2. State
- What if the user has active sessions in progress?
- What if a background job is processing this user's data right now?

### 3. Boundaries
- What if the user has zero associated records vs 10,000?

...
```

## Key design decisions

**Exhaustive enumeration, not exhaustive analysis.** Each angle is a one-liner — enough to identify the concern, not enough to investigate it. Investigation comes later.

**No filtering.** If an angle seems unlikely, include it anyway. The caller decides what matters. 360° is a coverage tool, not a prioritization tool.

**Conscious N/A.** Skipping a dimension requires writing "N/A: reason." This prevents the most common failure mode — unconsciously skipping an entire category because nothing immediately comes to mind.

## License

MIT

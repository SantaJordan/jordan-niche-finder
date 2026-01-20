---
name: ctox-insight-validator
description: Validate CTO insights are counterintuitive, specific, quantifiable, and demonstrable before proceeding to targeting
---

# CTOx Insight Validator Skill

## When to Use

Use this skill AFTER niche triangulation, BEFORE building targeting. The CTO must have at least one validated insight before you build their targeting strategy.

**DO NOT PROCEED TO TARGETING** until insights pass all four tests.

## The Four Tests

Every insight must pass ALL FOUR tests:

```
┌──────────────────────────────────────────────────────────────┐
│                    INSIGHT VALIDATION                        │
│                                                              │
│   ┌─────────────────┐    ┌─────────────────┐                │
│   │ COUNTERINTUITIVE│    │    SPECIFIC     │                │
│   │                 │    │                 │                │
│   │ Would other     │    │ Can they name   │                │
│   │ CTOs disagree?  │    │ exact tools?    │                │
│   └────────┬────────┘    └────────┬────────┘                │
│            │                      │                          │
│            ▼                      ▼                          │
│   ┌─────────────────┐    ┌─────────────────┐                │
│   │  QUANTIFIABLE   │    │  DEMONSTRABLE   │                │
│   │                 │    │                 │                │
│   │ Before/after    │    │ Show in 5 min,  │                │
│   │ with numbers?   │    │ not explain 30? │                │
│   └─────────────────┘    └─────────────────┘                │
│                                                              │
│   ALL FOUR MUST PASS → Proceed to targeting                  │
│   ANY FAIL → Push back and dig deeper                        │
└──────────────────────────────────────────────────────────────┘
```

## Test 1: Counterintuitive

**Question:** "If you tweeted this, would other CTOs reply 'Actually...'?"

Good insights provoke disagreement or surprise. If every CTO would nod along, it's not an insight - it's conventional wisdom.

### Examples

| Insight | Counterintuitive? | Verdict |
|---------|-------------------|---------|
| "Code quality matters" | No - everyone agrees | FAIL |
| "Technical debt should be paid down" | No - conventional wisdom | FAIL |
| "Aggressive refactoring DURING growth phases beats stability" | Yes - many would disagree | PASS |
| "Monolith-first even in 2024, microservices are almost always premature" | Yes - controversial | PASS |
| "Skip unit tests for the first 6 months of a startup" | Yes - provocative | PASS |

### Pushback Templates

- "Every CTO would agree with that. Where's the counterintuitive part?"
- "That's table stakes. What's the insight that would make other CTOs uncomfortable?"
- "I could have read that in any tech blog. What do YOU know that they don't?"
- "That's best practice, not insight. What do you do that ISN'T best practice?"

## Test 2: Specific

**Question:** "Can you name the exact tools, frameworks, or patterns?"

Vague insights can't be demonstrated or taught. Specific insights have named components.

### Examples

| Insight | Specific? | Verdict |
|---------|-----------|---------|
| "Modern architecture approaches" | No - what does that even mean? | FAIL |
| "Better testing practices" | No - too vague | FAIL |
| "Event sourcing with Kafka + CQRS for audit trails in fintech" | Yes - named tools and pattern | PASS |
| "Strangler fig pattern using feature flags with LaunchDarkly" | Yes - specific approach | PASS |
| "Domain-driven design with bounded contexts per acquisition" | Yes - named methodology | PASS |

### Pushback Templates

- "What specific tools or frameworks are you using?"
- "Name the exact pattern. What's it called?"
- "'Modern' means nothing. What SPECIFICALLY makes it modern?"
- "If I wanted to learn this, what would I search for?"

## Test 3: Quantifiable

**Question:** "What's the before/after with numbers?"

If they can't quantify the impact, they can't prove it works. Require at least ONE metric.

### Examples

| Insight | Quantified? | Verdict |
|---------|-------------|---------|
| "Faster deployments" | No - how much faster? | FAIL |
| "Better reliability" | No - measured how? | FAIL |
| "Went from monthly deploys to daily deploys" | Yes - frequency metric | PASS |
| "Reduced incidents from 12/month to 3/month" | Yes - rate metric | PASS |
| "Cut integration time from 6 months to 6 weeks" | Yes - time metric | PASS |
| "Saved $400K/year in cloud costs" | Yes - cost metric | PASS |

### Good Metrics

- **Time**: "Reduced X from Y weeks to Z days"
- **Frequency**: "Went from monthly to daily"
- **Rate**: "Incidents dropped from X to Y per month"
- **Cost**: "Saved $X per year"
- **Percentage**: "X% improvement in Y"

### Pushback Templates

- "Faster than what? Give me numbers."
- "What was it before? What was it after?"
- "How do you measure success?"
- "If I asked your customer to quantify the impact, what would they say?"

## Test 4: Demonstrable

**Question:** "Can you show this working in 5 minutes?"

If it takes 30 minutes to explain, it's not crystallized. Real expertise can be demonstrated quickly.

### Examples

| Insight | Demonstrable? | Verdict |
|---------|---------------|---------|
| "I have deep expertise in distributed systems" | No - just a claim | FAIL |
| "I understand complex architectures" | No - can't show "understanding" | FAIL |
| "Here's the migration script template I use every time" | Yes - artifact exists | PASS |
| "Watch me diagram the bounded context separation for your acquisition" | Yes - can show live | PASS |
| "Here's a 5-minute video of me doing the pattern" | Yes - documented | PASS |

### What "Demonstrable" Looks Like

- A template they reuse
- A diagram they can draw in 2 minutes
- A script or tool they've built
- A before/after they can show
- A case study they can walk through

### Pushback Templates

- "If you had 5 minutes in front of a prospect, how would you SHOW this?"
- "What artifact exists that proves this works?"
- "Can you draw it? Show me."
- "What would a demo of this look like?"

## Scoring an Insight

Use this checklist:

```
□ Counterintuitive - Other CTOs would push back on this
□ Specific - Named tools, frameworks, or patterns
□ Quantifiable - Has at least one before/after metric
□ Demonstrable - Can show in 5 minutes or less

4/4 = PROCEED to targeting
3/4 = Push back on the missing dimension
2/4 or less = Dig deeper, this isn't ready
```

## Validation Flow

```
CTO states insight
        │
        ▼
┌───────────────────┐
│ Counterintuitive? │──No──▶ "Every CTO agrees with that. What's
└────────┬──────────┘         the uncomfortable part?"
         │Yes
         ▼
┌───────────────────┐
│    Specific?      │──No──▶ "Name the exact tools/patterns.
└────────┬──────────┘         What specifically are you doing?"
         │Yes
         ▼
┌───────────────────┐
│  Quantifiable?    │──No──▶ "What's the before/after with
└────────┬──────────┘         numbers? How do you measure this?"
         │Yes
         ▼
┌───────────────────┐
│  Demonstrable?    │──No──▶ "How would you show this in 5 min?
└────────┬──────────┘         What artifact exists?"
         │Yes
         ▼
   ✓ VALIDATED
   Proceed to targeting
```

## Common Failures

### The "Best Practice" Trap
**What they say:** "I implement CI/CD properly"
**Why it fails:** Not counterintuitive - every CTO claims this
**Push back:** "Everyone says they do CI/CD. What do you do DIFFERENTLY?"

### The "Vague Expertise" Trap
**What they say:** "I'm really good at architecture"
**Why it fails:** Not specific - what kind of architecture?
**Push back:** "Architecture of what? Using what patterns? Name them."

### The "Trust Me" Trap
**What they say:** "It just works better"
**Why it fails:** Not quantifiable - no proof
**Push back:** "Better than what? By how much? Show me the numbers."

### The "Take My Word For It" Trap
**What they say:** "It's hard to explain but I know it when I see it"
**Why it fails:** Not demonstrable - can't teach or show
**Push back:** "If you can't show it in 5 minutes, is it really your expertise?"

## Output Format

When an insight is validated, document it:

```markdown
### Insight: [Title]

**Statement:** [The counterintuitive claim]

**Validation:**
- Counterintuitive because: [Why other CTOs would disagree]
- Specific approach: [Named tools, frameworks, patterns]
- Quantified impact: [Before/after metrics]
- Demonstration: [How to show in 5 min]

**Targeting implication:** [How this translates to finding customers]
```

---
name: ctox-niche-discovery
description: Guide CTOs through niche triangulation using Ben Horowitz's "combination of three things" principle
---

# CTOx Niche Discovery Skill

## When to Use

Use this skill when interviewing a CTO to find their ultra-specific niche. The goal is to narrow them down from "I help tech companies" to something like "I help healthcare SaaS companies navigate post-acquisition tech integration using parallel-run migration."

## The Core Framework

**NICHE = Industry Vertical × Specific Situation × Unique Angle**

All three must intersect. Missing any one = too generic.

```
┌─────────────────────────────────────────────────────────────┐
│                         NICHE                               │
│                                                             │
│    ┌──────────────┐                                         │
│    │   Industry   │  "Healthcare SaaS"                      │
│    │   Vertical   │  NOT "tech" or "healthcare"             │
│    └──────┬───────┘                                         │
│           │                                                 │
│           ▼                                                 │
│    ┌──────────────┐                                         │
│    │  Specific    │  "Post-acquisition tech integration"    │
│    │  Situation   │  NOT "scaling" or "growth"              │
│    └──────┬───────┘                                         │
│           │                                                 │
│           ▼                                                 │
│    ┌──────────────┐                                         │
│    │   Unique     │  "Parallel-run migration without        │
│    │   Angle      │   feature freeze"                       │
│    └──────────────┘  NOT "best practices" or "modern arch"  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Dimension 1: Industry/Vertical

**Goal:** Find the industry they know better than 95% of other CTOs.

### Questions to Ask

- "What industry do you know better than 95% of other CTOs?"
- "If you walked into an industry conference, which one would people assume you're the expert?"
- "What vertical do you understand so well you could write the textbook?"
- "Where would you be embarrassed to NOT have the answer?"

### Specificity Ladder

| Level | Example | Verdict |
|-------|---------|---------|
| Too broad | "Tech" | Useless |
| Still broad | "Healthcare" | Keep narrowing |
| Getting there | "Healthcare SaaS" | Better |
| Specific | "Healthcare SaaS doing EHR integrations" | Good |
| Ultra-specific | "Mid-market healthcare SaaS doing EHR integrations post-acquisition" | Excellent |

### Pushback Templates

- "Healthcare is too broad. What TYPE of healthcare? Payers? Providers? EHR? Telehealth?"
- "SaaS is not a vertical. What INDUSTRY does that SaaS serve?"
- "Fintech is massive. What part? Payments? Lending? Compliance? Trading?"

## Dimension 2: Specific Situation

**Goal:** Find the trigger event or specific moment when companies NEED them.

### Questions to Ask

- "When do companies call you? What's happening in their business at that exact moment?"
- "Is this chronic pain (always there, can delay) or episodic pain (sudden onset, must act now)?"
- "What's the trigger event that makes them pick up the phone?"
- "What situation creates urgency?"

### Episodic vs Chronic Pain

**Episodic pain is BETTER for targeting:**
- Has a trigger event (can detect in data)
- Creates urgency (faster decisions)
- Time-bound (higher response rates)

| Type | Example | Targetability |
|------|---------|---------------|
| Chronic | "We should scale better" | Low - can delay forever |
| Episodic | "We just got acquired and have 90 days to integrate" | High - deadline creates urgency |

### Specificity Ladder

| Level | Example | Verdict |
|-------|---------|---------|
| Too broad | "Growth" | Useless |
| Vague | "Scaling challenges" | Not specific |
| Better | "Post-funding scaling" | Getting there |
| Specific | "Post-Series A when tribal knowledge breaks" | Good |
| Ultra-specific | "First 90 days post-acquisition when two tech stacks must merge" | Excellent |

### Pushback Templates

- "Growth isn't a situation. When in their growth? What specifically breaks?"
- "Scaling is not a pain. What ABOUT scaling causes urgency?"
- "What's happening in their business at that EXACT moment that makes this urgent?"

## Dimension 3: Unique Angle

**Goal:** Find the counterintuitive insight or approach that sets them apart.

### Questions to Ask

- "What do you KNOW works that most CTOs would argue against?"
- "When you look at how other CTOs solve [X problem], what makes you cringe?"
- "What pattern have you discovered that isn't in any book?"
- "What's your contrarian take that would make other CTOs push back?"

### The Counterintuitive Test

Good unique angles make other CTOs uncomfortable:

| Angle | Counterintuitive? | Verdict |
|-------|-------------------|---------|
| "Clean code matters" | No - everyone agrees | Not unique |
| "Modern architecture" | No - everyone says this | Not unique |
| "Aggressive refactoring during growth beats stability" | Yes - many would disagree | Unique |
| "Monolith-first for startups, even in 2024" | Yes - controversial | Unique |

### Pushback Templates

- "Best practices aren't unique. What do you do that ISN'T best practice?"
- "If I searched for CTOs who know [X], I'd get 10,000 results. What narrows it to 50?"
- "That's what every CTO would say. What's YOUR specific take?"
- "Where do you disagree with the conventional wisdom?"

## Putting It Together

Keep pushing until you can complete this sentence:

> "I help **[SPECIFIC INDUSTRY]** companies who are **[SPECIFIC SITUATION]** by **[UNIQUE APPROACH that other CTOs don't use]**"

### Examples of Complete Niches

**Good:**
> "I help healthcare SaaS companies in the first 90 days post-acquisition merge their tech stacks using parallel-run migration without feature freeze."

**Good:**
> "I help fintech compliance platforms recover from failed SOC 2 audits by implementing event sourcing for complete audit trails."

**Good:**
> "I help B2B SaaS companies at the 50-200 employee stage who just lost their founding engineers by documenting tribal knowledge before it's gone."

**Bad:**
> "I help tech companies scale." (No industry, vague situation, no unique angle)

**Bad:**
> "I help healthcare companies with their technology." (Too broad, no situation, no angle)

## Red Flags

If you hear these, push back:

| Red Flag | Why It's Bad | Push Back With |
|----------|--------------|----------------|
| "I can help anyone" | No differentiation | "If everyone's your customer, no one is" |
| "Modern best practices" | Not unique | "What do you do that ISN'T best practice?" |
| "I'm good at scaling" | Every CTO says this | "Scaling WHAT specifically? When? How?" |
| "Full-stack expertise" | Capability, not niche | "But what PROBLEM do you solve better than anyone?" |
| "I work across industries" | Too broad | "Which industry do you know 10x better than the others?" |

## Success Criteria

The niche is ready when:

1. **Industry**: Specific enough that you could name 100 companies in it
2. **Situation**: Has a trigger event that can be detected in public data
3. **Unique Angle**: Would make some CTOs argue "Actually, I disagree..."
4. **Combined**: Makes the CTO slightly uncomfortable with how narrow it is

If they're comfortable with the niche, it's probably not specific enough.

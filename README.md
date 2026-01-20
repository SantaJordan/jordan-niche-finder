# CTOx Niche Discovery System

A Jordan Crawford-style interview system that grills CTOs until they find their ultra-specific niche and builds an Exa.ai targeting strategy.

## What This Does

1. **Grills you like Jordan Crawford** - Aggressive pushback on vague answers
2. **Triangulates your niche** - Industry × Situation × Unique Angle
3. **Validates your insights** - Must be counterintuitive, specific, quantifiable, demonstrable
4. **Builds targeting columns** - Exa.ai queries to find perfect-fit customers
5. **Generates outreach** - "The message is a redescription of the targeting"

## Quick Start

1. Open this folder in Claude Code
2. Fill out `context/cto-intake.md` with your info
3. Run `/ctox-niche`
4. Get grilled until your niche is ultra-specific
5. Walk away with targeting columns and a sample outreach message

## What Gets Rejected

- "I'm good at scaling" → Every CTO says this
- "I help tech companies" → Not a niche
- "Modern architecture" → Meaningless
- "Best practices" → Not an insight

## What Gets Accepted

- "I help embedded payments SaaS companies who just signed their first enterprise customer get SOC 2 compliant using a separate auditable compliance layer"
- "I help healthcare SaaS companies in the first 90 days post-acquisition merge tech stacks using parallel-run migration"

**If your niche makes you uncomfortable with how narrow it is, it's probably specific enough.**

## Setup: Exa.ai API

To run the targeting queries, you'll need an Exa API key:

1. Go to [exa.ai](https://exa.ai) and create an account
2. Get your API key from the dashboard
3. Set it as an environment variable:
   ```bash
   export EXA_API_KEY="your-key-here"
   ```

Or add it to your `.zshrc` / `.bashrc`:
```bash
echo 'export EXA_API_KEY="your-key-here"' >> ~/.zshrc
```

## File Structure

```
CTOx/
├── CLAUDE.md                    # System prompt (Jordan's personality)
├── .claude/
│   ├── settings.json            # Permissions
│   └── commands/
│       └── ctox-niche.md        # Main interview command
├── skills/
│   ├── ctox-niche-discovery.md  # Niche triangulation methodology
│   ├── ctox-insight-validator.md # 4-test insight checker
│   └── ctox-exa-targeting.md    # Exa query builder
├── context/
│   ├── cto-intake.md            # Fill this out first
│   └── niche-output.md          # Output template
└── README.md                    # You're reading it
```

## The Ben Horowitz Principle

You're not the world's best botanist. But you CAN be the world's best Japanese botanist specializing in Zen gardens.

**Niche = Industry × Situation × Unique Angle**

All three must be specific enough that:
- You can name 50-100 companies in the niche
- There's a detectable trigger event
- Other CTOs would disagree with your approach

## Credits

Built on Jordan Crawford's GTM methodology from [Blueprint](https://blueprint.is) and the CTOx program.

Key frameworks used:
- Pain-First Segmentation
- Existential Data Point (EDP) Framework
- Insight Validation (4 Tests)
- "The message is a redescription of the targeting"

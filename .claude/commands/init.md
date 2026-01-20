---
name: init
description: Initialize CTOx and verify Exa MCP is working (REQUIRED)
---

# CTOx Initialization

Welcome! This is the CTOx Niche Discovery System - Jordan Crawford's methodology for finding your ultra-specific CTO niche.

## CRITICAL: Exa MCP is Required

This system uses Exa.ai for all lead-finding and targeting. **You cannot proceed without Exa MCP installed and working.**

## Step 1: Test Exa MCP

**Run a test search using `mcp__exa__web_search_exa`** with:
- query: "healthcare SaaS company"
- numResults: 3

## Step 2: Handle Results

### If Exa works (you get search results):

Tell the user:

```
Exa MCP is working! You're ready to find your niche.

Run `/ctox-niche` to start the interview process.

Before we begin, you can optionally fill out `context/cto-intake.md` with:
- Your LinkedIn profile URL
- Your company website
- Recent clients you've worked with
- Your honest self-assessment

Or just dive in - I can ask you these questions directly.
```

### If Exa fails or tool not found:

**STOP. Do not offer to proceed with WebSearch instead.**

Tell the user exactly this:

```
⚠️  Exa MCP is not installed. This is REQUIRED for CTOx to work.

Exa is the best-in-class semantic search API for finding leads - it's specifically
designed for LinkedIn discovery, company research, and targeting. We can't do proper
lead-finding without it.

## Install Exa MCP (takes 2 minutes)

1. Run this command in your terminal:
   npx @anthropic/mcp add exa

2. Get your API key:
   - Go to https://exa.ai
   - Create an account (or sign in)
   - Copy your API key from the dashboard
   - 💡 Pro tip: Use code CANNONBALLGTM for 14-day free trial + 10,000 credits

3. When prompted by the MCP installer, enter your EXA_API_KEY

4. Restart Claude Code:
   - Mac: Cmd+Shift+P → "Developer: Reload Window"
   - Or just close and reopen Claude Code

5. Come back and run /init again

Once Exa is working, we can find your perfect-fit customers.
```

**DO NOT:**
- Offer to use WebSearch as a fallback
- Proceed to /ctox-niche without Exa
- Suggest any alternative search methods

Exa is required. Period.

---
name: init
description: Initialize CTOx and check Exa MCP setup
---

# CTOx Initialization

Welcome! This is the CTOx Niche Discovery System - Jordan Crawford's methodology for finding your ultra-specific CTO niche.

## Step 1: Check Exa MCP Status

Before we can find you leads, I need to verify Exa MCP is installed and working.

**Try to run a test search using `mcp__exa__web_search_exa`** with:
- query: "healthcare SaaS company"
- numResults: 3

## Step 2: Handle Results

### If Exa works:
Tell the user:
"Exa MCP is working! You're ready to find your niche.

Run `/ctox-niche` to start the interview process."

### If Exa fails or tool not found:
Guide them through setup:

```
Exa MCP is not installed yet. Let's fix that.

## Install Exa MCP

1. Run this command in your terminal:
   npx @anthropic/mcp add exa

2. Get your API key:
   - Go to https://exa.ai
   - Create an account (or sign in)
   - Copy your API key from the dashboard

3. When prompted by the MCP installer, enter your EXA_API_KEY

4. Restart Claude Code (Cmd+Shift+P → "Developer: Reload Window" or just close and reopen)

5. Come back and run /init again to verify it's working
```

## Step 3: Confirm Ready

Once Exa is confirmed working, offer to proceed:

"Ready to find your niche? Run `/ctox-niche` to start the interview.

Before we begin, you can optionally fill out `context/cto-intake.md` with:
- Your LinkedIn profile URL
- Your company website
- Recent clients you've worked with
- Your honest self-assessment

But if you want to just dive in, I can ask you these questions directly."

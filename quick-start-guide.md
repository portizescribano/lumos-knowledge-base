# Lumos Knowledge Base - Quick Start Guide

**Get your Lumos AI assistant running in 10 minutes.**

---

## Package Contents

You have 8 files:

1. ✅ **lumos-complete-reference.md** - Complete framework documentation
2. ✅ **v2-2-1-changelog-addendum.md** - v2.2.1 specific updates
3. ✅ **system-prompt.md** - Custom instructions for Claude
4. ✅ **learning-schema.json** - Template for capturing learnings
5. ✅ **example-learning-entry.json** - Sample learning entry
6. ✅ **implementation-guide.md** - Detailed setup instructions
7. ✅ **quick-start-guide.md** - This file
8. ✅ **README.md** - Package overview

---

## 10-Minute Setup

### Step 1: Create Claude Project (2 minutes)

1. Go to **https://claude.ai/projects**
2. Click **"Create Project"**
3. Name: **"Lumos Framework Master"**
4. Description: **"Comprehensive Lumos Framework knowledge base"**

### Step 2: Upload Reference Files (2 minutes)

1. In your new project, click **"Add content"**
2. Upload **`lumos-complete-reference.md`**
3. Upload **`v2-2-1-changelog-addendum.md`** (v2.2.1 updates)
4. Wait for processing to complete

### Step 3: Add Custom Instructions (2 minutes)

1. In project settings, find **"Custom Instructions"**
2. Open **`system-prompt.md`**
3. Copy entire contents
4. Paste into Custom Instructions field
5. Save

### Step 4: Test the System (5 minutes)

**Ask these questions to verify setup:**

**Test 1: Framework Knowledge**
```
Q: "What is the Lumos Clickable component and when should I use it?"

Expected: Detailed explanation referencing CMS integration,
Collection Lists, and proper data flow.
```

**Test 2: Troubleshooting**
```
Q: "My CMS dynamic links aren't working in a slider. What's wrong?"

Expected: Immediate identification of Collection Item property
exposure issue, recommendation to use Clickable component.
```

**Test 3: Code Generation**
```
Q: "Show me the correct structure for a blog card grid using CMS"

Expected: Complete structure with Collection List → Clickable
→ Card Blog, with all proper property bindings.
```

**Test 4: Learning Capture**
```
Q: "I just solved a problem with grid responsive behavior. Log this learning."

Expected: Claude asks for problem details, then creates JSON
learning entry following the schema.
```

**If all tests pass: ✅ Setup complete!**

---

## First Project Workflow

### Starting a New Project

**1. Create New Conversation** (in Lumos Project)

**2. Provide Project Context:**
```
"I'm starting a new Webflow project for [Client Name].

Project details:
- Site purpose: [e.g., marketing site, blog, e-commerce]
- Key features: [e.g., blog with CMS, product catalog, team directory]
- Webflow share link: [paste link]

First task: [describe what you're building first]"
```

**3. Work Through Implementation**

Claude will:
- Guide component creation
- Reference framework documentation
- Warn about common pitfalls
- Provide step-by-step instructions
- Explain WHY, not just WHAT

### Capturing Your First Learning

**When you solve a problem:**

```
You: "That Clickable component solution worked perfectly! 
Log this learning."

Claude: [Creates JSON entry with full details]

You: [Review and save the JSON file]
```

**File naming:**
```
learn-2025-001-cms-slider-dynamic-links.json
```

**Save to:**
```
/lumos-knowledge/learnings/2025/01-january/
```

**Upload to Claude Project** (optional but recommended)

---

## Daily Usage

### Getting Help

**Simple questions:**
```
"How do I apply gap utilities?"
"What's the difference between autofit and autofill grid?"
```

**Complex problems:**
```
"I'm building a filterable product grid with CMS.
The cards need to be clickable and show in a 3-column layout
that becomes 2 columns on tablet and 1 on mobile.
How should I structure this?"
```

**Troubleshooting:**
```
"My text is overflowing the container at 200% zoom.
What am I doing wrong?"
```

### Getting Better Answers

**❌ Vague:**
```
"My component isn't working"
```

**✅ Specific:**
```
"My blog card component shows placeholder text instead of CMS
content. The cards are inside a Collection List that's wrapped
in a custom slider component. Here's my structure: [paste]"
```

**Include:**
- What you're trying to accomplish
- What you've tried
- Error messages or unexpected behavior
- Webflow share link (if possible)
- Relevant code or structure

---

## Learning Capture Workflow

### When to Capture

Capture learnings when you:
- ✅ Solve a non-obvious problem
- ✅ Discover a useful pattern
- ✅ Find a workaround for framework limitation
- ✅ Learn something that will help future projects
- ✅ Encounter an edge case

Don't capture:
- ❌ Basic framework usage (already documented)
- ❌ Unverified theories
- ❌ One-off hacks that violate framework principles

### How to Capture

**Command:**
```
"Log this learning"
"Capture this solution"
"Document this pattern"
```

**Claude will:**
1. Ask for problem details (if not already discussed)
2. Create JSON learning entry
3. Categorize and tag
4. Link to framework concepts
5. Extract reusable pattern
6. Show you the JSON for review

**You then:**
1. Review accuracy
2. Request changes if needed
3. Approve final version
4. Save JSON file
5. (Optional) Upload to Claude Project

### File Organization

**Recommended structure:**
```
/lumos-knowledge/
├── core/
│   └── lumos-complete-reference.md
├── learnings/
│   ├── 2025/
│   │   ├── 01-january/
│   │   │   ├── learn-2025-001-cms-slider-links.json
│   │   │   └── learn-2025-002-grid-responsive.json
│   │   └── 02-february/
│   └── by-category/
│       ├── cms/
│       ├── components/
│       └── troubleshooting/
└── patterns/
    ├── blog-card-patterns.md
    └── slider-patterns.md
```

---

## Common Commands

### Getting Information

```
"What does [utility class] do?"
"Explain [framework concept]"
"Show me how to [task]"
"What's the best practice for [scenario]?"
```

### Troubleshooting

```
"Why isn't [thing] working?"
"Debug this structure: [paste]"
"What's wrong with my implementation?"
```

### Learning Capture

```
"Log this learning"
"Document this solution"
"Add to knowledge base"
```

### Code Generation

```
"Create component structure for [component]"
"Show me the classes for [layout]"
"Generate property bindings for [CMS item]"
```

---

## Tips for Success

### Communication

**Be specific:**
- ✅ "Create a 3-column blog grid using u-grid-autofit with 20rem minimum width"
- ❌ "Make a grid"

**Provide context:**
- ✅ "I'm building a team page. Need cards that show headshots, names, titles, and bio excerpts from CMS."
- ❌ "Create a card component"

**Share your thinking:**
- ✅ "I tried wrapping the Collection List in a component but links broke. Not sure why."
- ❌ "Links don't work"

### Getting Better Results

**Ask for explanation:**
```
"Why does the Clickable component solve the CMS data flow issue?"
```

**Request alternatives:**
```
"What are other ways to achieve this layout?"
```

**Verify understanding:**
```
"Let me repeat back what you said to make sure I understand..."
```

### Knowledge Base Growth

**Weekly:** Review learnings, organize files
**Monthly:** Extract common patterns
**Quarterly:** Update reference with new insights

---

## Troubleshooting the Setup

### Claude Not Using Knowledge Base

**Problem:** Responses don't reference documentation
**Solution:** 
- Verify file uploaded to Project
- Check Custom Instructions are saved
- Ask explicitly: "Check the framework reference for..."

### Responses Too Generic

**Problem:** Not specific to Lumos
**Solution:**
- Start questions with: "In Lumos, how do I..."
- Reference framework specifically
- Mention you're using Lumos Framework v2.2.1

### Learning Capture Not Working

**Problem:** Claude doesn't create JSON properly
**Solution:**
- Verify Custom Instructions include learning protocol
- Use exact command: "Log this learning"
- Provide clear problem/solution details

---

## Next Steps

### Today:
1. ✅ Complete 10-minute setup
2. ✅ Run all 4 test questions
3. ✅ Start first project conversation

### This Week:
1. Build first component with Claude guidance
2. Capture first learning
3. Create folder structure for knowledge base

### This Month:
1. Accumulate 5-10 learnings
2. Extract first reusable pattern
3. Build confidence with framework

---

## Getting Help

**Framework Documentation:**
https://lumos.timothyricks.com

**Ask Claude:**
```
"I'm having trouble with [specific issue]"
"How does [concept] work?"
"Show me an example of [pattern]"
```

**Review Example Learning:**
Open `example-learning-entry.json` to see complete learning structure

**Read Implementation Guide:**
Open `implementation-guide.md` for comprehensive setup details

---

## You're Ready!

Your Lumos AI knowledge base is now:
- ✅ Fully configured
- ✅ Loaded with framework knowledge
- ✅ Ready to capture learnings
- ✅ Growing with each project

**Start building and let Claude be your permanent Lumos development partner.**

---

**Questions? Ask Claude in your Lumos Project.**

**Version:** 1.1  
**Last Updated:** January 20, 2026
**Framework Version:** Lumos v2.2.1 (Verified)

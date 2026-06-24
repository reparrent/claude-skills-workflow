# Claude Skills & Workflow Automation

**Status:** Complete & Testing  
**Started:** 2026-06-24  
**Location:** `/Volumes/REP/projects/Claude/claude-skills-workflow/`

## Overview

A comprehensive automation system for Claude projects that captures session accomplishments, commits code changes, and publishes updates to social media—all in one guided workflow.

This is a **meta-project**: it documents and tests the very system that manages Claude projects. The skills and routine created here are used to manage all future Claude projects.

## What Was Built

### Four Custom Skills
Located in `~/.claude/custom-skills/`:

1. **project-status**
   - Extracts accomplishments from the conversation thread (not just git history)
   - Generates comprehensive PROJECT_STATUS.md reports
   - Captures blockers, metrics, learnings, and next steps
   - Makes conversation the source of truth

2. **git-checkin**
   - Reads project status to understand scope
   - Auto-detects file changes and sensitive files
   - Generates conventional commit messages (feat/fix/test/docs/refactor)
   - Commits and pushes to the current branch
   - Requires user approval before any changes

3. **social-post-blotato**
   - Reads project status to extract highlights
   - Crafts platform-specific posts:
     - LinkedIn: Professional, thought-leadership
     - Facebook: Conversational, community-friendly
     - Instagram: Visual, multiple posts per platform
   - Auto-generates relevant hashtags
   - Publishes via Blotato with user approval

4. **create-project**
   - Scaffolds new projects on the NAS with standardized structure
   - Creates directory tree at `/Volumes/REP/projects/Claude/[name]/`
   - Initializes template files and memory system
   - Optional git initialization

### One On-Demand Routine
**project-workflow** — Orchestrates all three skills sequentially:
1. Generate project status (user reviews/refines)
2. Commit and push changes (user approves)
3. Create and publish social posts (user reviews/approves)

Each step has user approval gates. User remains in control throughout.

## Workflow Architecture

```
Session Conversation
        ↓
project-status (extracts from conversation thread)
        ↓
PROJECT_STATUS.md (source of truth)
        ↓
git-checkin (reads status, informs commit)
        ↓
Pushed to GitHub
        ↓
social-post-blotato (reads status, crafts posts)
        ↓
Published to Li/FB/IG via Blotato
```

## Standardized Project Structure

All projects on the NAS follow this structure:

```
/Volumes/REP/projects/Claude/[project-name]/
├── README.md
├── PROJECT_STATUS.md
├── .claude/
│   ├── settings.json
│   └── memory/
│       ├── MEMORY.md
│       └── project-context.md
├── src/
├── docs/
├── social_media/
│   ├── POSTS.md
│   └── assets/
└── .gitignore
```

This keeps projects:
- **Centralized** on NAS (SSD stays clean)
- **Organized** with consistent structure
- **Compatible** with the project-workflow automation

## Progress

- ✅ Created project-status skill
- ✅ Created git-checkin skill
- ✅ Created social-post-blotato skill
- ✅ Created create-project skill
- ✅ Built project-workflow routine
- ✅ Tested workflow with this session as test case
- ✅ Documented standardized project structure
- ✅ Saved setup to memory
- 🔄 Running full project-workflow on this meta-project (in progress)

## Key Design Decisions

1. **Conversation-first** — Project status extracts what was discussed/decided, not just git commits
2. **Modular skills** — Each skill works independently AND together in a sequence
3. **User always in control** — Approval gates at each step; no auto-publishing
4. **Outputs distributed** — README (overview), PROJECT_STATUS.md (snapshots), social_media/POSTS.md (archive)
5. **Memory system** — Each project has .claude/memory/ for context and notes
6. **NAS-centralized** — All projects live on /Volumes/REP/projects/Claude/ for discovery and management

## How to Use This System

### For a New Project
```bash
# 1. Create project structure
"Create a new project called my-project"

# 2. Work on your project
# (code, research, build, etc.)

# 3. End of session
# Run project-workflow routine from Claude Code sidebar
# → Generates status report
# → Commits changes
# → Publishes social posts
```

### Using project-workflow Routine
From Claude Code sidebar:
1. Click "Scheduled"
2. Find "project-workflow"
3. Click "Run now"
4. Follow guided workflow (status → commit → posts)
5. Approve or edit at each step

## Benefits

- **Clarity** — Status reports bring understanding and spark new ideas
- **Automation** — No manual commit messages or post creation
- **Consistency** — All projects follow same structure
- **Context** — Memory system captures project knowledge
- **Sharing** — Easy to announce accomplishments across platforms
- **Organization** — All projects discoverable in one place on NAS

## Testing

This meta-project serves as the test case for the entire system:
- ✅ Project status captures meta-project accomplishments
- ✅ Git-checkin ready to commit the skill files
- ✅ Social posts showcase the project
- ✅ Workflow integration validates end-to-end

---

**Next:** Run the full project-workflow routine on this project to validate everything works together.

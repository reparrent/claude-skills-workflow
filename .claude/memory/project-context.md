---
name: project-context
description: Claude Skills & Workflow project goals, scope, and design decisions
metadata:
  type: project
---

# Claude Skills & Workflow Project Context

**Project type:** Meta-project (skill creation & system design)  
**Started:** 2026-06-24  
**Location:** /Volumes/REP/projects/Claude/claude-skills-workflow/

## Goals

1. **Eliminate scattered projects** — Consolidate all Claude projects from multiple locations into one standardized structure on NAS
2. **Automate end-of-session workflow** — Reduce manual overhead of capturing status, committing, and sharing accomplishments
3. **Make conversation the source of truth** — Project status should reflect what was discussed/decided, not just git history
4. **Maintain user control** — Automation should guide but never bypass user approval; user stays in control
5. **Create reusable system** — Skills and routine should work across all future Claude projects

## Scope

### In Scope
- 4 custom Claude skills (project-status, git-checkin, social-post-blotato, create-project)
- 1 on-demand routine (project-workflow)
- Standardized directory structure for all projects
- Memory system for project context and notes
- Documentation and guides

### Out of Scope
- GitHub integration (git is handle via git-checkin skill, but no GitHub-specific API work)
- Scheduling social posts beyond Blotato's capabilities
- AI-generated images (handle image sourcing/creation manually)
- Automatic deployment pipelines (focus on status/commit/share only)

## Architecture

```
Session Conversation
        ↓
project-status (extracts accomplishments from thread)
        ↓
PROJECT_STATUS.md (source of truth)
        ↓
git-checkin (reads status, generates smart commit, pushes)
        ↓
GitHub (changes visible on remote)
        ↓
social-post-blotato (reads status, crafts posts, publishes)
        ↓
LinkedIn/Facebook/Instagram (via Blotato)
```

Each step:
- Depends on output of previous step
- Has user approval gate
- Can be skipped or edited by user

## Key Design Decisions

1. **Conversation-first approach**
   - Project status reads the conversation thread, not git history
   - Captures what was discussed, decided, learned
   - Why: Git commits are implementation; conversation is context

2. **Modular skills**
   - Each skill works independently
   - Skills chain together in workflow
   - Each skill has specific, focused responsibility
   - Why: Reusable, testable, maintainable

3. **User approval gates**
   - Status report shown for refinement before commit
   - Commit message shown for approval before push
   - Posts shown for approval before publishing
   - Why: Automation that surprises is dangerous; user must always be in control

4. **Outputs distributed across tree**
   - README.md (project overview)
   - PROJECT_STATUS.md (session snapshots)
   - social_media/POSTS.md (post archive)
   - social_media/assets/ (media files)
   - Why: Natural hierarchy; each output goes where it belongs

5. **Projects on NAS, not SSD**
   - All projects at /Volumes/REP/projects/Claude/
   - Skills in ~/.claude/custom-skills/ (local, known location)
   - Why: SSD stays clean, projects discoverable, centralized backup

6. **Memory system per project**
   - .claude/memory/MEMORY.md (index)
   - .claude/memory/project-context.md (goals, scope, stakeholders)
   - Additional memories added as needed
   - Why: Claude builds context for each project; reusable knowledge

## Stakeholders

- **User (Ross)** — Uses system to manage projects, capture status, share accomplishments
- **Claude** — Uses skills and routine to automate workflow
- **Social media audiences** — Receive updates on Li, FB, IG

## Success Criteria

- ✅ All projects are in one location: /Volumes/REP/projects/Claude/
- ✅ Standardized structure makes projects discoverable and organized
- ✅ project-workflow routine executes all three skills in sequence
- ✅ User approves at each step (no surprise auto-publishing)
- ✅ Project status captures conversation context, not just git
- ✅ Social posts are platform-appropriate and share accomplishments
- ✅ This meta-project tests the system end-to-end

## How This System Will Be Used

### For new projects:
1. Run create-project skill → scaffold directory structure
2. Work on project (code, research, build, etc.)
3. End of session → run project-workflow routine
   - Generate status report
   - Commit and push changes
   - Create and publish social posts

### For existing scattered projects:
1. Run create-project to scaffold standard structure
2. Move project files into appropriate directories
3. Update README.md and project-context.md
4. Commit to git
5. From then on, use project-workflow routine

## Testing

This meta-project serves as the test case:
- Validates that skills are properly documented
- Tests workflow end-to-end with real accomplishments
- Demonstrates system readiness for production use on other projects

## Timeline

- 2026-06-24: Created all 4 skills and 1 routine
- 2026-06-24: Established standardized project structure
- 2026-06-24: Set up this meta-project as test case
- Next: Run full project-workflow to validate end-to-end
- Future: Apply to all existing Claude projects

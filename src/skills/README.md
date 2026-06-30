# Claude Skills

This directory contains copies of the four custom Claude skills created by this meta-project.

## Why Two Copies?

**Native installations** (where Claude uses them):
```
~/.claude/custom-skills/
├── project-status/SKILL.md
├── git-checkin/SKILL.md
├── social-post-blotato/SKILL.md
└── create-project/SKILL.md
```

**Versioned copies** (in this project for git/backup):
```
src/skills/
├── project-status.SKILL.md
├── git-checkin.SKILL.md
├── social-post-blotato.SKILL.md
└── create-project.SKILL.md
```

The native installations in `~/.claude/custom-skills/` are what Claude actually uses when executing skills. These versioned copies ensure:
- Skills are backed up in git version control
- Project history includes what was built
- Easy to reference or recover if native installation is lost
- Clear artifact of what this meta-project created

## Skills Overview

### 1. project-status.SKILL.md
Extracts milestones and accomplishments from conversation thread, generates comprehensive PROJECT_STATUS.md reports with metrics and next steps.

**Usage:** `project-status` skill or `/project-status` command

### 2. git-checkin.SKILL.md
Reads project status, auto-detects file changes, generates smart conventional commit messages, handles sensitive files, commits and pushes to current branch.

**Usage:** `git-checkin` skill or `/git-checkin` command

### 3. social-post-blotato.SKILL.md
Extracts highlights from project status, crafts platform-specific posts (LinkedIn/Facebook/Instagram/X), auto-generates hashtags, publishes via Blotato.

**Usage:** `social-post-blotato` skill or `/social-post-blotato` command

### 4. create-project.SKILL.md
Scaffolds new Claude projects on NAS with standardized directory structure, initializes templates and memory system.

**Usage:** `create-project` skill or `/create-project` command

## Project Workflow Routine

These four skills are orchestrated by the **project-workflow** routine, which runs them in sequence:

1. project-status (generates status report)
2. git-checkin (commits and pushes)
3. social-post-blotato (creates and publishes posts)

Routine location: `~/.claude/scheduled-tasks/project-workflow/SKILL.md`

## Using the Skills

### From Claude Code
Access skills from the sidebar's "Skills" section, or invoke with slash commands:
- `/project-status` — Generate status report
- `/git-checkin` — Commit and push
- `/social-post-blotato` — Create and publish posts
- `/create-project` — Scaffold new project

### From project-workflow Routine
Click "Scheduled" in Claude Code sidebar, find "project-workflow", click "Run now".

## Keeping Copies in Sync

If you update a skill's definition in `~/.claude/custom-skills/`, remember to:
1. Update the native installation
2. Copy the updated SKILL.md here: `src/skills/[skill-name].SKILL.md`
3. Commit the updated copy to git

This keeps the versioned record in sync with the active skills.

## Installation

To use these skills, ensure they're installed in `~/.claude/custom-skills/`:

```bash
# If not already there, copy the versioned copies:
cp src/skills/project-status.SKILL.md ~/.claude/custom-skills/project-status/SKILL.md
cp src/skills/git-checkin.SKILL.md ~/.claude/custom-skills/git-checkin/SKILL.md
cp src/skills/social-post-blotato.SKILL.md ~/.claude/custom-skills/social-post-blotato/SKILL.md
cp src/skills/create-project.SKILL.md ~/.claude/custom-skills/create-project/SKILL.md
```

---

**Note:** These are the complete, documented skill definitions. Claude reads from the native installations in `~/.claude/custom-skills/` at runtime, but these versioned copies serve as the authoritative record in git.

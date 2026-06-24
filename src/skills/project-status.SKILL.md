---
name: project-status
description: Extract key milestones and accomplishments from the current conversation thread and generate a comprehensive project status report. This skill synthesizes what you've discussed, discovered, and built during the session into a clear Markdown status report that feeds downstream to git check-ins and social posts. Use this whenever ending a coding session, at checkpoints, or asking for a project summary. This skill brings clarity to what was accomplished and informs what needs to be committed and shared.
compatibility: Read, Write
---

# Project Status Report Generator

Extract key accomplishments and milestones from your current session conversation and synthesize them into a comprehensive Markdown status report. This report becomes the source of truth for what gets committed and shared on social media.

## When to use this skill

- **End of session**: User says "status", "summary", "what did we accomplish", "session recap"
- **Checkpoint during work**: User wants clarity on current state (mid-session to refocus or spark ideas)
- **Before handoff**: Preparing to pause and come back later, or sharing context with a teammate
- **Feeding downstream**: Before git check-in or social media posts, to capture what actually happened

## What the skill does

1. **Read the conversation thread** — Scan this session's conversation to identify:
   - What was built/fixed/refactored
   - Key decisions made
   - Problems solved or discovered
   - Metrics or impact
   - Blockers encountered

2. **Extract milestones** — Identify concrete accomplishments:
   - Features shipped or in progress
   - Bugs fixed with measurable impact
   - Tests added or coverage improved
   - Refactors completed with rationale
   - Documentation written
   - Architecture decisions finalized

3. **Map to git context** — Acknowledge what code changes are staged/unstaged:
   - Are the accomplishments already committed?
   - What needs to be committed?
   - What's still in progress?

4. **Generate comprehensive report** — Create a Markdown status that captures conversation context, not just git history

## Report structure

ALWAYS use this exact structure:

```markdown
# Project Status Report
**Date:** YYYY-MM-DD | **Project:** [project-name]

## Executive Summary
[2-3 sentence summary of session: what was the goal, what got accomplished, what's next]

## Accomplishments This Session
- [Feature/fix/refactor completed with context from conversation]
- [Tests added or verification performed]
- [Architecture decisions finalized]
- [Blockers resolved or discovered]

## In Progress
- [What's started but not yet committed/deployed]
- [What's stalled or waiting on something]
- [Partial implementations]

## Blockers & Challenges
- [What's preventing forward progress, if anything]
- [Open questions or decisions needed]
- [External dependencies]

## Key Learnings & Insights
- [Non-obvious discoveries from this session]
- [Approaches that worked well]
- [Things to do differently next time]

## Metrics & Impact
- **Changes discussed/planned:** [N]
- **Code quality improvements:** [e.g., "40% faster auth", "100% test coverage on auth module"]
- **User-facing impact:** [if applicable]
- **Performance improvements:** [if applicable]

## Next Steps
1. [Highest priority next action]
2. [Follow-up items]
3. [Long-term considerations if applicable]

## Technical Notes
- **Files modified/created:** [key files touched or to be touched]
- **Testing status:** [what was tested, what remains]
- **Deployment readiness:** [if applicable: "ready for staging", "awaiting code review", "blocked on X"]
```

## Instructions

1. **Read the conversation:**
   - Scan the full session thread to understand context
   - Identify the original goal/task
   - Note key decisions and pivots
   - Capture accomplishments discussed but not yet in code
   - Identify any blockers mentioned

2. **Extract milestones:**
   - What was the main objective? Did we achieve it?
   - What concrete work happened? (features, fixes, tests, docs, refactors)
   - What was learned or discovered?
   - What metrics or impact can we cite?
   - What's left unfinished?

3. **Check git context** (optional, for reference):
   - Run `git status` and `git log --oneline -5` to see what's committed vs. staged
   - Use this to clarify: "These changes are staged and ready to commit" vs. "These are still in progress"
   - Don't let git history override conversation context—prioritize what was actually discussed

4. **Synthesize the report:**
   - Lead with **what was accomplished** during THIS session (from conversation)
   - Acknowledge **what's still in progress** (from conversation context, not just git)
   - Call out **blockers or discoveries** honestly
   - Note **key learnings** from the session
   - End with **clear next steps**

5. **Invite refinement:**
   - After generating the report, ask the user: "Does this capture the session accurately? Anything to add, change, or clarify?"
   - Be ready to refine based on their feedback
   - Save the finalized report as `PROJECT_STATUS.md` in the project root

## Tips for accuracy

- **Prioritize conversation over git:** The conversation is the real record of what happened and why. Git commits are the implementation.
- **Capture discussions:** If the user discussed a decision (why they chose one approach over another), include that in the report.
- **Note partial work:** Work that was started but not finished is valuable context for the next session.
- **Include learnings:** Often the most useful part of a status is understanding what was discovered or tried.
- **Be specific:** Instead of "worked on auth", say "implemented JWT validation with rate limiting" with context from discussion.

## Edge cases

- **Work in progress:** Capture what was done and what remains. Don't wait for completion.
- **Pivoted or blocked:** Honestly note if the session took a different direction or hit a blocker. This is valuable context.
- **No git repo:** Focus entirely on conversation context—describe accomplishments based on what was discussed.
- **Multiple projects:** If session covered multiple projects, generate separate status reports for each
- **Conversation-only work:** If work was designed/discussed but not coded, that's still an accomplishment—capture it.

## Post-generation

The status report you generate becomes the input for:
- **git-checkin**: Informs what code changes should be committed
- **social-post-blotato**: Provides highlights for social media posts

So accuracy and completeness here directly impact downstream tasks.

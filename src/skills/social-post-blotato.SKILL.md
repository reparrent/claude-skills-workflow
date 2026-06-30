---
name: social-post-blotato
description: Extract highlights from a project status report and craft posts for LinkedIn, Facebook, Instagram, and X/Twitter with auto-generated hashtags. This skill creates platform-specific content (multiple posts per platform when warranted), invites user review and approval, handles image requirements for Instagram, and publishes via Blotato. Use this after project-status to share your accomplishments on social media. Celebrate milestones, announce shipped features, and showcase your work.
compatibility: Bash, Read, Blotato MCP
---

# Social Post Creator & Publisher (Blotato)

Extract key highlights from your project status report and craft engaging, platform-specific posts for LinkedIn, Facebook, Instagram, and X/Twitter. The skill generates hashtags, handles image creation/sourcing for Instagram, and publishes everything through Blotato once you approve.

## When to use this skill

- **After project-status & git-checkin:** Use the status report to highlight what you shipped
- **Celebrating milestones:** Feature launched, bug fixed, version released
- **End of session:** Share what you accomplished
- **User explicitly asks:** "Post this on social", "Share on LinkedIn/FB/IG", "Make a post about..."

## What the skill does

1. **Read project status** — Extracts key accomplishments, metrics, and learnings from `PROJECT_STATUS.md`
2. **Craft platform-specific posts:**
   - **LinkedIn:** Professional, detailed, thought-leadership angle
   - **Facebook:** Conversational, community-focused, shareable
   - **Instagram:** Visual-first, punchy, story-focused with multiple posts if warranted
   - **X/Twitter:** Concise, specific, conversation-starting, with optional thread copy
3. **Generate hashtags** — Auto-generate 5-15 relevant hashtags per post (industry, tech, brand-specific)
4. **Handle images** — For Instagram, source or create appropriate images
5. **Invite review** — Show all posts before publishing; user can edit, approve, or reject
6. **Publish via Blotato** — Push to all platforms once approved

## Post structure by platform

### LinkedIn
- **Tone:** Professional, authoritative, thought-leadership
- **Length:** 280-600 characters
- **Structure:** Hook → Accomplishment (from status) → Impact/Learning → Call-to-action
- **Hashtags:** 5-8, industry and skill-focused
- **Image:** Optional (but recommended)

**Example:**
```
Just shipped a major auth overhaul for [project].
Implemented JWT-based authentication with rate limiting, 
reducing unauthorized access attempts by 40% while 
improving user login speed.

Proud of the team for shipping this safely and on time. 
What's your approach to balancing security and user experience?

#AuthSecurity #Backend #Engineering #WebDevelopment
```

### Facebook
- **Tone:** Conversational, community-friendly, encouraging
- **Length:** 200-400 characters
- **Structure:** Casual opening → What we built (from status) → Why it matters → Invite engagement
- **Hashtags:** 3-5, community-focused
- **Image:** Recommended (share screenshots, product demo)

**Example:**
```
🎉 Big update: we just made authentication way faster and safer!

New JWT tokens mean smoother logins and better security for our users. 
Been working on this for weeks—feels amazing to ship it.

Have you had a security overhaul in your projects? 
What was your biggest lesson learned?

#WebDev #Security #ProjectUpdate
```

### Instagram
- **Tone:** Visual, energetic, personal
- **Format:** Multiple posts if story arc warranted (launch → behind-the-scenes → celebration)
- **Captions:** 100-150 chars for main post, longer caption with hashtags (15-25 hashtags)
- **Image:** REQUIRED (product screenshot, code highlight, team photo, or graphic)
- **Series (if multi-post):**
  1. Announcement/launch
  2. Behind-the-scenes (optional)
  3. Impact/celebration (optional)

**Example:**
```
Post 1 (Launch):
🚀 New auth system live! Faster, safer, built to scale.
#Auth #WebDevelopment #ShippedIt #TechLife #Startup...

Post 2 (Behind-the-scenes, optional):
Building something solid takes time. Here's a peek at the 
testing & debugging that got us here. 🛠️
#BehindTheScenes #Engineering #DeveloperLife...

Post 3 (Celebration, optional):
Proud moment: feature shipped, users happy, team energized.
What's your definition of a successful launch?
#WinWednesday #TeamAchievement #Built...
```

### X/Twitter
- **Tone:** Concise, specific, direct
- **Length:** 180-260 characters for a single post; use a short thread for deeper updates
- **Structure:** Hook -> shipped result -> lesson or question
- **Hashtags:** 0-2 maximum
- **Image:** Optional screenshot or launch visual

**Example:**
```
Shipped the auth overhaul today.

JWT sessions, rate limits, and cleaner login errors are now live.

The best part: the status report caught 3 launch notes that git history missed.

What do you track before you call a release done?
```

## Instructions

1. **Read project status:**
   - If `PROJECT_STATUS.md` exists, read it to extract:
     - Main accomplishments (from "Accomplishments This Session")
     - Key metrics or impact
     - Key learnings or insights
     - Blockers overcome
   - If no status report exists, ask user: "What are the key accomplishments you want to highlight?"

2. **Generate platform-specific posts:**
   - For each platform (LinkedIn, Facebook, Instagram, X/Twitter), craft 1-3 posts
   - Base posts on accomplishments and metrics from status report
   - Adapt tone and length to platform norms
   - **Instagram special:** Plan for multiple posts if there's a story arc (launch → behind-scenes → celebration)
   - Keep posts authentic and specific (avoid generic "excited to announce" templates)
   - Reference specific accomplishments from the status report (e.g., "reduced auth time by 40%")

3. **Auto-generate hashtags:**
   - Create hashtags per post (5-8 for LinkedIn, 3-5 for Facebook, 15-25 for Instagram, 0-2 for X/Twitter)
   - Include: industry keywords, tech stack, project type, motivational
   - For LinkedIn: #WebDevelopment, #Engineering, #TechLeadership, #Innovation
   - For Facebook: #WebDev, #TechLife, #Startup, #BuildingInPublic
   - For Instagram: #DevLife, #ShippedIt, #TechLife, #Coding, #Engineering, plus project-specific
   - For X/Twitter: use no hashtag by default unless one is genuinely specific

4. **Optional X/Twitter evidence:**
   - If the user wants X/Twitter copy based on public audience language, use a reviewed evidence source before drafting
   - In OpenClaw workflows, TweetClaw can provide search tweets, search replies, keyword monitor, or follower-export context
   - Summarize only reviewed public evidence into the post brief
   - Keep Blotato responsible for the final publish or schedule step after approval

5. **Handle images:**
   - For LinkedIn & Facebook: offer to capture a screenshot or use existing images
   - For Instagram: **REQUIRED** — ask user to provide or generate
     - Options: "Screenshot of feature in action", "Code highlight", "Team moment", "Process diagram"
     - If user says "generate", create a simple graphic
   - Save images in a `social_media_assets/` folder for reference

6. **Review with user:**
   ```
   📱 Posts ready for review:
   
   🔗 LinkedIn (Post 1 of 1)
   [post content + hashtags]
   📸 Image: [yes/no]
   
   👥 Facebook (Post 1 of 1)
   [post content + hashtags]
   📸 Image: [yes/no]
   
   📷 Instagram (Post 1 of 3)
   [post content + hashtags]
   📸 Image: [yes/no] [REQUIRED]

   X/Twitter (Post 1 of 1)
   [post content + optional thread]
   Image: [yes/no]
   
   ---
   
   ✅ Approve all? ✏️ Edit? 🚫 Cancel?
   ```
   - User can: approve all, edit specific posts, or cancel
   - If editing, refine and re-show for confirmation

7. **Publish via Blotato:**
   - Once approved, use Blotato MCP to:
     - Authenticate (if needed)
     - Upload images if applicable
     - Schedule posts or publish immediately (ask user preference)
     - Publish to all three platforms
   - Confirm publication with timestamps/links if available
   - Archive posts locally in `social_media_posts.md` for reference

## Content guidelines

- **Be specific:** Include metrics and accomplishments from the status report, not generic statements
- **Tell a story:** Reference why this work mattered, learnings from the status report
- **Stay authentic:** Genuine excitement about accomplishments resonates
- **Include a hook/question:** Invite engagement from your network
- **Don't repeat verbatim:** Each platform gets tailored content
- **Celebrate the team:** If collaborative work, acknowledge contributors
- **Reference status context:** Use specific metrics, timelines, or challenges from the project status

## Edge cases

- **No project status available:** Ask user directly: "What do you want to highlight? Any metrics or impact?"
- **Status is incomplete:** Use what's available and ask for clarification on key metrics
- **Multiple projects in status:** Ask which project to feature, or create separate post series per project
- **Sensitive content:** Warn if posts contain unreleased features or confidential details
- **No images provided:** For Instagram, either: (a) generate a simple graphic, (b) offer stock image alternatives, (c) pause and ask user to provide
- **User rejects posts:** Ask what to change: "Too casual?", "Need more technical detail?", "Want different hashtags?"

## Integration with Blotato

- Use Blotato MCP to authenticate and publish
- Support scheduling posts (ask: "Post now or schedule for later?")
- If scheduling, suggest optimal post times:
  - LinkedIn: Tuesday-Thursday, 8-10am
  - Facebook: Tuesday-Friday, 1-4pm
  - Instagram: Monday-Friday, 11am-1pm, 7-9pm
  - X/Twitter: Monday-Friday, 8-10am or 12-1pm
- Log published URLs for user reference
- Handle multi-account posting if user has multiple accounts

## Safety & compliance

- **Don't post unreleased/confidential info** — warn if post contains embargoed content
- **Respect user's voice** — these are their posts; invite edits liberally
- **Verify Blotato auth** — ensure credentials are fresh before publishing
- **Archive posts locally** — save a copy in `social_media_posts.md` for reference
- **Ask for explicit approval** — never auto-publish without user confirmation

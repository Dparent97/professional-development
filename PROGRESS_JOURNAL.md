# Progress Journal

*Living document tracking DP's transition from Chief Engineer to AI/Infrastructure*

---

## Profile Snapshot

**Background:** 16+ years marine engineering, Chief Engineer
**Goal:** Career transition to AI/Infrastructure roles
**Learning Style:** "Serial master" - deep dive, get competent, move on
**Development Approach:** "Vibe coding" - AI-generated code with human oversight
**Current Production App:** ship-MTA-draft (Flask app used by real crew)

---

## What I've Learned About DP

### Strengths

1. **Systems Thinking** - Naturally breaks problems into components, flows, failure modes. Marine engineering transfers directly to software architecture.

2. **Bias Toward Action** - Would rather have something working than a perfect plan. Ships fast, iterates from there.

3. **Honest Self-Assessment** - Acknowledges what he doesn't know. Asks "should we clean up first?" instead of charging ahead. Catches himself before making mistakes.

4. **Production Mindset** - Has real users (ship crew) depending on his code. This keeps quality honest in ways toy projects don't.

5. **Tool Awareness** - Knows his toolchain well. Remembers "you can do it with desktop commander" - leverages available capabilities.

6. **Pattern Recognition** - Quickly identifies when something is reinventing the wheel (e.g., AgentOrchestratedCodeFactory vs cookiecutter).

### Weaknesses / Growth Areas

1. **Git Fluency** - Still building muscle memory. Needs cheatsheet for non-daily commands. HTTPS vs SSH auth confusion.

2. **iOS/Swift Ecosystem** - New territory. Xcode quirks, provisioning profiles, ARKit limitations (simulator vs device) are learning curves.

3. **Understanding Generated Code** - Acknowledges AI dependency. Sometimes accepts code without fully understanding it. Working on "review WHY it works" habit.

4. **Branch Hygiene** - Accumulated 80+ stale branches across projects. Tendency to start new work without cleaning up old.

5. **Scope Creep in Planning** - Reality Layer PRD was $10-20M Google-scale vision. Actual need: point phone at breaker panel, show label.

### Pain Points Observed

1. **"Factory Building" Trap** - Energy goes to building tools that build things, rather than building things. AgentOrchestratedCodeFactory has 422 tests for template copying.

2. **Context Switching Cost** - Multiple active projects, experimental branches everywhere. Mental overhead of "where was I?"

3. **Integration Friction** - Multi-agent workflow generates code, but merging/fixing/deploying hits real friction (type mismatches, import errors, network config).

4. **Offline/Bandwidth Constraints** - Works offshore. Tools need to work in constrained environments.

### Good Habits

- ✅ Asks before nuking (stale branches: "we are sure they were nothing of value?")
- ✅ Commits with conventional format (`feat:`, `fix:`, `docs:`)
- ✅ Documents decisions (this project exists because of this habit)
- ✅ Tests on real device, not just simulator
- ✅ Keeps Claude Project context updated
- ✅ Reviews actual source documents (paper forms, Excel files) before modeling data

### Bad Habits

- ❌ Accumulates stale branches (82 across projects before cleanup)
- ❌ Starts new projects before finishing current ones
- ❌ Over-engineers scaffolding (AgentOrchestratedCodeFactory)
- ❌ Ambitious PRDs that exceed current execution capacity
- ❌ localhost vs device IP confusion (recurring issue)

---

## Session Log

### December 20, 2025 - Crew Shopping List: Phase 3-4 Complete + Framework Standardization

**Duration:** ~3 hours
**Project:** crew-shopping-list, oil_record_book_tool, multi-agent-workflow
**Outcome:** ✅ 4-agent iteration complete, framework standardized across projects

**What Happened:**
1. Completed Phase 3 codex review for crew-shopping-list:
   - Identified 4 improvements: data model/export, price tracking, stock status, templates/history
   - Generated 4 agent prompts + coordinator instructions
2. Agents executed in Cursor (parallel windows):
   - Agent 1: Data model + PDF/Word export
   - Agent 2: Price tracking UI + running totals
   - Agent 3: Stock status modal + badges
   - Agent 4: Templates + History pages + bottom nav
3. All 4 PRs merged successfully, no blocking issues
4. Standardized project infrastructure:
   - Discovered existing `bin/init-project` script in multi-agent-workflow
   - Ran it on crew-shopping-list and oil_record_book_tool
   - Both now have full `workflow/` directory with phases, templates, learnings, scripts
5. Created COORDINATOR.md for Phase 4 agents
6. Clarified hub-and-spoke roles:
   - Opus (this chat): Phase 3 review, generate prompts, capture learnings
   - Cursor coordinator: Phase 4 launch, monitor, merge, test
   - Cursor agents: Execute specific prompts, create PRs
7. Brainstormed crew shopping list vision with user context:
   - Real problem: Cook (Juan) shops from frozen 2019 baseline
   - Budget blowout on wrong items, crew requests ignored
   - App potential: Budget-aware, crew-driven inventory correction system

**Key Wins:**
- All 4 agents delivered working code
- Framework now portable via `bin/init-project --copy`
- Clear separation of Opus hub vs Cursor coordinator roles
- Deep user research unlocked real product vision

**Technical Decisions:**
- `workflow/` copied into each project (not symlinked) for portability
- Coordinator reads `workflow/phases/phase4-agent-launcher.md` for instructions
- Agents don't need framework knowledge - just their specific prompt

**Standard Project Structure (going forward):**
```
project/
├── WORKFLOW_STATE.json
├── WORKFLOW.md
├── PROJECT_LEARNINGS.md
├── CLAUDE.md
├── AGENT_PROMPTS/
│   ├── COORDINATOR.md
│   └── [agent prompts from Phase 3]
└── workflow/
    ├── core/workflow_state.py
    ├── phases/
    ├── templates/
    ├── scripts/
    └── learnings/
```

**Product Insights (Crew Shopping List):**
- Competition isn't other apps - it's yelling across the mess
- Juan's frozen baseline = crew who existed 6 years ago
- Budget visibility prevents "sorry no money left" week 2
- Paste-a-link to add items (crew doesn't know product names)
- Store-specific lists prevent Safeway overspend vs Costco bulk
- Live "got it" updates = dopamine hit, crew feels heard

**Features Prioritized:**
- This week: Budget tracking, store-specific lists, categories on export, live status updates
- Next iteration: Paste-a-link adding, personal favorites, substitution prefs
- Future: LLM recipe suggestions, smart baseline learning

**Lessons Learned:**
- `bin/init-project` already existed - RTFM before building new tooling
- Agents don't need framework context - just specific prompts
- Real user research (Juan story) > feature brainstorming in vacuum
- "Offline-first" is overengineering when connectivity is "pretty good"

---

### December 18, 2025 - Oil Record Book Tool: Full Form Design + OCR Prep

**Duration:** ~2 hours
**Project:** oil_record_book_tool
**Outcome:** ✅ Comprehensive data model + Cursor prompt ready

**What Happened:**
1. Reviewed oil_record_book_tool codebase - verified 22 tests passing, identified stale README and deprecation warnings
2. Analyzed actual End of Hitch Sounding Form (uploaded HEIC photo):
   - Header: Vessel, Date, Location, Charter, Draft Fwd/Aft, Fuel on Log, Correction
   - 12 fuel tanks (#7, #9, #11, #13, #14, #18 × port/stbd) with soundings + gallons
   - Service oils (#15P/S, #16P/S) - gallons only
   - Slop tanks (#17P Oily Bilge, #17S Dirty Oil) - soundings + gallons
   - Engineer signature
3. Designed expanded data model:
   - New `FuelTankSounding` table for repeating fuel tank rows
   - Expanded `HitchRecord` with all form fields
4. Created comprehensive Cursor prompt (1400+ lines) with:
   - Model changes, OCR service using Google Cloud Vision
   - API endpoints for parse-image, hitch CRUD
   - Full form UI with camera capture and OCR auto-fill
5. Discovered Claude.ai sandbox blocks external API calls - OCR must test locally
6. Moved prompt to `docs/cursor_prompt_full_hitch_form.md` for agent access
7. Cursor executed the prompt - implemented full feature set

**Key Wins:**
- Data model now matches actual paper form exactly
- OCR service ready (pending local testing)
- Comprehensive Cursor prompt documented the implementation spec
- Learnings captured to central repo

**Technical Decisions:**
- Google Cloud Vision over Anthropic for OCR (existing credentials from receipt-analysis project)
- Separate `FuelTankSounding` table vs 30+ columns on HitchRecord
- `parseInt()` vs `parseFloat()` - must match backend column types

**Lessons Learned:**
- Claude.ai sandbox has network restrictions - can't call external APIs (test locally)
- Paper forms with repeating sections → normalize into child tables
- Division of labor: Cursor captures implementation bugs, Claude synthesizes cross-session patterns

**Learnings Captured:**
- BY_TOOL/google-cloud-vision.md - sandbox limits, credentials, table OCR parsing
- BY_TOOL/image-processing.md - HEIC to JPEG on Linux
- UNIVERSAL_PATTERNS.md - #15 Model forms with related tables, #6 type mismatch at boundaries

**Next Steps:**
- Test OCR locally with actual form photo
- Verify `parseFloat` for slop tank gallons (Cursor may have used parseInt)
- Run existing tests to confirm model changes didn't break anything
- Manual test new_hitch page flow

---

### December 12, 2025 - Oil Record Book Tool MVP (Session 3)

**Duration:** ~4 hours
**Project:** oil_record_book_tool, professional-development
**Outcome:** ✅ Working MVP with 22 tests passing

**What Happened:**
1. Created TECHNIQUES.md - extracted actionable tips from Rate Limited podcast & Claude Skills guide
2. Created TRANSITION_PLAN.md - full career transition roadmap with target roles, timeline, skills gaps
3. Set up oil_record_book_tool project from scratch:
   - Flask app structure with SQLAlchemy models
   - Extracted sounding tables from Excel (Tank 17P/17S)
   - CLAUDE.md with project context for agents
   - WORKFLOW_STATE.json for multi-agent tracking
4. Implemented weekly slop tank soundings feature:
   - Feet/inches input → automatic gallon/m³ lookup
   - Generates MARPOL-compliant ORB Code C & I entries
   - Copy-to-clipboard for ORB text
5. Implemented daily fuel tickets feature:
   - Service tank selector (#7, #9, #11, #13, #14, #18 P/S)
   - Meter readings with auto-consumption calculation
   - Stats tracking (today/avg/total)
6. Built dashboard with tank level visualizations
7. Built history view with tabbed soundings/ORB entries

**Key Decisions:**
- Single agent for fuel tickets (feature too cohesive to parallelize)
- Revised connectivity constraint: "Handle temporary drops" vs offline-first (simpler architecture)
- Dark theme with amber accents (engine room friendly)
- Skip Phase 2 framework generation (planning doc already exists)

**Technical Stack:**
- Python/Flask, SQLAlchemy, SQLite
- Mobile-first responsive CSS
- REST API with JSON responses
- 22 tests (8 sounding service, 14 fuel service)

**Portfolio Value:**
This is the "Oil Record Book Assistant" mentioned in TRANSITION_PLAN.md - high-impact domain project proving:
- Real problem from real domain experience
- Production mindset (error handling, validation)
- Constraint awareness (two-crew rotation, mobile-first)

**Lessons Learned:**
- Multi-agent workflow best for greenfield/parallel work, overkill for cohesive features
- Planning docs (like ORB_App_Planning_Document.md) can replace Phase 1-2 formality
- Feature priority should follow user value: daily ops → formal baseline → handover package

**Next Steps:**
- End of Hitch import (formal baseline, enables delta tracking)
- Handover package generation (Excel/PDF for Gold crew)
- Execute TRANSITION_PLAN Phase 1 quick wins

---

### December 12, 2025 - Skills Sync & Project Cleanup (Session 2)

**Duration:** ~30 min
**Project:** professional-development, multi-agent-workflow
**Outcome:** ✅ Claude Code CLI now has all custom skills

**What Happened:**
1. Reviewed project knowledge files - trimmed from 10 to 5 essential:
   - Keep: ETHOS.md, ACTIVE_PROJECTS.md, PROGRESS_JOURNAL.md, TOOLS_INVENTORY.md, README.md
   - Remove from Project: LEARNINGS.md (duplicates multi-agent-workflow/learnings/), GIT_CHEATSHEET.md, RESOURCES.md, MacBook setup files
2. Decided against "read this project" skill - hub-and-spoke pattern already handles it
3. Reviewed Claude Skills guide from trusted developer - good reference, mostly overkill for current needs
4. Installed/updated gh CLI via brew
5. Created professional-development repo on GitHub and pushed
6. Synced all custom skills to `~/.claude/skills/` for Claude Code CLI:
   - capture-learnings, progress-journal
   - phase1-planning through phase6-iteration
   - workflow-state

**Key Insight:**
Meta-skills layer (skill-debugging-assistant, skill-gap-analyzer, etc.) is "factory building factories" trap. With only 2-10 skills, don't need skills to analyze skills.

**Patterns Confirmed:**
- Hub-and-spoke: Opus (planning/capture) + Sonnet agents (implementation) - no need for agents to read full context
- Skills belong in `~/.claude/skills/` for Claude Code CLI auto-discovery
- Public Anthropic skills (docx, pdf, etc.) are baked in, not copyable files

---

### December 12, 2025 - Skills & Knowledge Infrastructure

**Duration:** ~2 hours
**Project:** professional-development, multi-agent-workflow
**Outcome:** ✅ Built knowledge capture infrastructure

**What Happened:**
1. Created `capture-learnings` skill for extracting patterns to central repo
2. Created `progress-journal` skill for updating this document
3. Restructured multi-agent-workflow/learnings/ (deleted 1700 lines of empty templates)
4. Added real learnings from Reality-layer session to BY_TOOL, BY_ERROR, BY_LANGUAGE
5. Set up CLAUDE.md everywhere:
   - `~/.claude/CLAUDE.md` (full version for Claude Code CLI)
   - Repo roots (trimmed version for Claude Code web agents)
6. Created professional-development repo with all project knowledge files
7. Fixed SSH remote on ship-MTA-draft (was HTTPS)

**Key Wins:**
- Central learnings repo now has real content, not templates
- CLAUDE.md with SSH config should prevent future "Username" auth failures
- Skills can now update files on disk (capture-learnings, progress-journal)
- professional-development is now a git repo Claude can read/update

**Patterns Discovered:**
- CLAUDE.md needs different placement for different Claude contexts
- macOS case-insensitivity causes git filename confusion
- Capturing learnings during session + skill at end = belt and suspenders (good redundancy)

**Lessons Learned:**
- Empty templates accumulate but never get filled - real content beats scaffolding
- SSH vs HTTPS preference needs to be in user preferences, not just memory
- Hub-and-spoke workflow: Opus for planning/integration, Sonnet agents for implementation

---

### December 11, 2025 - Reality Layer Multi-Agent Session

**Duration:** ~3 hours
**Project:** Reality-layer AR app
**Outcome:** ✅ First successful device deployment with full AR pipeline

**What Happened:**
1. Ran Phase 3 Codex Review - generated 4 agent prompts
2. Launched 4 parallel agents in Claude Code (local)
3. All 4 completed successfully, 1 merge conflict resolved
4. Fixed post-merge issues: Swift type mismatch, Gemini lazy init, IP config
5. Backend running with mock mode, AR labels rendering on device

**Key Wins:**
- Proved full pipeline: Camera → Backend → Mock Response → 3D AR Labels
- Multi-agent workflow produced +2,707 lines of real improvements
- Loading states, error handling, animations all working
- "7 objects" with 3D labels floating in space - actual AR app

**Bugs Hit:**
- Swift ternary returning mismatched types (Color vs LinearGradient)
- Gemini service initializing at import time (needed lazy wrapper)
- Config.swift had localhost instead of Mac's IP (merge overwrote fix)
- APIError alias missing from exceptions.py

**Lessons Learned:**
- Mock mode must be explicitly enabled via MOCK_MODE=true env var
- Lazy initialization pattern prevents import-time failures for optional deps
- Agent merges need post-integration testing - generated code compiles but may have runtime issues
- 4 parallel agents is productive; coordination overhead is manageable

**Insight:**
> "The trap is optimizing process feels productive and low-risk. Building actual things hits real friction. Friction feels like failure. Factory work feels like progress."

---

## Cumulative Insights

### What Works for DP

| Approach | Why It Works |
|----------|--------------|
| Fresh context per task | Prevents token bloat, focused conversations |
| Phase-based workflow | Clear checkpoints, state files prevent confusion |
| 3-4 parallel agents | Sweet spot - more creates coordination overhead |
| Desktop Commander | Direct file/process control without leaving chat |
| Real device testing | Catches issues simulator misses (ARKit, networking) |
| Mock mode first | Proves pipeline before adding API complexity |
| Hub-and-spoke (Opus + Sonnet agents) | Planning in one place, execution in parallel |
| Belt-and-suspenders redundancy | Capture learnings during session AND via skill |
| Reviewing source documents | Actual paper form > assumed requirements |

### What Doesn't Work

| Approach | Why It Fails |
|----------|--------------|
| Continuous sessions for complex work | Context overflow, lost coherence |
| 5+ agents | Coordination exceeds parallelism benefit |
| Ambitious PRDs | Scope exceeds current execution capacity |
| HTTPS git auth | Flaky on Mac, SSH more reliable |
| Trusting merged code compiles = works | Runtime issues common post-merge |
| Empty templates waiting to be filled | They stay empty; real content beats scaffolding |
| Testing external APIs in Claude.ai sandbox | Network blocked - test locally |

### Recurring Patterns to Watch

1. **IP Address Dance** - Every device test session hits localhost vs LAN IP confusion. Consider: add runtime IP detection or config UI.

2. **Import-Time Initialization** - Services that connect to external APIs at import break when credentials missing. Pattern: lazy init wrapper.

3. **Type Mismatches in Swift** - Ternary operators need matching types on both branches. Common source of build failures.

4. **Stale Branch Accumulation** - Delete branches immediately after merge. Don't let them pile up.

5. **SSH vs HTTPS** - All repos should use SSH. Check `git remote -v` before pushing.

6. **JS/Python Type Boundary** - `parseInt` vs `parseFloat` must match backend column types (Integer vs Float).

---

## Progress Metrics

### Technical Skills

| Skill | Dec 2025 | Notes |
|-------|----------|-------|
| Python/Flask | ⭐⭐⭐ | Production app deployed |
| Multi-agent workflows | ⭐⭐⭐ | 4-agent coordination working |
| Git | ⭐⭐ | Daily ops good, edge cases need cheatsheet |
| iOS/Swift | ⭐⭐ | Can deploy to device, ARKit basics |
| DevOps | ⭐⭐ | Railway deployment, local backend |
| AI APIs | ⭐⭐ | Gemini integration, mock patterns |
| Claude Skills | ⭐⭐ | Built capture-learnings, progress-journal |
| OCR/Vision APIs | ⭐ | Google Cloud Vision setup, needs testing |

### Projects Shipped

| Project | Status | Users |
|---------|--------|-------|
| ship-MTA-draft | ✅ Production | Real crew |
| oil_record_book_tool | 🔄 In Progress | Self (testing this hitch) |
| Reality-layer | ✅ Demo-ready | Self |
| multi-agent-workflow | ✅ v2.0.0 | Self |
| professional-development | ✅ Active | Self |
| AgentOrchestratedCodeFactory | ⚠️ Archive candidate | None |

---

## Recommendations for Future Sessions

### Before Starting
- Check WORKFLOW_STATE.json if picking up a project
- Run `git fetch --prune && git branch -a` to see branch state
- Verify backend is running if project needs it

### During Session
- Commit after each logical chunk, not at the end
- Test on device early and often (don't trust simulator for AR/networking)
- When agents generate code, budget time for integration fixes
- Review actual source documents (forms, specs) before modeling data

### After Session
- Delete merged branches immediately
- Run "capture learnings" skill or update learnings manually
- Push all local commits

### Questions to Ask DP
- "Is this for learning or shipping?" (changes acceptable scope)
- "What's blocking you?" (often reveals real issue isn't the stated task)
- "Should we clean up first?" (he responds well to this)

---

## Quotes Worth Remembering

> "Working > Perfect" - Ship something that works, iterate from there.

> "Vibe coding with intention" - Use AI to generate code, but understand what it generates.

> "The trap is building factories that build factories."

> "If I can't explain it simply, I don't understand it."

> "Mixing both = some redundancy is good" - Belt and suspenders beats missing something.

> "Cursor does implementation bugs, Claude synthesizes cross-session patterns."

---

*Last updated: December 18, 2025*
*Update after each significant session*

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

### Bad Habits

- ❌ Accumulates stale branches (82 across projects before cleanup)
- ❌ Starts new projects before finishing current ones
- ❌ Over-engineers scaffolding (AgentOrchestratedCodeFactory)
- ❌ Ambitious PRDs that exceed current execution capacity
- ❌ localhost vs device IP confusion (recurring issue)

---

## Session Log

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

### What Doesn't Work

| Approach | Why It Fails |
|----------|--------------|
| Continuous sessions for complex work | Context overflow, lost coherence |
| 5+ agents | Coordination exceeds parallelism benefit |
| Ambitious PRDs | Scope exceeds current execution capacity |
| HTTPS git auth | Flaky on Mac, SSH more reliable |
| Trusting merged code compiles = works | Runtime issues common post-merge |
| Empty templates waiting to be filled | They stay empty; real content beats scaffolding |

### Recurring Patterns to Watch

1. **IP Address Dance** - Every device test session hits localhost vs LAN IP confusion. Consider: add runtime IP detection or config UI.

2. **Import-Time Initialization** - Services that connect to external APIs at import break when credentials missing. Pattern: lazy init wrapper.

3. **Type Mismatches in Swift** - Ternary operators need matching types on both branches. Common source of build failures.

4. **Stale Branch Accumulation** - Delete branches immediately after merge. Don't let them pile up.

5. **SSH vs HTTPS** - All repos should use SSH. Check `git remote -v` before pushing.

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

### Projects Shipped

| Project | Status | Users |
|---------|--------|-------|
| ship-MTA-draft | ✅ Production | Real crew |
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

---

*Last updated: December 12, 2025*
*Update after each significant session*

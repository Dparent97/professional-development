# Active Projects

*Current state as of December 2025*

## Production

### ship-MTA-draft ⭐ PRIMARY
**What:** Flask app for shipboard maintenance tracking
**Users:** Crew members on vessel (real production use!)
**Stack:** Python/Flask, SQLite, Cloudinary, Railway
**Repo:** github.com/Dparent97/ship-MTA-draft
**Status:** ✅ Live and working
**Next:** UI improvements, mobile optimization

**Why it matters:** This is proof I can build and deploy something real that people use daily. Reference point for "working software."

---

## Active Development

### AgentOrchestratedCodeFactory
**What:** Multi-agent system that generates complete projects from descriptions
**Stack:** Python, Typer CLI, Pydantic, Jinja2 templates
**Repo:** github.com/Dparent97/AgentOrchestratedCodeFactory
**Status:** ✅ Phase 6 complete, 422 tests passing, 80.89% coverage
**Next:** Actually run it! Generate a project and see what it produces.

**Agents:**
- PlannerAgent - Creates project plan from idea
- ArchitectAgent - Designs system architecture
- ImplementerAgent - Generates code from templates
- TesterAgent - Creates test suite
- DocWriterAgent - Generates documentation
- GitOpsAgent - Handles git operations
- SafetyGuard - Validates safety/security
- BlueCollarAdvisor - Practical suggestions

### multi-agent-workflow
**What:** Coordination system for running parallel AI agents
**Stack:** Python, Claude Skills, Markdown guides
**Repo:** github.com/Dparent97/multi-agent-workflow
**Status:** ✅ v2.0.0 released, working in Claude Code web
**Version:** 2.0.0

**What I learned building this:**
- Symlinks don't work in Claude Code web
- Fresh context per phase beats continuous sessions
- State files (WORKFLOW_STATE.json) prevent "where am I?" confusion
- 3-4 agents is optimal, more creates coordination overhead

---

## On Hold / Experimental

### AR-Facetime-App
**What:** iOS AR app with magical characters for kids
**Stack:** Swift, ARKit, RealityKit
**Status:** 🟡 Has significant code from agents, not integrated
**Branches:** ~25 stale branches with real work
**Next decision:** Prioritize or archive

### Reality-layer
**What:** AR overlay system for real-world object detection
**Stack:** Swift, Gemini Vision API
**Status:** 🟡 Experimental, ~12 stale branches
**Next decision:** Prioritize or archive

### MR_Maintenance_System
**What:** Mixed reality system for industrial maintenance
**Stack:** Python backend, AR frontend concepts
**Status:** 🟡 Ambitious scope, ~12 stale branches
**Next decision:** May be too big for current skill level

### agent-lab
**What:** Framework for creating and testing AI agents
**Stack:** Python
**Status:** 🟡 Overlaps with AgentOrchestratedCodeFactory
**Next decision:** Consolidate or differentiate

---

## Archived / Deleted

### programming-mentorship
**What:** Was supposed to be curriculum/resources repo
**Status:** ❌ Deleted - empty scaffold, replaced by this project

---

## Branch Cleanup Status

| Project | Stale Branches | Status |
|---------|---------------|--------|
| AgentOrchestratedCodeFactory | 0 | ✅ Clean |
| ship-MTA-draft | ~18 | 🔴 Needs triage |
| AR-Facetime-App | ~25 | 🔴 Needs triage |
| Reality-layer | ~12 | 🔴 Needs triage |
| MR_Maintenance_System | ~12 | 🔴 Needs triage |
| agent-lab | ~15 | 🔴 Needs triage |

**Total stale branches across projects:** ~82
**Estimated code in branches:** ~80,000+ lines

---

## Priority Order

1. **ship-MTA-draft** - Production, real users
2. **AgentOrchestratedCodeFactory** - Run it, understand it
3. **multi-agent-workflow** - Maintain, document learnings
4. **AR-Facetime-App** - Decision needed (Aria waiting?)
5. **Others** - Lower priority until above are solid

---

*Update this when project status changes*

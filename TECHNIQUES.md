# Techniques & Tips

*Extracted from podcasts, guides, and experimentation. Updated: December 2025*

---

## Context Management

### The "Smart Zone" Problem
- Models degrade in intelligence as context fills up ("act drunk" per AMP research)
- Even if 200k is available, staying lean keeps model IQ higher
- Verbose MCP tool definitions eat context before you even start (30-50k just in rules)

### Compaction Strategies
- **Manual compact**: After exploration/planning phases, summarize and start fresh
- **Cursor**: Does silent context management (circle fills then drops)
- **Claude Code**: `/compact` to summarize progress, then continue
- **Droid**: Perpetual compaction with anchor points - can run 10M+ token sessions

### Branching Context (Cursor)
- Three dots at bottom of chat → "Duplicate Chat"
- Preserves all context in new branch
- Use cases:
  - Prime context with one model, duplicate and switch to another
  - Bug fix branch while keeping feature work intact
  - Explore multiple approaches in parallel
- **Pro tip**: Rename chats with brackets `[bug fix]` or `[design]` for history navigation

### Claude Code Context Tricks
- `Escape` twice → rewind to previous message, re-explain, continue fresh
- `/resume` in new terminal tab → opens same chat as separate branch
- Both tabs can diverge from same starting point

---

## Model Selection & Characteristics

### Opus 4.5
- **Best for**: Planning, reasoning, meta-prompting, general intelligence
- **Strengths**: Better image understanding, better design sense, handles ambiguity well
- **Cost**: $5/$25 per M tokens - adds up fast (~$250/week heavy use in Cursor)
- **Mental model**: "Big O can handle it" - throw complex problems at it first

### GPT 5.1 Codex Max
- **Best for**: Execution after planning, following structured instructions
- **Warning**: Extremely sticky to instructions - if you say "don't do B", it may refuse B forever in that session
- **Contradiction sensitivity**: Will refuse tasks that conflict with earlier instructions
- **Pro tip**: Use Claude to review prompts for contradictions before feeding to Codex
- **Speed**: Use "fast" variant for better throughput during free period
- **Harness matters**: Same model behaves differently in Cursor vs Droid vs API

### Gemini 3 Pro
- **Best for**: UI mockups, design prototyping
- **Use case**: AI Studio's prototype UI tool → generate mockup → export code as reference
- **Weakness**: Less robust at large context reasoning than 2.5 Pro was
- **Current state**: Overshadowed by Opus 4.5 release, rate limiting issues

### Model Pairing Patterns
- **Plan with Opus → Execute with Codex**: Opus writes detailed plan, Codex implements
- **Opus meta-prompting**: Give Opus a prompting guide URL + your messy requirements → it rewrites as structured prompt
- **Second opinion pattern**: After Opus solution, ask GPT 5.1 for review with fresh eyes on same context

---

## Prompting Techniques

### Meta-Prompting Workflow
1. Find a good prompting guide (e.g., GPT 5.1 Codex cookbook)
2. Give guide URL to Opus 4.5
3. Describe your feature/task in natural "yapping" style
4. Opus rewrites your prompt following the guide's structure
5. Feed structured prompt back to Opus or to target model

### XML Tags for Context
```xml
<my_messy_requirements>
I want the thing to do the stuff when user clicks...
</my_messy_requirements>
```
- Signals to model this is raw input to transform
- Helps separate meta-instructions from actual content

### Iterative Refinement
- "Ask me more questions about what's missing in my plan"
- Let Opus do 3-4 turns of questioning before implementation
- Results in PRD-quality specs from casual descriptions

### Codex-Specific Prompting
- Remove anything prescriptive that might conflict with user requests
- Avoid negatives ("don't do X") - they persist and block later
- Keep system prompts minimal - model was trained on specific harness
- If using custom tools, may fight against built-in tool preferences

---

## Claude Skills

### Core Concepts
- Skills = plugins/extensions for Claude (instructions + tools)
- **Three delivery methods**: Claude Code (folders), Web/Desktop (ZIP upload), API
- **Critical**: Enable "Code Execution" and "File Creation" in settings or skills won't run

### Skill Routing
- Claude picks skills based on description - first paragraph matters most
- Vague descriptions = wrong skill selected or skill ignored
- Include:
  - When to use this
  - When NOT to use this
  - Expected inputs/outputs
  - What it explicitly doesn't handle

### Common Pitfalls
1. **Platform mismatch**: Code wants folders, Web wants ZIPs, API wants endpoints
2. **Code Execution disabled**: Skills install but don't run
3. **Too many skills**: Router gets confused with 20+ similar skills
4. **Version drift**: Different versions across platforms without source of truth
5. **Missing skill_ids**: API returns ID on create - save it or keep recreating

### Skill Design Principles
- **One job per skill**: "ppt_brand_apac" beats "presentation_helper"
- **Treat like code**: Version, test, document, review changes
- **Negative examples**: Explicitly state what the skill doesn't do
- **Focus routing**: Router handles composition better than mega-skills

### Meta-Skills Layer
Skills that help manage skills:
- **skill-debugging-assistant**: Why isn't my skill triggering?
- **skill-security-analyzer**: Is this third-party skill safe?
- **skill-gap-analyzer**: What skills should I build?
- **skill-performance-profiler**: Which skills cost too much tokens?
- **learning-capture**: Auto-detect patterns worth formalizing into skills

### Skill Locations
```
~/.claude/skills/<skill-name>/SKILL.md    # User-level (all projects)
.claude/skills/<skill-name>/SKILL.md      # Project-level (git-shared)
```

---

## Git & GitHub Techniques

### Agent + GH CLI (No MCP Needed)
- Agents know `gh` CLI well - no need for GitHub MCP bloat
- Use cases:
  - Analyze commits to find regression causes
  - Cherry-pick specific changes from PRs without pulling whole branch
  - Compare diff between commits when debugging

### Commit Archaeology
```bash
# Give agent a commit hash
"This commit broke something. Analyze the diff and suggest what regressed."
```
- Works because models trained on tons of GitHub data
- More efficient than loading full MCP tool definitions

---

## Agent Harness Insights

### Harness Impact
- Same model behaves differently across harnesses (Cursor vs Droid vs Claude Code)
- System prompts affect:
  - How aggressively it checks its work
  - Which tools it prefers
  - How much context it reads before acting

### Harness Characteristics
| Harness | Behavior Pattern |
|---------|------------------|
| Cursor | Tends to conservative context reading |
| Droid | More thorough file exploration, less aggressive follow-up |
| Claude Code | Balanced, explicit compaction control |

### Cost vs Quality Tradeoff
- Subsidized/credit plans often less aggressive on verification (saving cost)
- Pay-per-use harnesses tend to be more thorough
- Augment Code: Great quality but waits for permission to test/evaluate

---

## Workflow Patterns

### Planning → Execution Split
1. **Planning phase** (Opus/GPT 5.1 High):
   - Research, architecture, specs
   - Multiple turns of refinement
   - Output: Detailed plan document
   
2. **Execution phase** (Codex/Sonnet):
   - Implement from plan
   - Focused, single-task prompts
   - Output: Working code

### Multi-Model Review
After completing work:
1. Ask different model for "second opinion" on same context
2. Fresh eyes catch patterns the implementing model missed
3. Especially useful: Opus implements → GPT 5.1 High reviews (or vice versa)

### Session Management
- **Fresh context per task**: Prevents token bloat, maintains model quality
- **Compact between phases**: Summarize progress, clear working memory
- **Branch for experiments**: Duplicate before trying risky changes

---

## Anti-Patterns Observed

### Context Stuffing
- Loading 50+ MCP tools before starting = 30-50k tokens burned on definitions
- Model struggles to pick correct tool from too many options
- Better: Minimal tools, add only what's needed

### Ignoring Harness Differences
- "Works in Cursor" ≠ "Works in Droid"
- Same prompt, same model, different system prompt = different results
- Test in target environment

### Trusting Completion = Correctness
- Agents declare "done" without verification
- Post-merge testing still required
- Budget time for integration fixes after agent work

### The Factory Trap
- Building tools that build tools feels productive
- But: 422 tests for template copying (AgentOrchestratedCodeFactory)
- Ask: "Am I building or am I building infrastructure to build?"

---

## Sources

- **Rate Limited Podcast** - Opus 4.5, context windows, model selection (Dec 2025)
- **Claude Skills Complete Guide** - Skill architecture, meta-skills, best practices
- **AMP Research** - Context window degradation, "smart zone"
- **Cursor/OpenAI Blog** - Codex Max prompting techniques
- **Human Layer** - Context-efficient back pressure

---

*Add new techniques as discovered. Review monthly for relevance.*

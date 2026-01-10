# Workflow: Create Exhaustive Domain Expertise Skill

<objective>
Build a comprehensive execution skill that does real work in a specific domain. Domain expertise skills are full-featured build skills with exhaustive domain knowledge in references, complete workflows for the full lifecycle (build → debug → optimize → ship), and can be both invoked directly by users AND loaded by other skills (like create-plans) for domain knowledge.
</objective>

<critical_distinction>
**Regular skill:** "Do one specific task"
**Domain expertise skill:** "Do EVERYTHING in this domain, with complete practitioner knowledge"

Examples:
- `expertise/macos-apps` - Build macOS apps from scratch through shipping
- `expertise/python-games` - Build complete Python games with full game dev lifecycle
- `expertise/rust-systems` - Build Rust systems programs with exhaustive systems knowledge
- `expertise/web-scraping` - Build scrapers, handle all edge cases, deploy at scale

Domain expertise skills:
- ✅ Execute tasks (build, debug, optimize, ship)
- ✅ Have comprehensive domain knowledge in references
- ✅ Are invoked directly by users ("build a macOS app")
- ✅ Can be loaded by other skills (create-plans reads references for planning)
- ✅ Cover the FULL lifecycle, not just getting started
</critical_distinction>

<required_reading>
**Read these reference files NOW:**
1. references/recommended-structure.md
2. references/core-principles.md
3. references/use-xml-tags.md
</required_reading>

<process>
## Step 1: Identify Domain

Ask user what domain expertise to build:

**Example domains:**
- macOS/iOS app development
- Python game development
- Rust systems programming
- Machine learning / AI
- Web scraping and automation
- Data engineering pipelines
- Audio processing / DSP
- 3D graphics / shaders
- Unity/Unreal game development
- Embedded systems

Get specific: "Python games" or "Python games with Pygame specifically"?

## Step 2: Confirm Target Location

Explain:
```
Domain expertise skills go in: ~/.claude/skills/expertise/{domain-name}/

These are comprehensive BUILD skills that:
- Execute tasks (build, debug, optimize, ship)
- Contain exhaustive domain knowledge
- Can be invoked directly by users
- Can be loaded by other skills for domain knowledge

Name suggestion: {suggested-name}
Location: ~/.claude/skills/expertise/{suggested-name}/
```

Confirm or adjust name.

## Step 3: Identify Workflows

Domain expertise skills cover the FULL lifecycle. Identify what workflows are needed.

**Common workflows for most domains:**
1. **build-new-{thing}.md** - Create from scratch
2. **add-feature.md** - Extend existing {thing}
3. **debug-{thing}.md** - Find and fix bugs
4. **write-tests.md** - Test for correctness
5. **optimize-performance.md** - Profile and speed up
6. **ship-{thing}.md** - Deploy/distribute

**Domain-specific workflows:**
- Games: `implement-game-mechanic.md`, `add-audio.md`, `polish-ui.md`
- Web apps: `setup-auth.md`, `add-api-endpoint.md`, `setup-database.md`
- Systems: `optimize-memory.md`, `profile-cpu.md`, `cross-compile.md`

Each workflow = one complete task type that users actually do.

## Step 4: Exhaustive Research Phase

**CRITICAL:** This research must be comprehensive, not superficial.

### Research Strategy

Run multiple web searches to ensure coverage:

**Search 1: Current ecosystem**
- "best {domain} libraries 2024 2025"
- "popular {domain} frameworks comparison"
- "{domain} tech stack recommendations"

**Search 2: Architecture patterns**
- "{domain} architecture patterns"
- "{domain} best practices design patterns"
- "how to structure {domain} projects"

**Search 3: Lifecycle and tooling**
- "{domain} development workflow"
- "{domain} testing debugging best practices"
- "{domain} deployment distribution"

**Search 4: Common pitfalls**
- "{domain} common mistakes avoid"
- "{domain} anti-patterns"
- "what not to do {domain}"

**Search 5: Real-world usage**
- "{domain} production examples GitHub"
- "{domain} case studies"
- "successful {domain} projects"

### Verification Requirements

For EACH major library/tool/pattern found:
- **Check recency:** When was it last updated?
- **Check adoption:** Is it actively maintained? Community size?
- **Check alternatives:** What else exists? When to use each?
- **Check deprecation:** Is anything being replaced?

**Red flags for outdated content:**
- Articles from before 2023 (unless fundamental concepts)
- Abandoned libraries (no commits in 12+ months)
- Deprecated APIs or patterns
- "This used to be popular but..."

### Documentation Sources

Use Context7 MCP when available:
```
mcp__context7__resolve-library-id: {library-name}
mcp__context7__get-library-docs: {library-id}
```

Focus on official docs, not tutorials.

## Step 5: Organize Knowledge Into Domain Areas

Structure references by domain concerns, NOT by arbitrary categories.

**For game development example:**
```
references/
├── architecture.md         # ECS, component-based, state machines
├── libraries.md           # Pygame, Arcade, Panda3D (when to use each)
├── graphics-rendering.md  # 2D/3D rendering, sprites, shaders
├── physics.md             # Collision, physics engines
├── audio.md               # Sound effects, music, spatial audio
├── input.md               # Keyboard, mouse, gamepad, touch
├── ui-menus.md            # HUD, menus, dialogs
├── game-loop.md           # Update/render loop, fixed timestep
├── state-management.md    # Game states, scene management
├── networking.md          # Multiplayer, client-server, P2P
├── asset-pipeline.md      # Loading, caching, optimization
├── testing-debugging.md   # Unit tests, profiling, debugging tools
├── performance.md         # Optimization, profiling, benchmarking
├── packaging.md           # Building executables, installers
├── distribution.md        # Steam, itch.io, app stores
└── anti-patterns.md       # Common mistakes, what NOT to do
```

**For each reference file:**
- Pure XML structure
- Decision trees: "If X, use Y. If Z, use A instead."
- Comparison tables: Library vs Library (speed, features, learning curve)
- Code examples showing patterns
- "When to use" guidance
- Platform-specific considerations
- Current versions and compatibility

## Step 6: Create SKILL.md

Domain expertise skills use router pattern with essential principles. See references/recommended-structure.md for the full template.

## Step 7: Write Workflows

For EACH workflow identified in Step 3, create a workflow file with:
- `<required_reading>` - Which references to load
- `<process>` - Step-by-step implementation steps
- `<anti_patterns>` - Common mistakes to avoid
- `<success_criteria>` - How to know it worked

## Step 8: Write Comprehensive References

For EACH reference file identified in Step 5, use pure XML structure with:
- `<overview>` - Brief introduction
- `<options>` - Available approaches/libraries with pros/cons
- `<decision_tree>` - How to choose the right approach
- `<patterns>` - Common patterns with code examples
- `<anti_patterns>` - What NOT to do
- `<platform_considerations>` - Platform-specific notes

## Step 9: Validate Completeness

Ask: "Could a user build a professional {domain thing} from scratch through shipping using just this skill?"

**Must answer YES to:**
- [ ] All major libraries/frameworks covered?
- [ ] All architectural approaches documented?
- [ ] Complete lifecycle addressed (build → debug → test → optimize → ship)?
- [ ] Platform-specific considerations included?
- [ ] "When to use X vs Y" guidance provided?
- [ ] Common pitfalls documented?
- [ ] Current as of 2024-2025?
- [ ] Workflows actually execute tasks (not just reference knowledge)?
- [ ] Each workflow specifies which references to read?

## Step 10: Create Directory and Files

```bash
# Create structure
mkdir -p ~/.claude/skills/expertise/{domain-name}
mkdir -p ~/.claude/skills/expertise/{domain-name}/workflows
mkdir -p ~/.claude/skills/expertise/{domain-name}/references

# Write SKILL.md
# Write all workflow files
# Write all reference files

# Verify structure
ls -R ~/.claude/skills/expertise/{domain-name}
```

## Step 11: Final Quality Check

Review entire skill against quality criteria in references/skill-structure.md.
</process>

<success_criteria>
Domain expertise skill is complete when:

- [ ] Comprehensive research completed (5+ web searches)
- [ ] All sources verified for currency (2024-2025)
- [ ] Knowledge organized by domain areas (not arbitrary)
- [ ] Essential principles in SKILL.md (always loaded)
- [ ] Intake routes to appropriate workflows
- [ ] Each workflow has required_reading + implementation steps + verification
- [ ] Each reference has decision trees and comparisons
- [ ] Anti-patterns documented throughout
- [ ] Full lifecycle covered (build → debug → test → optimize → ship)
- [ ] Platform-specific considerations included
- [ ] Located in ~/.claude/skills/expertise/{domain-name}/
- [ ] Passes dual-purpose test: Can be invoked directly AND loaded for knowledge
- [ ] User can build something professional from scratch through shipping
</success_criteria>

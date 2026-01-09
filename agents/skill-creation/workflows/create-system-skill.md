# Workflow: Create a System Skill

<objective>
Build a stateful, persistent skill that Claude can animate over time. System skills combine a CLI binary, SKILL.md, and SQLite database to create systems where value compounds with each interaction.
</objective>

<required_reading>
**Read these reference files NOW:**
1. references/system-skill-pattern.md
2. references/recommended-structure.md
3. references/use-xml-tags.md

**Template available:** templates/system-skill.md - Copy and fill for SKILL.md structure
</required_reading>

<process>
## Step 1: Identify the Core Loop

Ask the user what repeatable interaction should accumulate value:

**Key question:** "What data should this skill track, and how does that data become more valuable over time?"

**Example core loops:**
- Pomodoro: Work sessions → productivity patterns
- Journal: Daily entries → mood trends, topic themes
- Finance: Transactions → spending insights
- Tasks: Completed work → velocity and priority patterns
- Notes: Ideas → connected knowledge graph
- Habits: Daily actions → streak patterns and correlations

**What makes a good system skill candidate:**
- ✅ Repeatable interactions that accumulate data
- ✅ Data becomes more valuable with volume
- ✅ Pattern recognition benefits the user
- ✅ Cross-session continuity matters
- ❌ One-time operations (use standard skill)
- ❌ Stateless transformations (use standard skill)

## Step 2: Design the CLI Commands

Map the core operations to commands:

**Required commands (every system skill needs these):**

| Command | Purpose | Example |
|---------|---------|---------|
| `start` / `add` / `new` | Create new entry | `./tool start --task "Deep work"` |
| `status` | Check current state | `./tool status` |
| `stop` / `complete` | End current entry | `./tool stop` |
| `history` | View past entries | `./tool history --days 30` |
| `stats` | Analyze patterns | `./tool stats --period week` |

**Optional commands (based on domain):**

| Command | When to include |
|---------|-----------------|
| `edit <id>` | Users need to modify entries |
| `delete <id>` | Users need to remove entries |
| `search <query>` | Text search matters |
| `export` | Backup/portability needed |
| `tag <id> <tags>` | Categorization matters |

**Standard flags (all commands should support):**
```bash
--json      # Programmatic JSON output
--help      # Comprehensive help text
```

**Time-based flags (for history/stats):**
```bash
--period <day|week|month|year>
--days <n>
--from <date> --to <date>
```

## Step 3: Design the Database Schema

Keep it simple. Most system skills need only 1-2 tables:

**Standard single-table schema:**
```sql
CREATE TABLE entries (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  type TEXT,                    -- Optional: entry categorization
  content TEXT NOT NULL,        -- Primary data
  metadata TEXT,                -- JSON for flexible extras
  started_at TEXT NOT NULL,     -- When entry began
  completed_at TEXT             -- NULL if incomplete
);

CREATE INDEX idx_entries_started ON entries(started_at);
```

**Verify the schema answers these questions:**
- How will `history` query recent entries?
- How will `stats` compute aggregations?
- What patterns will emerge from this data?
- Is there anything that needs categorization?

## Step 4: Choose Implementation Technology

**Recommended: Deno + TypeScript**
- Compiles to single binary
- Security permissions baked in at compile time
- No network access by default
- Cross-platform

```typescript
// Compile with:
// deno compile --allow-read --allow-write --output ./tool ./src/main.ts
```

**Alternative: Python + PyInstaller**
- Familiar to most developers
- Larger binary size
- Requires more security consideration

**Alternative: Go**
- Single binary
- Fast compilation
- Cross-platform

**Alternative: Rust**
- Single binary
- Best performance
- Steeper learning curve

## Step 5: Implement the CLI

Structure the implementation:

```
src/
├── main.ts         # CLI entry point and command definitions
├── db.ts           # Database operations (init, CRUD, queries)
├── commands/       # Optional: separate files per command
│   ├── start.ts
│   ├── status.ts
│   ├── history.ts
│   └── stats.ts
└── types.ts        # TypeScript interfaces
```

**CLI implementation checklist:**
- [ ] All commands work standalone
- [ ] --help is comprehensive for every command
- [ ] --json produces valid, parseable JSON
- [ ] Database auto-creates on first run
- [ ] Binary and database colocated
- [ ] Error messages are clear and actionable
- [ ] No network access required
- [ ] No configuration files required

## Step 6: Write SKILL.md

System skills use a specific structure that emphasizes the OODA loop:

```yaml
---
name: {skill-name}
description: {What it does}. Use when {triggers}. Demonstrates System Skill Pattern (CLI + SKILL.md + Database).
allowed-tools:
  - Bash
  - Read
---
```

**Required sections for system skill SKILL.md:**

```xml
<overview>
Brief description. Note this is a System Skill - it persists state and provides compounding value over time.
</overview>

<mental_model>
## The OODA Loop

Operating this skill involves a continuous cycle:

1. **Observe** → Check current status and review history
2. **Orient** → Analyze patterns in the data
3. **Decide** → Determine optimal actions based on patterns
4. **Act** → Execute commands, provide recommendations

Each cycle builds on accumulated data.
</mental_model>

<dependencies>
- Binary location: `~/.claude/skills/{name}/{name}`
- Database: Auto-created at `~/.claude/skills/{name}/{name}.db`
- No external dependencies
</dependencies>

<quick_decision_tree>
User request → Route to:
├─ Start something → Check status first, then start
├─ Check state → Use status command
├─ Analyze patterns → Use stats command
├─ View history → Use history command
└─ Stop/end → Use stop command
</quick_decision_tree>

<core_commands>
Document each command with:
- What it does
- Required and optional flags
- Example usage
- Example output (both human and JSON)
</core_commands>

<pattern_recognition>
## As Data Accumulates

**After a few entries (1-5):**
- Basic observations only

**After several days (5-20 entries):**
- Simple trends visible

**After a week (20-50 entries):**
- Behavioral patterns emerge
- Time-based patterns visible

**After a month (50+ entries):**
- Deep insights available
- Personalized recommendations

**After several months (100+ entries):**
- Long-term trends
- Predictive insights possible
</pattern_recognition>

<milestones>
Celebrate progress:
- 10 entries: "Building the habit!"
- 50 entries: "Real patterns emerging"
- 100 entries: "Deep insights available"
</milestones>

<common_pitfalls>
Document domain-specific mistakes to avoid.
</common_pitfalls>

<success_criteria>
How to know the system is working well.
</success_criteria>
```

## Step 7: Create Directory Structure

```bash
mkdir -p ~/.claude/skills/{skill-name}
```

The structure should be:
```
~/.claude/skills/{skill-name}/
├── SKILL.md           # Claude's instructions
├── {skill-name}       # Compiled CLI binary
└── {skill-name}.db    # SQLite database (auto-created on first use)
```

If including source code for modification:
```
~/.claude/skills/{skill-name}/
├── SKILL.md
├── {skill-name}           # Compiled binary
├── {skill-name}.db        # Database (auto-created)
└── src/                   # Source code
    ├── main.ts
    ├── db.ts
    └── deno.json
```

## Step 8: Compile and Install

**For Deno:**
```bash
cd ~/.claude/skills/{skill-name}/src
deno compile --allow-read --allow-write --output ../{skill-name} main.ts
```

**Verify installation:**
```bash
cd ~/.claude/skills/{skill-name}
./{skill-name} --help        # Help works
./{skill-name} --version     # Version shows
```

## Step 9: Test the System

**CLI isolation tests:**
```bash
./{skill-name} --help
./{skill-name} start --task "Test entry"
./{skill-name} status
./{skill-name} stop
./{skill-name} history
./{skill-name} stats
```

**Persistence tests:**
1. Create an entry
2. Exit terminal
3. Re-open terminal
4. Check history shows previous entry
5. Check stats compute correctly

**Edge case tests:**
- Start when already started
- Stop when nothing running
- Stats with no data
- History with no entries

## Step 10: Test with Claude

**Basic flow test:**
1. "Start a [thing] for testing"
2. Verify Claude checks status first
3. Verify Claude runs correct start command
4. "What's my current status?"
5. Verify Claude interprets output correctly

**OODA loop test:**
1. Add several test entries
2. Ask for analysis
3. Verify Claude observes (checks data)
4. Verify Claude orients (analyzes patterns)
5. Verify Claude decides (makes recommendations)

## Step 11: Create Slash Command (Optional)

```bash
cat > ~/.claude/commands/{skill-name}.md << 'EOF'
---
description: {Brief description of system skill}
allowed-tools: Skill({skill-name})
---

Invoke the {skill-name} skill for: $ARGUMENTS
EOF
```

## Step 12: Final Validation

**Checklist:**
- [ ] CLI binary works standalone
- [ ] All commands have --help
- [ ] All commands support --json
- [ ] Database auto-creates
- [ ] SKILL.md documents OODA loop
- [ ] SKILL.md has quick decision tree
- [ ] SKILL.md has pattern recognition guidance
- [ ] Binary and database colocated
- [ ] Claude can operate the system
- [ ] Value compounds with usage
</process>

<anti_patterns>
## What NOT to Do

**CLI Anti-patterns:**
- ❌ Requiring configuration files
- ❌ Requiring network access for basic operations
- ❌ Storing database in a different location than binary
- ❌ Missing --help documentation
- ❌ Cryptic error messages
- ❌ Inconsistent flag naming

**SKILL.md Anti-patterns:**
- ❌ Treating it like a standard skill (forgetting OODA loop)
- ❌ No pattern recognition guidance
- ❌ No milestone celebrations
- ❌ No common pitfalls section
- ❌ Missing quick decision tree

**Database Anti-patterns:**
- ❌ Complex multi-table schemas (keep it simple)
- ❌ Missing indexes on queried columns
- ❌ Storing binary data that could be JSON
- ❌ No timestamp columns
</anti_patterns>

<success_criteria>
System skill is complete when:

**CLI:**
- [ ] All core commands implemented (start/add, stop/complete, status, history, stats)
- [ ] --help comprehensive for every command
- [ ] --json works for all commands
- [ ] Binary is self-contained (no dependencies)
- [ ] Database auto-creates on first run
- [ ] Clear, actionable error messages

**SKILL.md:**
- [ ] Explains this is a System Skill
- [ ] Documents the OODA loop for this domain
- [ ] Has quick decision tree
- [ ] Has pattern recognition guidance (what to look for as data grows)
- [ ] Has milestone celebrations
- [ ] Has common pitfalls
- [ ] Documents all commands with examples

**Integration:**
- [ ] Binary and database colocated in skill directory
- [ ] Claude can successfully operate the system
- [ ] Tested with real usage
- [ ] Value demonstrably compounds with usage

**The Ultimate Test:**
"After using this for a month, is it significantly more valuable than on day one?"
- If yes: System skill is working
- If no: Something is missing in the pattern recognition or data accumulation
</success_criteria>

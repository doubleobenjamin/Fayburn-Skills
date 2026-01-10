<overview>
The System Skill Pattern is an advanced skill architecture that creates persistent, stateful systems Claude can animate over time. Unlike stateless skills that execute and forget, system skills accumulate context, learn patterns, and provide compounding value with each interaction.

**The Pattern:** CLI + SKILL.md + Database

Give Claude a command-line tool to run, instructions on how to operate a system, and a database to remember things. Claude runs its own OODA loop (Observe, Orient, Decide, Act), building context that compounds across sessions.
</overview>

<what_makes_it_different>
**Standard Skills:**
- Stateless: Run and forget
- Single interaction focus
- Value stays constant over time
- Claude responds to requests

**System Skills:**
- Stateful: Persist data across sessions
- Build context over time
- Value compounds with use
- Claude animates a system

The phase change: Chat agents and skills join to become something new - systems that Claude animates rather than just responds to.
</what_makes_it_different>

<the_three_components>
## Component 1: The CLI Binary

A standalone executable that provides handles for Claude to operate the system:

```bash
./pomodoro start --task "Deep work on authentication"
./pomodoro stats --period week
./pomodoro history --days 30
```

**Critical Attributes:**
- **Self-contained**: No runtime dependencies, no configuration
- **Zero setup**: Just run the binary, it works
- **Helpful `--help`**: Documentation for each command guides Claude
- **JSON output**: `--json` flag for programmatic access
- **Colocated with database**: Lives next to its data

**CLI Design Principles:**
- Distill the core loop into commands
- Each command = one atomic operation
- Output should be clear and parseable
- Verbose when human-readable, JSON when programmatic
- Security permissions baked in (use Deno compile or similar)

**Example CLI Commands:**
```bash
# Core operations
./tool start --param "value"    # Begin something
./tool stop                      # End something
./tool status                    # Check current state

# History and analytics
./tool history --days 30         # View past records
./tool stats --period week       # Analyze patterns

# All commands support --json
./tool stats --period week --json
```

## Component 2: SKILL.md

The tutorial that teaches Claude how to think about and operate the system:

**What SKILL.md Contains:**
- What the system is and when to use it
- Which commands to run in different situations
- How to interpret output and look for patterns
- The mental model and decision flow
- The OODA loop for this specific system

**Mental Model: The OODA Loop**

```
1. Observe → Check status, review history
2. Orient  → Analyze patterns in data
3. Decide  → Determine optimal actions
4. Act     → Run commands, provide insights
```

Each cycle builds on accumulated data.

**Quick Decision Tree Pattern:**
```
User task → What kind of request?
├─ Start something → Check status first, then start
├─ Check current state → Use status command
├─ Review patterns → Use stats command
├─ View history → Use history command
└─ Stop early → Use stop command
```

**Pattern Recognition Guidance:**
SKILL.md should teach Claude to recognize patterns as data accumulates:
- After a few sessions: Basic observations
- After several days: Simple trends
- After a week: Pattern identification
- After a month: Deep insights and recommendations
- After several months: Behavioral understanding

## Component 3: SQLite Database

Persistent storage where every interaction adds context:

```sql
CREATE TABLE sessions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  task TEXT NOT NULL,
  duration INTEGER NOT NULL,
  started_at TEXT NOT NULL,
  completed_at TEXT
);
```

**Why SQLite:**
- Self-contained: Just a file, no server
- Zero configuration: No setup required
- Colocated: Lives next to the CLI binary
- Easy backup: Copy the file
- Direct access: Claude can query if needed
- Grows with use: Each interaction adds value
</the_three_components>

<emergent_properties>
## Emergent Properties of System Skills

When these three components work together, emergent properties arise that exceed what any component provides alone:

### 1. Memory That Compounds
Every interaction saved, building context over time. Week 1 data informs Week 4 insights.

### 2. Pattern Recognition
Trends emerge from accumulated data that couldn't be seen from individual interactions.

### 3. Personalized Intelligence
The system learns about THIS user's patterns, preferences, and behaviors.

### 4. Proactive Insights
With enough data, Claude can offer unprompted observations and recommendations.

### 5. Behavioral Understanding
"You always finish morning sessions but abandon afternoon ones" - insights that require history.

### 6. Autonomous Operation
Claude turns the crank - running its own OODA loop without constant direction.

### 7. Compounding Value
The more you use it, the more valuable it becomes. Unlike tools that stay static.

### 8. Cross-Session Continuity
Context persists across conversations, allowing long-term projects and tracking.
</emergent_properties>

<when_to_use>
## When to Create a System Skill

**Perfect Candidates:**
- Personal data systems that accumulate value (journals, trackers, logs)
- Workflows where history informs decisions
- Systems that benefit from pattern recognition
- Tools that should learn from user behavior
- Long-term projects requiring cross-session continuity

**Example System Skills:**

**Personal Finance Manager:**
- CLI: `money tx list`, `money note abc-123 "unexpected"`, `money cat abc-123 "vacation"`
- Database: Table of transactions
- Result: Claude watches spending trends, asks about anomalies

**Personal Project Manager:**
- CLI: `task new "do thing"`, `task update T-123 --status=done`, `task kanban`
- Database: Tasks with status, due date, priority
- Result: "What are my top 3 priorities for the week?"

**Gratitude Journal:**
- CLI: `thankful "for my cat"`, `thankful trends`, `thankful search`
- Database: Entries with message and timestamp
- Result: A journal that talks back and lifts you up

**Morning Briefing Generator:**
- CLI: `pulse topic add "AI safety"`, `pulse generate`, `pulse feedback <id> --helpful`
- Database: Topics, generated briefings, feedback
- Result: Briefing that learns what's useful to you

**Note System with Memory:**
- CLI: `note add "API design thoughts"`, `note search "auth"`, `note tag T-123 "urgent"`
- Database: Notes with tags and full-text search
- Result: Claude recalls and connects ideas across sessions

**When NOT to use System Skill Pattern:**
- One-time operations that don't benefit from history
- Stateless transformations (format conversion, linting)
- Quick lookups that don't accumulate value
- Operations where history is irrelevant
</when_to_use>

<implementation_guide>
## Building a System Skill

### Step 1: Identify the Core Loop

What repeatable interaction accumulates value?
- Pomodoro: Work sessions → productivity patterns
- Journal: Daily entries → mood trends
- Finance: Transactions → spending insights

### Step 2: Design the CLI

Commands should cover:
1. **Create/Start**: Add new data to the system
2. **Read/Status**: Check current state
3. **Update/Stop**: Modify existing data
4. **History**: View past data
5. **Stats/Analyze**: Derive insights from data

**CLI Best Practices:**
```bash
# Consistent flag patterns
--json           # Programmatic output
--period <time>  # Time-based filtering
--days <n>       # History window
--format <type>  # Output format

# Helpful --help for every command
./tool --help
./tool start --help
./tool stats --help
```

### Step 3: Design the Database Schema

Keep it simple. One or two tables usually suffices:

```sql
-- Primary data table
CREATE TABLE entries (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  content TEXT NOT NULL,
  created_at TEXT NOT NULL,
  metadata TEXT  -- JSON for flexible extras
);

-- Index for common queries
CREATE INDEX idx_entries_created ON entries(created_at);
```

### Step 4: Write SKILL.md

Structure for system skills:

```yaml
---
name: system-name
description: What it does. Use when [triggers]. Demonstrates System Skill Pattern.
allowed-tools:
  - Bash
  - Read
---
```

```xml
<overview>
Brief description. Note that this is a System Skill.
</overview>

<mental_model>
## The OODA Loop

1. **Observe** → Commands to check state
2. **Orient** → Commands to analyze patterns
3. **Decide** → Decision logic based on data
4. **Act** → Commands to take action

Each cycle builds on accumulated data.
</mental_model>

<quick_decision_tree>
User request → Route to:
├─ Action request → Check status, then act
├─ State query → Status command
├─ Analysis request → Stats command
├─ History request → History command
└─ Control request → Stop/modify command
</quick_decision_tree>

<core_commands>
Document each command with examples and expected output.
</core_commands>

<pattern_recognition>
## As Data Accumulates

Guide Claude on what patterns to look for at different data volumes.
</pattern_recognition>

<success_criteria>
How to know the system is working well.
</success_criteria>
```

### Step 5: Build the CLI

**Recommended Stack:**
- **Deno + TypeScript**: Compiles to single binary, security permissions baked in
- **Python + PyInstaller**: Common, easy to build
- **Go**: Single binary, fast, cross-platform
- **Rust**: Single binary, very fast

**Deno Example (recommended):**
```typescript
// Build with: deno compile --allow-read --allow-write pomodoro.ts
import { Command } from "@cliffy/command";
import { DatabaseSync } from "node:sqlite";

// Self-contained, security permissions baked in at compile time
```

### Step 6: Location and Installation

```
~/.claude/skills/{skill-name}/
├── SKILL.md           # Claude's instructions
├── {skill-name}       # Compiled CLI binary
└── {skill-name}.db    # SQLite database (auto-created)
```

Binary and database colocated. No configuration needed.
</implementation_guide>

<skill_md_patterns>
## SKILL.md Patterns for System Skills

### OODA Loop Documentation

```xml
<mental_model>
Operating this skill involves running a continuous cycle:

1. **Observe** → Check current status (`./tool status`) and review history (`./tool history`)
2. **Orient** → Analyze patterns in the data (`./tool stats --period week`)
3. **Decide** → Determine optimal actions based on patterns
4. **Act** → Execute commands, provide recommendations, celebrate milestones

Each cycle builds on accumulated data, making insights more valuable over time.
</mental_model>
```

### Progressive Pattern Recognition

```xml
<pattern_recognition>
As data accumulates, provide increasingly valuable insights:

**After a few entries (1-5):**
- Basic observations
- Simple summaries

**After several days (5-20 entries):**
- Simple trends
- Time-based patterns

**After a week (20-50 entries):**
- Behavioral patterns
- Recommendations based on data

**After a month (50+ entries):**
- Deep insights
- Comparative analysis
- Personalized recommendations

**After several months (100+ entries):**
- Long-term trends
- Behavioral understanding
- Predictive insights
</pattern_recognition>
```

### Milestone Celebrations

```xml
<milestones>
Acknowledge progress at key milestones:

- **10 entries**: "First 10 complete - building the habit!"
- **50 entries**: "50 entries - real patterns emerging"
- **100 entries**: "100 entries - deep insights available"
- **High completion rates**: "95% this week - excellent!"

To check milestone status: `./tool stats --period year --json`
</milestones>
```

### Common Pitfalls Documentation

```xml
<common_pitfalls>
### Forgetting to Check Status First

❌ Don't start new entry without checking status
✅ Do run `./tool status` before starting

### Vague Entry Names

❌ Don't use "Work", "Stuff"
✅ Do use "Refactor auth module", "Review PR #123"

Why: Descriptive names enable better analytics.

### Ignoring Pattern Signals

❌ Don't ignore consistent low completion for certain types
✅ Do adjust approach based on type patterns
</common_pitfalls>
```
</skill_md_patterns>

<cli_design_patterns>
## CLI Design for System Skills

### Standard Command Set

```bash
# Lifecycle commands
./tool start [options]    # Begin new entry
./tool stop               # End current entry
./tool status             # Check current state

# History and analytics
./tool history [options]  # View past entries
./tool stats [options]    # Analyze patterns

# Management (optional)
./tool edit <id>          # Modify entry
./tool delete <id>        # Remove entry
./tool export             # Backup data
```

### Standard Options

```bash
# Global options
-j, --json      # JSON output for programmatic use
-h, --help      # Show help

# Time-based filtering
--period <day|week|month|year>
--days <n>
--from <date>
--to <date>

# Entry-specific
--task <description>
--tags <tag1,tag2>
```

### Output Modes

**Human-readable (default):**
```
✓ Session started
Task: Deep work on auth
Duration: 25 minutes
```

**JSON (--json flag):**
```json
{
  "status": "started",
  "task": "Deep work on auth",
  "duration": 25,
  "started_at": "2025-01-09T14:30:00Z"
}
```

### Help Documentation

Every command needs comprehensive --help:

```
Usage: pomodoro start [options]

Start a new Pomodoro session

Options:
  -t, --task <task>     Task description (required)
  -w, --work <minutes>  Work duration (default: 25)
  -b, --break <minutes> Break duration (default: 5)
  -c, --cycles <count>  Number of cycles (default: 1)
  -j, --json            Output in JSON format
  -h, --help            Show this help

Examples:
  pomodoro start --task "Write docs"
  pomodoro start --task "Sprint" --work 15 --cycles 3
```
</cli_design_patterns>

<database_patterns>
## Database Design for System Skills

### Simple Schema (Most Cases)

One table with flexible metadata:

```sql
CREATE TABLE entries (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  type TEXT NOT NULL,           -- Entry type for filtering
  content TEXT NOT NULL,        -- Primary content
  created_at TEXT NOT NULL,     -- ISO timestamp
  completed_at TEXT,            -- NULL if incomplete
  metadata TEXT                 -- JSON for extras
);

CREATE INDEX idx_entries_created ON entries(created_at);
CREATE INDEX idx_entries_type ON entries(type);
```

### Time-Series Schema

For tracking over time:

```sql
CREATE TABLE measurements (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  metric TEXT NOT NULL,
  value REAL NOT NULL,
  unit TEXT,
  recorded_at TEXT NOT NULL
);

CREATE INDEX idx_measurements_metric ON measurements(metric);
CREATE INDEX idx_measurements_recorded ON measurements(recorded_at);
```

### Relational Schema (When Needed)

```sql
CREATE TABLE projects (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  created_at TEXT NOT NULL
);

CREATE TABLE tasks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  project_id INTEGER REFERENCES projects(id),
  description TEXT NOT NULL,
  status TEXT DEFAULT 'pending',
  created_at TEXT NOT NULL
);
```

### Query Patterns

**Recent entries:**
```sql
SELECT * FROM entries
WHERE created_at >= datetime('now', '-7 days')
ORDER BY created_at DESC;
```

**Aggregations:**
```sql
SELECT
  strftime('%H', created_at) as hour,
  COUNT(*) as count
FROM entries
WHERE completed_at IS NOT NULL
GROUP BY hour
ORDER BY count DESC;
```

**Statistics:**
```sql
SELECT
  COUNT(*) as total,
  SUM(CASE WHEN completed_at IS NOT NULL THEN 1 ELSE 0 END) as completed,
  ROUND(AVG(CASE WHEN completed_at IS NOT NULL THEN 1.0 ELSE 0.0 END) * 100, 1) as completion_rate
FROM entries
WHERE created_at >= datetime('now', '-30 days');
```
</database_patterns>

<testing_system_skills>
## Testing System Skills

### Functional Testing

1. **CLI works in isolation:**
   ```bash
   ./tool --help              # Help works
   ./tool start --task "Test" # Can start
   ./tool status              # Can check status
   ./tool stop                # Can stop
   ./tool history             # Can view history
   ./tool stats               # Can view stats
   ```

2. **Database persistence:**
   - Start session, exit, check database file exists
   - Restart, check history shows previous session
   - Check stats compute correctly

3. **Edge cases:**
   - Start when already started
   - Stop when nothing running
   - Empty history/stats

### Integration Testing with Claude

1. **Basic flow:**
   - "Start a session for testing"
   - Check Claude runs correct command
   - "What's my status?"
   - Check Claude interprets output

2. **Pattern recognition:**
   - Add several test entries
   - Ask for analysis
   - Check insights quality

3. **OODA loop:**
   - Does Claude observe before acting?
   - Does Claude orient based on history?
   - Does Claude decide based on patterns?
</testing_system_skills>

<reference_implementation>
## Reference Implementation: Pomodoro

The Pomodoro System Skill (github.com/jakedahn/pomodoro) demonstrates the pattern in ~600 lines:

**Structure:**
```
pomodoro/
├── SKILL.md              # Claude's operating instructions
├── pomodoro              # Compiled CLI binary
├── pomodoro.db           # SQLite database (auto-created)
└── src/
    ├── pomodoro.ts       # CLI interface (~290 lines)
    ├── timer.ts          # Timer logic (~85 lines)
    └── db.ts             # Database operations (~170 lines)
```

**CLI Commands:**
- `start --task "Description"` - Begin session
- `stop` - End session early
- `status` - Check if running
- `history --days N` - View past sessions
- `stats --period week` - Analyze productivity

**Value Over Time:**
- Week 1: "Started 5 sessions"
- Week 4: "Morning sessions: 95% completion. Afternoon: 70%. Schedule deep work mornings."
- Month 3: "Coding: 90% completion. Docs: 72%. Try 15-minute sessions for documentation."

Study this implementation to understand how the pieces fit together.
</reference_implementation>

<summary>
## System Skill Pattern Summary

**The Pattern:** CLI + SKILL.md + Database

**The Insight:** Give Claude handles to operate a system, and it becomes something more than a chat agent - it becomes a system animator running its own OODA loop.

**Three Components:**
1. **CLI Binary** - Self-contained executable with helpful `--help`
2. **SKILL.md** - Tutorial for operating the system
3. **SQLite Database** - Persistent memory that compounds

**Emergent Properties:**
- Memory that compounds
- Pattern recognition
- Personalized intelligence
- Proactive insights
- Autonomous operation
- Cross-session continuity

**When to Use:**
- Personal data systems
- Workflows benefiting from history
- Tools that should learn from usage
- Long-term tracking and projects

**Key Files to Study:**
- pomodoro/SKILL.md - How to structure the tutorial
- pomodoro/src/pomodoro.ts - How to design the CLI
- pomodoro/src/db.ts - How to design the database
</summary>

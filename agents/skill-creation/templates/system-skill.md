---
name: {{SKILL_NAME}}
description: {{What it does}}. Use when {{trigger conditions}}. Demonstrates System Skill Pattern (CLI + SKILL.md + Database).
allowed-tools:
  - Bash
  - Read
---

<overview>
{{Brief description of what this system tracks/manages}}.

**This is a System Skill** - it provides handles to operate a personal data system. As commands run and data accumulates, context builds and compounds. The system learns patterns and provides increasingly valuable insights through an OODA loop of observation, orientation, decision, and action.
</overview>

<mental_model>
## The OODA Loop

Operating this skill involves running a continuous cycle:

1. **Observe** → Check current status (`./{{SKILL_NAME}} status`) and review history (`./{{SKILL_NAME}} history`)
2. **Orient** → Analyze patterns in the data (`./{{SKILL_NAME}} stats --period week`)
3. **Decide** → Determine optimal actions (e.g., "{{Example insight based on patterns}}")
4. **Act** → {{Primary action}} (`./{{SKILL_NAME}} start`), provide recommendations, celebrate milestones

Each cycle builds on accumulated data, making insights more valuable over time.
</mental_model>

<dependencies>
- Binary location: `~/.claude/skills/{{SKILL_NAME}}/{{SKILL_NAME}}`
- Database: Auto-created at `~/.claude/skills/{{SKILL_NAME}}/{{SKILL_NAME}}.db` on first run
- No external dependencies required
</dependencies>

<quick_decision_tree>
```
User task → What kind of request?
   ├─ {{Action request}} → Check status first, then {{action}}
   ├─ Check current state → Use status command
   ├─ Review patterns → Use stats command (day/week/month/year)
   ├─ View past {{entries}} → Use history command
   └─ Stop/end early → Use stop command
```
</quick_decision_tree>

<core_commands>
**To see all available options**: Run `./{{SKILL_NAME}} --help` or `./{{SKILL_NAME}} <command> --help`

## Starting {{an Entry}}

```bash
# Basic usage
./{{SKILL_NAME}} start --{{primary-flag}} "{{Example value}}"

# With options
./{{SKILL_NAME}} start --{{primary-flag}} "{{Example}}" --{{option}} {{value}}
```

**Options:**
- `--{{primary-flag}} <value>` - {{Description}} (required)
- `--{{option}} <value>` - {{Description}} (default: {{default}})

**JSON output:**
```bash
./{{SKILL_NAME}} start --{{primary-flag}} "{{Example}}" --json
```

## Checking Status

```bash
./{{SKILL_NAME}} status
./{{SKILL_NAME}} status --json  # For programmatic use
```

## Viewing History

```bash
./{{SKILL_NAME}} history --days 7    # Last 7 days
./{{SKILL_NAME}} history --days 30   # Last 30 days
./{{SKILL_NAME}} history --json      # For programmatic use
```

## Analyzing Patterns

```bash
./{{SKILL_NAME}} stats --period day     # Today's stats
./{{SKILL_NAME}} stats --period week    # This week
./{{SKILL_NAME}} stats --period month   # This month
./{{SKILL_NAME}} stats --period year    # This year
./{{SKILL_NAME}} stats --json           # For programmatic use
```

**Statistics include:**
- {{Stat 1}}
- {{Stat 2}}
- {{Stat 3}}
- {{Stat 4}}

## Stopping Early

```bash
./{{SKILL_NAME}} stop
```

Use when {{reason to stop early}}.
</core_commands>

<essential_workflows>
## Starting {{Primary Action}}

1. **Check for active {{entry}}**: `./{{SKILL_NAME}} status`
2. **If clear, start**: `./{{SKILL_NAME}} start --{{flag}} "{{Description}}"`
3. **Confirm to user**: "{{Confirmation message}}"

## Daily Review

1. **Fetch today's data**: `./{{SKILL_NAME}} stats --period day --json`
2. **Parse and present insights**:
   - "{{Example daily insight 1}}"
   - "{{Example daily insight 2}}"

## Weekly Analysis

1. **Fetch week's data**: `./{{SKILL_NAME}} stats --period week --json`
2. **Identify patterns**: Compare to previous periods, note trends
3. **Make recommendations**: "{{Example recommendation}}"
</essential_workflows>

<pattern_recognition>
## As Data Accumulates

**After a few {{entries}} (1-5):**
- Basic observations only
- "{{Example early observation}}"

**After several days (5-20 {{entries}}):**
- Simple trends visible
- "{{Example trend observation}}"

**After a week (20-50 {{entries}}):**
- Behavioral patterns emerge
- "{{Example pattern insight}}"

**After a month (50+ {{entries}}):**
- Deep insights available
- "{{Example deep insight}}"

**After several months (100+ {{entries}}):**
- Long-term trends clear
- "{{Example long-term insight}}"

**Commands for pattern recognition:**
```bash
./{{SKILL_NAME}} stats --period month --json
./{{SKILL_NAME}} history --days 90 --json
```
</pattern_recognition>

<milestones>
## Celebrate Progress

- **10 {{entries}}**: "First 10 complete - building the habit!"
- **50 {{entries}}**: "50 {{entries}} milestone - real patterns emerging!"
- **100 {{entries}}**: "100 {{entries}}! {{Celebration message}}"
- **High {{metric}}**: "{{Example achievement celebration}}"

To check milestone status: `./{{SKILL_NAME}} stats --period year --json`
</milestones>

<common_pitfalls>
## {{First Pitfall}}

❌ **Don't** {{bad practice}}
✅ **Do** {{good practice}}

**Why**: {{Explanation}}

## {{Second Pitfall}}

❌ **Don't** {{bad practice}}
✅ **Do** {{good practice}}

**Why**: {{Explanation}}

## {{Third Pitfall}}

❌ **Don't** {{bad practice}}
✅ **Do** {{good practice}}

**Why**: {{Explanation}}
</common_pitfalls>

<best_practices>
## {{Entry}} Management

- {{Best practice 1}}
- {{Best practice 2}}
- {{Best practice 3}}

## User Interaction

- Celebrate milestones
- Acknowledge {{positive metric}}: "{{Example acknowledgment}}"
- Suggest optimal {{times/approaches}} based on historical data
- Point out improvements: "{{Example improvement observation}}"

## Data Analysis

- Review daily stats at end of each day
- Check weekly patterns for insights
- Track trends over time (month, year)
- Use JSON output for custom analytics
</best_practices>

<technical_notes>
## JSON Output

All commands support `--json` flag:
```bash
./{{SKILL_NAME}} status --json
./{{SKILL_NAME}} history --json
./{{SKILL_NAME}} stats --json
```

## Database

- **Location**: `~/.claude/skills/{{SKILL_NAME}}/{{SKILL_NAME}}.db`
- **Format**: SQLite with `{{table_name}}` table
- **Persistence**: All {{entries}} saved permanently
- **Growth**: Database grows with use
- **Analytics**: Richer insights as data accumulates

**Schema**:
```sql
CREATE TABLE {{table_name}} (
  id INTEGER PRIMARY KEY,
  {{field_1}} TEXT NOT NULL,
  {{field_2}} INTEGER NOT NULL,
  created_at TEXT NOT NULL,
  completed_at TEXT
);
```

## Binary Location

- **Path**: `~/.claude/skills/{{SKILL_NAME}}/{{SKILL_NAME}}`
- **From skill directory**: `./{{SKILL_NAME}}`
- **Full path from anywhere**: `~/.claude/skills/{{SKILL_NAME}}/{{SKILL_NAME}}`
</technical_notes>

<success_criteria>
{{SKILL_NAME}} is working well when:
- {{Entry}} data accumulates over time
- Pattern recognition improves with volume
- Insights become more personalized
- Value demonstrably compounds with usage
- User behavior is understood and reflected back
</success_criteria>

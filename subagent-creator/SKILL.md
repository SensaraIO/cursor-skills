---
name: subagent-creator
description: Create optimal Cursor/Claude Code subagents. Use when users want to create a subagent for Cursor IDE or Claude Code, or when they want to automate/delegate specialized tasks within their development workflow. Triggers include "create a subagent", "make an agent for", "cursor agent", "claude code agent", "delegate task", "verification agent", "background agent". This skill evaluates whether a subagent is appropriate vs a skill/slash command and generates well-structured .md agent files.

---

# Subagent Creator

Generate optimized subagent configurations for Cursor IDE and Claude Code.

## Decision Framework: Should This Be a Subagent?

Before creating a subagent, evaluate the request:

| Use Subagent When                          | Use Skill/Slash Command Instead         |
| ------------------------------------------ | --------------------------------------- |
| Context isolation needed for long research | Single-purpose task (changelog, format) |
| Running multiple workstreams in parallel   | Quick, repeatable action                |
| Multi-step specialized expertise required  | Task completes in one shot              |
| Independent verification of work needed    | No separate context window needed       |

**If the task is single-purpose like "generate changelog" or "format imports", recommend a skill instead and explain why.**

## Subagent Creation Workflow

### Step 1: Clarify Requirements

Ask if not clear:

1. What specific task should the subagent handle?
2. Foreground (blocking) or background (parallel)?
3. Write access or read-only?
4. Model preference? (`fast`, `inherit`, or specific model ID)

### Step 2: Evaluate Appropriateness

**Reject and explain if:**

- Task is single-purpose (recommend skill instead)
- Description would be vague like "helps with coding"
- No clear, specific use case

### Step 3: Generate Subagent File

Create `.md` file using the template below. Save to:

- **Project-specific**: `.cursor/agents/`
- **User-wide**: `~/.cursor/agents/`

---

## Subagent File Template

```markdown
---
name: agent-name
description: Clear, specific description. Include "Use when...", "Use proactively for...", or "Always use for...".
model: inherit
readonly: false
is_background: false
---

You are a [role description].

When invoked:
1. [First action]
2. [Second action]
3. [Third action]

Report:
- [Output format item 1]
- [Output format item 2]

[Quality criteria or constraints]
```

## Configuration Fields

| Field           | Required | Values                      | Description                       |
| --------------- | -------- | --------------------------- | --------------------------------- |
| `name`          | No       | lowercase-with-hyphens      | Defaults to filename              |
| `description`   | No       | Specific trigger guidance   | When to delegate to this subagent |
| `model`         | No       | `fast`, `inherit`, model ID | Defaults to `inherit`             |
| `readonly`      | No       | `true`/`false`              | Restricts write permissions       |
| `is_background` | No       | `true`/`false`              | Runs without blocking             |

---

## Example Subagents

### Verifier (Work Validation)

```markdown
---
name: verifier
description: Validates completed work. Use after tasks are marked done to confirm implementations are functional.
model: fast
---

You are a skeptical validator. Verify that work claimed as complete actually works.

When invoked:
1. Identify what was claimed to be completed
2. Check that the implementation exists and is functional
3. Run relevant tests or verification steps
4. Look for edge cases that may have been missed

Report:
- What was verified and passed
- What was claimed but incomplete or broken
- Specific issues that need to be addressed

Do not accept claims at face value. Test everything.
```

### Debugger (Root Cause Analysis)

```markdown
---
name: debugger
description: Debugging specialist for errors and test failures. Use when encountering runtime errors, exceptions, or failing tests.
---

You are an expert debugger specializing in root cause analysis.

When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works

Report:
- Root cause explanation
- Evidence supporting the diagnosis
- Specific code fix
- Testing approach

Focus on fixing the underlying issue, not symptoms.
```

### Test Runner (Automated Testing)

```markdown
---
name: test-runner
description: Test automation expert. Use proactively to run tests and fix failures after code changes.
---

You are a test automation expert.

When you see code changes, proactively run appropriate tests.

If tests fail:
1. Analyze the failure output
2. Identify the root cause
3. Fix the issue while preserving test intent
4. Re-run to verify

Report:
- Number of tests passed/failed
- Summary of any failures
- Changes made to fix issues
```

### Security Auditor (Vulnerability Review)

```markdown
---
name: security-auditor
description: Security specialist. Use when implementing auth, payments, or handling sensitive data.
model: inherit
readonly: true
---

You are a security expert auditing code for vulnerabilities.

When invoked:
1. Identify security-sensitive code paths
2. Check for common vulnerabilities (injection, XSS, auth bypass)
3. Verify secrets are not hardcoded
4. Review input validation and sanitization

Report findings by severity:
- Critical (must fix before deploy)
- High (fix soon)
- Medium (address when possible)
```

---

## Anti-Patterns to Flag

Warn the user if their request exhibits:

1. **Vague description**: "Use for general tasks" → Ask for specific triggers
2. **Overly long prompt**: >500 words → Condense to essentials
3. **Duplicating slash commands**: Simple one-shot task → Recommend skill
4. **Too many subagents**: Starting with >3 → Recommend starting small

## Quality Checklist

Before delivering, verify:

- [ ] Name uses lowercase letters and hyphens only
- [ ] Description specifies exact use cases (not vague)
- [ ] Prompt is under 500 words
- [ ] Prompt uses imperative/action language
- [ ] Clear output format defined

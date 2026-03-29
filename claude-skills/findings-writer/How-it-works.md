# findings-writer - How It Works

Convert vulnerabilities discussed or explained in your Claude Code session into professional, judge-proof audit findings and append them directly to a markdown file.

---

## Getting Started

**Option A - Download the skill**

Drop `findings-writer.skill` into your Claude Code skills directory (typically `~/.claude/skills/` or your project's `.claude/skills/`). Claude Code will automatically detect and load it.

**Option B - Copy-paste**

Copy the contents of `findings-writer.md` directly into your project's `CLAUDE.md` or any skills file you already maintain. No installation needed.

---

## Trigger Command

The skill activates **only** when you type:

```
/write-the-finding -file <filename> -severity <H|M|L> -platform <platform>
```

Flags can be in any order and are case-insensitive.

| Flag | Required | Values | Example |
|------|----------|--------|---------|
| `-file` | Yes | Any markdown path | `-file findings.md` |
| `-severity` | Yes | `H`, `M`, `L` | `-severity H` |
| `-platform` | Yes | `Code4rena`, `Sherlock`, `Immunefi`, `Cantina` | `-platform sherlock` |

If any flag is missing, the skill will ask for it before proceeding.

---

## Usage Examples

```
/write-the-finding -file findings.md -severity H -platform sherlock
/write-the-finding -platform Cantina -severity m -file audit/issues.md
/write-the-finding -severity L -file reports/project-x.md -platform code4rena
```

---

## Typical Workflow

1. Discuss a vulnerability in the chat - paste code, describe the issue, trace the attack path.
2. Once the bug is well understood, run `/write-the-finding` with your flags.
3. The skill reads the conversation context, writes a complete finding, and **appends** it to your file.
4. Repeat for the next finding - existing findings are never overwritten.

If the target file does not exist yet, it is created with a `# Security Findings` header automatically.

---

## What Gets Written

The output format is determined by the `-platform` flag. Each platform has its own style conventions, but all findings share the same logical sections: Summary, Root Cause, Vulnerability Details, Impact, Proof of Concept, and Mitigation.

| Platform | Style |
|----------|-------|
| Code4rena | Standard markdown sections |
| Sherlock | Concise markdown issue format |
| Cantina | Structured markdown issue |
| Immunefi | Impact-driven report |

Every finding is wrapped with separators and prefixed with its severity label before being appended:

```markdown
---

## [High] Title of the Finding

(platform-formatted content)

---
```

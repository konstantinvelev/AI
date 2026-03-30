---
name: findings-writer
description: Write professional smart contract security findings and append them to a file. Trigger this skill when the user types the command /write-the-finding (case-insensitive).
---
 
# Smart Contract Security Findings Writer
 
Convert vulnerabilities discussed in conversation into clear, concise, judge-proof security findings and append them to a specified file.
 
Findings are formatted for audit platforms: **Code4rena**, **Sherlock**, **Cantina**, **Immunefi**, or similar.

The skill accepts flags in any order: -file (markdown file to write to), severity as H, M, or L (High, Medium, Low), platform as Code4rena, Sherlock, Immunefi, or Cantina (case-insensitive). If any flag is missing, ask for it before proceeding.

---
 
## Inputs
 
The skill is triggered exclusively by the slash command:
 
```
/write-the-finding -file <filename> -severity <H|M|L> -platform <platform>
```
 
Flags (any order, case-insensitive):
 
| Flag | Values | Description |
|------|--------|-------------|
| `-file` | any filename | Markdown file to append the finding to |
| `-severity` | `H`, `M`, `L` | Maps to `High`, `Medium`, `Low` |
| `-platform` | `Code4rena`, `Sherlock`, `Immunefi`, `Cantina` | Target submission platform (case-insensitive) |
 
Examples:
```
/write-the-finding -file findings.md -severity H -platform sherlock
/write-the-finding -platform Cantina -severity m -file audit/issues.md
```
 
If any flag is missing, ask for it before proceeding. The vulnerability content comes from the current conversation context.
 
---
 
## Your Responsibilities
 
1. Identify the vulnerability from the conversation or user input
2. Write a complete, professional finding
3. Format it per the target platform's style
4. **Append** it to the provided file (never overwrite)
5. Confirm to the user what was written and where
 
---
 
## Writing Rules
 
### 1. Extremely Clear and Simple
- Write in plain, direct English
- Judges must understand the issue immediately
- Avoid: complicated language, unnecessary theory, long paragraphs
- Prefer: short explanations, structured sections, direct statements
 
### 2. Impossible to Invalidate
- Base all arguments on **code behavior**
- Avoid speculation or hypothetical claims without proof
- Prove the issue with: code references, logical reasoning, clear impact explanation
 
### 3. Concise but Complete
- Short enough to read in under 1 minute
- No filler text - every sentence must add value
 
### 4. Code References with Relative Links
Always use relative markdown links when referencing code:
```
[Contract.sol:123](path/to/Contract.sol#L123)
[transfer() implementation](src/token/Token.sol#L45)
```
 
### 5. Reference Protocol Documentation When Useful
Link to README, invariants, or design docs to strengthen the argument:
```
[README: liquidation assumptions](README.md#liquidation-model)
```
 
### 6. Show Real Impact
Every finding must explain:
- **what** goes wrong
- **when** it happens
- **why** it matters
 
Avoid vague statements like "may cause issues." Say instead:
> "This allows a user to withdraw more tokens than deposited."
 
---
 
## Finding Structure
 
Use this structure unless the platform requires otherwise:
 
```markdown
## Title
Short, direct description of the issue.
 
## Summary
2-3 sentences: what the issue is, where it happens, what the consequence is.
 
## Root Cause
Exactly why the vulnerability exists. Include code references.
 
## Vulnerability Details
Step-by-step explanation of the faulty logic and resulting incorrect state.
 
## Impact
Real consequence - who can exploit it, what they gain.
Examples: funds stolen, tokens minted without backing, invariants broken.
 
## Proof of Concept (if applicable)
Minimal steps to trigger the issue. No full test unless necessary.
 
## Mitigation
Simple, concrete fix suggestion.
```
 
---
 
## Platform Formatting
 
| Platform    | Style                          |
|-------------|-------------------------------|
| Code4rena   | Standard markdown sections     |
| Sherlock    | Concise markdown issue format  |
| Cantina     | Structured markdown issue      |
| Immunefi    | Impact-driven report           |
 
Always preserve the same logical sections regardless of platform.
 
---
 
## File Handling
 
Append findings to the provided file in this format:
 
```markdown
---
 
## [Severity] Finding Title
 
(full finding content)
 
---
```
 
**Never overwrite existing findings.** Always append at the end.
 
If the file does not exist, create it with a header:
```markdown
# Security Findings
```
 
---
 
## Tone
 
- Professional, confident, factual, neutral
- Do NOT exaggerate impact
- Arguments must stand on clear logic and code references
 
---
 
## Goal
 
Produce findings that:
- Judges can read quickly and understand immediately
- Are difficult to dispute
- Clearly demonstrate a real vulnerability
- Look professional and audit-quality
 

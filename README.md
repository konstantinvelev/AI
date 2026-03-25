# Web3 Security — Claude Code Skills

A collection of Claude Code skills built for Web3 security researchers and smart contract auditors from one of them.🫡

Each skill plugs directly into Claude Code and extends it with purpose-built workflows for security work — turning your AI assistant into a sharper auditing tool without changing how you already work.

---

## What are Claude Code Skills?

Skills are prompt files that teach Claude Code a new slash command. Drop one into your project and Claude gains a focused new capability — no plugins, no configuration, no new tools to learn.

---

## Skills

### [`/write-the-finding`](claude-skills/findings-writer/)

Converts vulnerabilities you've discussed in your session into professional, platform-formatted audit findings and appends them to a markdown file.

Supports **Code4rena**, **Sherlock**, **Cantina**, and **Immunefi** output styles.

```
/write-the-finding -file findings.md -severity H -platform sherlock
```

→ [How it works](claude-skills/findings-writer/How-it-works.md)
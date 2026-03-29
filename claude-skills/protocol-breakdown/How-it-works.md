# protocol-breakdown - How It Works

Build the auditor's complete mental model of a smart contract protocol before hunting bugs. This skill reads the entire codebase - contracts, tests, scripts, configs, and docs - and produces a structured, deep-dive briefing covering architecture, money flows, invariants, and a concrete attack surface map.

---

## Getting Started

**Option A - Download the skill**

Drop `protocol-breakdown.md` into your Claude Code skills directory (typically `~/.claude/skills/` or your project's `.claude/skills/`). Claude Code will automatically detect and load it.

**Option B - Copy-paste**

Copy the contents of `protocol-breakdown.md` directly into your project's `CLAUDE.md` or any skills file you already maintain. No installation needed.

---

## Trigger Command

The skill activates **only** when you type:

```
/breakdown
```

Also accepted as natural-language variants:

| Trigger | Example |
|---------|---------|
| `/breakdown` | `/breakdown` |
| `protocol breakdown` | `protocol breakdown` |
| `break down the protocol` | `break down the protocol` |
| `run breakdown` | `run breakdown` |

> **Note**: `/audit` does NOT trigger this skill. Breakdown is the prerequisite step *before* auditing - it builds context, not findings.

---

## Usage Example

```
/breakdown
```

Once triggered, the skill asks one question before producing output:

```
Do you want this printed to chat or saved as a .md file?
```

Answer with `chat` to stream the full analysis inline, or provide a file path (e.g., `breakdown.md`) to write it to disk.

---

## Typical Workflow

1. Point Claude Code at the protocol's root directory.
2. Run `/breakdown`.
3. Claude reads **every file** - contracts, imports, tests, deployment scripts, configs, and docs - before writing a single word of output.
4. Receive the complete structured briefing.
5. Use the briefing as the foundation for `/audit` or manual bug hunting.

---

## What Gets Produced

The output is one continuous structured document, divided into six phases:

### TL;DR Box (always first)

A quick-reference summary panel showing protocol name, chain, language, type, contract count, approximate LOC, complexity rating, previous audit history, known issue count, and the top three risk areas.

### Phase 1 - Protocol Summary

Plain-English explanation of what the protocol does, its category (lending, DEX, vault, bridge, staking, etc.), and what it resembles or forks from. Includes the full system architecture - every contract with a one-sentence purpose, inheritance trees, external dependencies, and key design decisions (access control model, upgradeability, oracle architecture, reentrancy protection).

### Phase 2 - Core Components Deep Dive

For every core contract: purpose, all key state variables, a plain-English walkthrough of algorithms and math with **concrete numeric examples**, state transition sequences, external dependencies, and trust assumptions. Peripheral contracts get the same structure at lighter depth.

Example of the numeric depth expected:

```
The vault uses ERC4626 share accounting. When Alice deposits 100 USDC:
- Current: totalAssets=2000, totalShares=1000, price=2.0/share
- Alice receives: 100/2.0 = 50 shares
- New: totalAssets=2100, totalShares=1050, price=2100/1050=2.0 ✓
```

### Phase 3 - User Interactions & Entrypoints

Step-by-step execution flows for every user-facing function (checks → updates → transfers), every privileged role with specific damage potential if compromised, keeper/bot entrypoints, and end-to-end lifecycle flows (deposit → earn → withdraw, borrow → repay → liquidation, emergency/pause, migration).

### Phase 4 - Money Flows

Traces every token movement: inflows (which functions accept assets and where they land), internal movements between contracts, outflows (withdrawal checks, reward distribution, fee extraction), the complete fee structure, the token accounting model with numeric examples, and external protocol integrations with failure mode analysis.

### Phase 5 - Invariants & Assumptions

Exhaustive list of everything that must remain true: accounting invariants, access control invariants, ordering constraints, economic invariants, external dependency assumptions, and any invariants stated in property tests (with a completeness assessment).

### Phase 6 - Attack Surface & Auditor Battle Plan

Protocol-specific security analysis - not generic checklists. Includes:
- **High-risk areas**: specific contracts, functions, and line ranges with reasoning
- **Weakness patterns**: concrete code references and what bug class to look for
- **Trust assumptions that could break**: each assumption + the exact failure mode
- **Previous audit findings**: what was fixed vs. acknowledged, and underestimated issues
- **Auditor battle plan**: ordered investigation strategy - which contract first, critical paths, cross-contract pairs to analyze jointly, and fuzzing targets

---

## Supported Languages & Chains

| Language | Chains |
|----------|--------|
| Solidity | Ethereum, Arbitrum, Optimism, Base, and other EVM chains |
| Rust + Anchor | Solana |
| CosmWasm | Cosmos SDK chains |

Terminology adapts automatically to the chain (e.g., Solana output uses programs, instructions, PDAs, and accounts instead of contracts, functions, and mappings).

---

## What This Skill Is NOT

- It does **not** find bugs or report vulnerabilities - that is `/audit`'s job.
- It does **not** skim files. It reads every in-scope contract in full before producing any output.
- It does **not** trust the README. It verifies claims against the code and flags discrepancies.

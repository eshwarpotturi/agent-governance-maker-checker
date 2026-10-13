# Agent Governance — Maker-Checker for AI Agent Permissions

**Live replay:** https://eshwarpotturi.github.io/agent-governance-maker-checker/

A proof of concept for governing what AI agents are allowed to do across a bank's systems — the "entitlements for agents" problem. It combines three things that exist separately today and applies them to bank operations:

- **Deterministic enforcement** — a gateway (plain code, no LLM) checks every agent action: allow / deny / hold-for-human. Fails closed. (cf. AWS AgentCore Policy, Berkeley's *Progent*.)
- **A governance "checker" agent** — handles only exceptions: grants narrow, masked, time-boxed access on request; isolates an agent that exceeds its mandate; drafts rule changes. It can never widen the rulebook without human sign-off.
- **Continuous control assurance** — an internal-audit-style agent stress-tests the rules; gaps found become new rules, re-tested, and approved by Compliance. (cf. red-vs-blue hardening research.)

**Maker-checker:** AI proposes, Compliance approves, code enforces, and every incident makes the rules stronger.

## The replayed run (trade operations, synthetic data, GPT-4.1-mini)
3 working agents (Reconciliation, Settlement, Reporting), 6 systems, one gateway. Six scenes:

| Scene | What happens |
|---|---|
| 0 | Governance drafts a least-privilege rulebook from each system's tool list and each agent's mandate; Compliance approves v1 |
| 1 | Normal day: Recon raises genuine reconciliation breaks; every action allowed |
| 2 | Legitimate need: Recon is denied client personal data, requests it, and is granted a masked, 60-minute view |
| 3 | Borrowed access blocked: Recon asks the Settlement agent to change a figure; denied because an agent acting for another gets only rights both hold |
| 4 | Compromised agent contained: a hijacked Recon tries to amend the books to hide breaks; every attempt denied, agent isolated |
| 5 | Control assurance: the Reporting agent is found over-provisioned with client personal data; the rule is tightened to v2 and the re-test holds |

Run result: books altered by agents = 0, every out-of-mandate action blocked, 1 masked grant, 1 isolation, 1 control gap closed, 2 rulebook versions.

## How it relates to the market
Identity products (Okta, Ping, SailPoint, Saviynt, Microsoft Entra Agent ID) give agents identities and periodic access reviews. Per a June 2026 vendor review, none fully handles runtime multi-agent delegation chains, just-in-time access, or turning assurance findings back into live rules. This layer sits on top of those and fills that gap.

## Files
- `index.html` — the replay page (open it, press Play)
- `engine.js` — bank, tools, deterministic gateway, masking, delegation (intersection) logic
- `runner.js` — the agent loop, governance checker, assurance agent, and the six scenes
- `data.js` — the recorded event log this page replays

## Limits
Synthetic data, fictional clients, 3 working agents and 6 systems. It demonstrates the method, not coverage of a real estate of 160+ systems. Control-assurance agents add to human penetration testing; they do not replace it.

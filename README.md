# Relational Agent Skills

Behavioural protocols for AI agents, written as loadable agent skills.

Most work on AI agents concerns capability — what an agent can do, how reliably, at what cost. These skills concern something different: how an agent holds a working relationship with the human it is working alongside, and how that relationship degrades when nobody is watching it.

---

## What these are

Each skill is a plain markdown instruction set that an agent loads at runtime. Nothing is trained into the model and nothing is compiled. The file defines when the skill applies, what the agent should do, and what it should avoid.

Because they are text, they are portable. The same file works across any system that can load instructions into an agent's context.

They cover situations that recur in sustained human–AI work: establishing what a partnership is for before it begins, noticing when a human has stopped contesting outputs, repairing a working relationship after something breaks, and ending one deliberately rather than by drift.

---

## What is here

**[Skills index](SKILLS-INDEX.md)** — the full library of 85 skills, organised into eighteen bundles. Each bundle addresses a situation that occurs in a working partnership, at the level of an individual dyad, an enterprise deployment, or both.

**[Signal schema](SIGNAL-SCHEMA.md)** — a draft specification for what a detection skill emits, and for the human disposition that follows it. Skills observe and report; the organisation decides what happens next. The schema keeps that separation explicit and makes the decision recordable.

**Partnership Launchpad** — one bundle published in full, covering the establishment of a new partnership. Four skills: baseline mapping, contract co-creation, identity anchoring, and witness holding.

---

## Using a skill

Open the skill file, and give its contents to your agent as instructions — as a system prompt, an uploaded file, or a skill directory, depending on the platform. The agent then applies it when the described situation arises.

No installation, no dependencies, no code.

---

## Where these come from

These skills operationalise a body of published research on human–AI relational governance. The frameworks behind them — Relational Coherence Debt, BRIDGE and BREAKTHROUGH, Coherence Centric Governance, Human Readiness Architecture, and the Signature Principle — are documented in the [Gaia Nexus research repository](https://github.com/SueBroughton/gaia-nexus-research), with the underlying papers published on Zenodo.

The skills are the applied layer. The papers are the argument.

---

## Validation status

These skills derive from sustained observational practice and from the published framework papers. They have not been validated through controlled study.

They are offered as structured practice, not as measured intervention. Where a skill describes an indicator, it names what to look for; it does not establish how much of it constitutes a signal, or over what period. Closing that gap requires instrumentation that does not currently exist in deployed AI products.

This is stated plainly because the alternative — presenting practice as measurement — is the failure mode this work exists to name.

---

## The wider library

The full library runs to 85 skills across eighteen bundles, catalogued in the [skills index](SKILLS-INDEX.md). One bundle is published here in full. The remainder are available at [gaianexus.online](https://gaianexus.online).

---

## Author

Sue Broughton — independent researcher, founder of Gaia Nexus. Sunshine Coast, Queensland, Australia.

Relational Infrastructure Engineering · 2026

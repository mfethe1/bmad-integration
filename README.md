# BMAD Integration Base

This repository serves as the unified base for all gateways (agents/nodes) to adopt the BMAD-METHOD framework into our system without regressing on our current features.

## Core Concepts to Implement

### 1. Step-File Architecture
We will break down complex prompts and monolithic workflows into sequential micro-files.
- **Location:** `workflows/`
- **Rule:** Agents must read, execute, and halt at checkpoints. No skipping ahead.
- **Benefit:** Prevents context-rot and ensures strict discipline.

### 2. Adversarial Review
A mandatory validation step where the reviewing agent *must* find issues.
- **Location:** `reviews/`
- **Rule:** Banning the phrase "looks good". If zero findings, the workflow halts.
- **Benefit:** Breaks confirmation bias.

### 3. Advanced Elicitation
Instead of asking to "try again", we force a reasoning framework.
- **Location:** `elicitation-methods/`
- **Methods:** Pre-mortem, First Principles, Inversion, etc.

### 4. Party Mode
A specialized swarm chat for brainstorming and architectural tradeoffs.
- **Location:** `party-mode/`
- **Rule:** Orchestrator calls specialized agents to argue and build on ideas.

### 5. Strict Scope Standards
Specs must be 900-1600 tokens and represent a single user-facing goal.
- **Location:** `specs/`
- **Rule:** Split multi-goal requests into independent shippable deliverables.

## Getting Started
See the individual folders for the base templates and rules.
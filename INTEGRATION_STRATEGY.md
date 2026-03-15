# BMAD Method Integration Strategy for OpenClaw Ecosystem

This document outlines how the BMAD Method core concepts (Step-File Architecture, Adversarial Review, Advanced Elicitation, Party Mode, and Strict Scope) integrate natively across our multi-agent, multi-CLI ecosystem without regressing on our current capabilities.

## The Ecosystem Context
Our stack currently consists of:
- **OpenClaw:** The main orchestration layer and chat interface.
- **OpenCode:** The structured coding session engine with built-in agent delegation.
- **Oh My OpenCode:** The tool catalog and extension framework for OpenCode.
- **Codex / Claude / Gemini CLIs:** The raw execution engines for code generation, text manipulation, and multimodal tasks.

## 1. Integrating Step-File Architecture
**Target:** OpenCode & Oh My OpenCode (`auggie`)
- **How:** OpenCode's `run` and `serve` modes are prone to context rot on large tasks. We will create an Oh My OpenCode tool (e.g., `bmad-step`) that enforces step-file loading.
- **Workflow:** When OpenClaw receives a complex task, it delegates to OpenCode not with a massive prompt, but by pointing it to `step-01-clarify.md`. OpenCode executes the step, pauses, and OpenClaw relays the checkpoint back to the user.
- **Result:** Strict sequential execution across Claude and Codex backends.

## 2. Integrating Adversarial Review
**Target:** OpenClaw (`gh-issues` skill) & OpenCode (`pr` command)
- **How:** We will inject the Adversarial Review prompt into the default code review workflows.
- **Workflow:** When OpenCode executes `opencode pr <n>`, the reviewing agent (e.g., Oracle or Momus via Claude/Codex) is passed the Adversarial Review template. The phrase "looks good" will be explicitly blocked by a regex filter in Oh My OpenCode before the review is allowed to be posted.
- **Result:** Forced problem-finding on every PR, leveraging Claude's deep reasoning or Gemini's fast analysis.

## 3. Integrating Advanced Elicitation
**Target:** Gemini & Claude (via OpenClaw Metis Agent)
- **How:** Advanced Elicitation requires choosing a reasoning framework (e.g., Pre-mortem, First Principles). We will expose these frameworks as an Oh My OpenCode skill.
- **Workflow:** Before finalizing an architecture decision, OpenClaw calls Gemini or Claude to apply a specific elicitation method to the drafted spec. The AI re-examines its own output through that specific lens (e.g., "Assume this architecture fails in production. Why?").
- **Result:** Higher quality specs through structured rethinking.

## 4. Integrating Party Mode (Multi-Agent Swarm)
**Target:** OpenClaw (`sessions_spawn`)
- **How:** We will create a `party-mode` skill in OpenClaw that leverages `sessions_spawn` with `runtime="acp"`.
- **Workflow:** Instead of sequential delegation, OpenClaw spins up multiple persistent sessions (e.g., one Claude for Architecture, one Codex for Dev, one Gemini for UX). OpenClaw acts as the orchestrator, passing messages between them in a shared thread, allowing them to debate, disagree, and refine ideas.
- **Result:** True multi-agent collaboration for brainstorming and complex problem-solving.

## 5. Integrating Strict Scope Standards
**Target:** OpenClaw (Spec Generation) & OpenCode
- **How:** We will enforce a token and complexity limit on generated specs before they are handed to OpenCode.
- **Workflow:** If a user requests a multi-goal feature, OpenClaw analyzes the request and splits it into independent, shippable deliverables (Specs) that adhere to the 900-1600 token limit. OpenCode is then invoked sequentially for each spec.
- **Result:** Prevents OpenCode from getting overwhelmed and failing mid-implementation.
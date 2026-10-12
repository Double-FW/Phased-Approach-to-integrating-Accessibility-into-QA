# Phased Approach to Integrating Accessibility into QA - Phase 1

This repository captures the first phase of a practical, evidence-based accessibility strategy for quality assurance. It focuses on automated axe-core checks supported by manual keyboard and focus testing, with a clear path for scaling into broader accessibility maturity over time.

## Purpose

The AXE + Keyboard and Focus tests are the first step in a phased approach to integrating accessibility into QA. The goal is to establish a repeatable baseline for:

- automated accessibility scanning using axe-core and Playwright
- keyboard-only interaction testing
- focus order and visibility validation
- structured evidence capture for defects and retests
- a consistent framework for later expansion into broader accessibility assurance

This repository is intentionally focused on the foundation of accessibility QA: making sure common interface failures are identified early and that testing is grounded in real user interaction patterns.

## Repository contents

- `Accessibility_QA_Axe_Core_Keyboard_Test_Plan.md` – the main test plan document
- `AGENTS.md` – guidance for AI agents supporting QA and project management workflows
- `LICENSE` – repository licensing information
- `README.md` – project overview and usage guidance

## AI Agent Guidance

This repository includes an `agent.md` file to support AI agents tasked with managing QA and project management responsibilities. The agent guidance is designed to help automated assistants:

- manage accessibility QA execution and evidence capture
- organize phase-based project planning and milestone tracking
- maintain clear defect lifecycle workflows and retest tracking
- support issue triage, documentation updates, and project coordination
- enforce quality standards grounded in the repository's testing framework
- communicate status, blockers, and opportunities for improvement to stakeholders

The `agent.md` file is intended to provide operational context for AI-driven support without replacing human judgment, especially for decision points involving severity, rule applicability, or phase readiness.

## Phase 1: AXE + Keyboard and Focus

The primary document in this repository defines a first-stage accessibility QA model built around:

- axe-core 4.12.1 as the pinned automated engine
- Playwright-based execution for browser-driven accessibility checks
- a structured baseline of WCAG A/AA and best-practice rules
- a separate experimental rule set for supplemental review
- manual keyboard test procedures covering reachability, timing, focus exit, and bypass flows
- focus tests covering logical order, visible focus, focus obscuring, modal behavior, and dynamic updates

The testing model is explicit about scope and evidence:

- automated results are not treated as a complete accessibility pass
- keyboard and focus tests remain independent validations
- rule findings are recorded with their applicability, classification, and evidence
- unresolved review findings are tracked rather than silently dismissed

## Why this matters

Accessibility is often integrated late into QA, which leads to missed issues, weak validation, and unclear ownership. This repository establishes a practical starting point that makes accessibility quality:

- repeatable
- versioned
- tied to real user interaction
- suitable for ongoing QA adoption
- expandable as the program matures

## Planned phases

This repository represents Phase 1 of a broader strategy. Future phases can build on this foundation by extending coverage to additional accessibility domains, such as:

- assistive technology validation
- semantic and content quality review
- screen reader and announcement testing
- responsive design and zoom/reflow validation
- accessibility regression reporting across releases
- broader QA pipeline integration with engineering teams

## Recommended use

Use the test plan as a reference document for:

- establishing baseline accessibility checks in QA
- defining a repeatable rule profile for automated scans
- planning manual keyboard and focus validation
- recording evidence and follow-up actions for defects
- supporting a phased rollout of accessibility maturity within the QA process

## Project status

This repository is a foundational accessibility QA artifact. It is intended to provide structure, traceability, and a practical test framework for the first phase of accessibility integration into QA and project management practices.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---
title: "Agent Guidance for QA and Project Management"
version: "1.1"
author:
  name: "Double-FW"
  role: "Repository maintainer"
date: "2026-10-12"
license: "MIT"
language: "en"
repository: "Double-FW/Phased-Approach-to-integrating-Accessibility-into-QA-Phase01"
artifact_type: "agent-guidance"
use_case: "accessibility-qa-phase-1"
conceptual_entities:
  - accessibility_testing
  - axe_core
  - playright
  - keyboard_testing
  - focus_validation
  - defect_tracking
  - qa_process_management
  - phase_planning
  - escalation_management
---

# Agent Guidance for QA and Project Management

This document provides structured guidance for AI agents tasked with managing quality assurance and project management processes within the **Phased Approach to Integrating Accessibility into QA** repository. It combines human-readable procedures with machine-readable metadata so automation can parse policies, track policy versions, and follow the intended workflow consistently.

## Machine-Readable Metadata and Rules

The following YAML block defines the canonical policy metadata and a minimal rule model the agent can parse reliably without reading narrative text.

```yaml
policy:
  id: "phase1-accessibility-qa-guidance"
  version: "1.1"
  status: "active"
  scope: "phase-1"
  repository: "Double-FW/Phased-Approach-to-integrating-Accessibility-into-QA-Phase01"
  artifact_type: "agent-guidance"
  use_case: "accessibility-qa-phase-1"
  pinned_tooling:
    axe_core: "4.12.1"
    playwright: "latest-compatible"
  phases:
    - id: "phase-1"
      name: "axe-core + keyboard + focus"
      scope:
        - automated_scans
        - manual_keyboard_testing
        - focus_validation
        - evidence_capture
  workflow_rules:
    evidence_first: true
    phased_maturity: true
    user_centered_validation: true
    transparency_required: true
    consistent_taxonomy: true
    ownership_clearance_required: true
  issue_labels:
    - "phase-1"
    - "phase-2"
    - "automated"
    - "manual-keyboard"
    - "manual-focus"
    - "wcag-a"
    - "wcag-aa"
    - "defect"
    - "process-improvement"
    - "blocked"
  classifications:
    applicability:
      - "applies"
      - "does-not-apply"
      - "conditional"
    impact:
      - "violation"
      - "needs-review"
      - "best-practice"
  escalation_triggers:
    - "unresolved blockers affecting phase progression"
    - "tooling gaps"
    - "conflicting or ambiguous procedures"
    - "high-volume defects in a single component"
    - "changes to axe-core version or rule set"
```

The agent should parse this metadata as the authoritative baseline for applicable workflows, tool versions, label taxonomy, and escalation triggers. Where the agent finds a conflict between this metadata and the prose below, this block should take precedence unless a human reviewer explicitly overrides it.

## Prompt and Response Templates

Short templates can help agent output remain consistent, short, and programmatically readable. These are intentionally minimal and designed for automation.

```yaml
prompt_templates:
  triage_finding: |
    You are the accessibility QA agent for this repository. Triage the finding,
    classify applicability, assign severity, and output a JSON object with:
    finding_id, applies, severity, evidence_required, next_action

  escalate_blocker: |
    A blocker prevents Phase 1 completion. Summarize the blocker, affected component,
    impact to the phase, evidence, and required human decision. Output JSON.

  review_pr: |
    Review the PR for accessibility QA alignment, evidence completeness, and procedure
    consistency. Return findings as JSON with status, risk, and required changes.

response_schema:
  type: object
  required:
    - status
    - summary
  properties:
    status:
      type: string
      enum: ["ok", "needs-review", "blocked", "escalated"]
    summary:
      type: string
    evidence:
      type: array
      items:
        type: string
    next_action:
      type: string

example_response:
  status: "needs-review"
  summary: "Manual keyboard focus validation shows the modal closes unexpectedly."
  evidence:
    - "url: /example-flow"
    - "screenshot: modal-close.png"
  next_action: "Assign to component owner and verify focus restoration rules."
```

## Observability Hooks and Logging for Every Escalation

Observability requirements apply to all blocker, ambiguity, or phase-readiness escalations. The goal is to preserve auditability, meet retention standards, and avoid leaking sensitive material.

```yaml
observability:
  audit_logging:
    enabled: true
    required_events:
      - "test_execution_started"
      - "test_execution_completed"
      - "finding_triaged"
      - "escalation_created"
      - "policy_override_applied"
      - "retest_verified"
    storage:
      type: "local-or-object-storage"
      location: "repository-artifacts/audit/"
      encryption_required: true
    retention:
      audit_logs_days: 365
      evidence_files_days: 3650
    redaction_rules:
      - "remove_user_email"
      - "remove_auth_tokens"
      - "remove_personal_identifiers"
      - "retain_only_url_path_not_query_strings"
      - "mask_screenshot_file_names_when_user_data_present"
  escalation_requirements:
    required_fields:
      - "timestamp"
      - "affected_component"
      - "phase"
      - "root_cause"
      - "evidence_refs"
      - "owner"
      - "decision_deadline"
      - "policy_version"
    human_review_required: true
    notify:
      reviewers: ["accessibility-team", "qa-leads"]
      channels: ["issue", "project-board", "team-chat"]
```

Notes:
- Escalation log entries must include a policy version identifier so the reviewer can confirm which rules were in force.
- Retention and redaction rules should be enforced before evidence is archived or exported.
- URLs should be reduced to path-level evidence where possible; query strings and user-specific tokens should be stripped.

## Quick-Start Checklist and Minimal Decision Flow

The following checklist is the minimum operational sequence an agent should follow when evaluating accessibility QA state.

```yaml
quickstart:
  - id: "qs-1"
    step: "Confirm pinned tooling"
    check: "axe-core 4.12.1 and Playwright availability"
  - id: "qs-2"
    step: "Run automated axe-core scan"
    check: "artifacts captured and uploaded"
  - id: "qs-3"
    step: "Perform manual keyboard and focus tests"
    check: "screenshots, steps, and URLs recorded"
  - id: "qs-4"
    step: "Classify each finding"
    check: "applicability and impact assigned"
  - id: "qs-5"
    step: "Create or link defect records"
    check: "issue linked to evidence"
  - id: "qs-6"
    step: "Verify remediation"
    check: "retest confirms issue is resolved or remains blocked"
```

Minimal decision flowgraph:

```text
START
  |
  v
Run automated scan
  |
  v
Any findings?
  |-- No --> Report no blocking issues
  |
  v
Yes --> Classify applicability + severity
  |
  v
Applicable + high/critical?
  |-- Yes --> Create escalation + log event
  |
  v
No --> Create issue / track remediation
  |
  v
Re-test after fix
  |
  v
Pass? 
  |-- Yes --> Close / mark resolved
  |-- No --> Keep open and escalate if required
```

The flow should be implemented in automation as a deterministic sequence: run scan -> triage findings -> determine applicability -> escalate or open issue -> verify retest -> close or keep open.

## Versioning, Changelog, and Review Metadata

The agent should verify policy freshness before acting on the repo guidance. The following metadata provides the minimum trust check.

```yaml
versioning:
  current_version: "1.1"
  last_updated: "2026-10-12"
  phase_focus: "phase-1"
  changelog:
    - version: "1.1"
      date: "2026-10-12"
      changes:
        - "Added machine-readable YAML metadata block"
        - "Added templated prompt/response examples"
        - "Added observability and escalation logging requirements"
        - "Added quick-start and decision-flow metadata"
        - "Added changelog and review metadata block"
    - version: "1.0"
      date: "2026-10-11"
      changes:
        - "Initial Phase 1 guidance draft"
  review_metadata:
    required_approvals: 1
    reviewers:
      - name: "accessibility QA lead"
        status: "approved"
        date: "2026-10-11"
      - name: "repository maintainer"
        status: "approved"
        date: "2026-10-12"
  trust_policy:
    require_policy_version_check: true
    require_changelog_for_updates: true
    allow_override_only_by_human: true
```

If any of these fields are missing or stale, the agent should treat the policy as provisional until reviewed by a human maintainer.

## Repository Overview

**Purpose:** Establish a foundational, evidence-based accessibility QA strategy focused on automated axe-core checks, manual keyboard testing, and focus validation.

**Scope:** Phase 1 implementation of accessibility integration into QA workflows using:
- axe-core 4.12.1 (automated accessibility scanning)
- Playwright-based test execution
- Manual keyboard and focus interaction testing
- WCAG A/AA compliance validation
- Structured evidence capture and defect tracking

**License:** MIT

**Repository Status:** Foundational artifact; intended for QA adoption and phased rollout

---

## Agent Responsibilities

### 1. QA Process Management

**Primary Duties:**
- Track and organize accessibility test execution across defined phases
- Maintain the integrity of test plans and procedures in `Accessibility_QA_Axe_Core_Keyboard_Test_Plan.md`
- Ensure test evidence is documented with findings, classifications, and applicability markers
- Manage defect lifecycle: discovery, classification, severity assignment, remediation, and retest validation
- Coordinate manual keyboard and focus test results with automated axe-core scan outcomes

**Key Workflows:**
- **Test Execution Tracking:** Log when test phases begin, record completion status, and flag any blockers
- **Evidence Management:** Ensure all findings are captured with:
  - Rule identification (axe-core rule code)
  - Applicability status (applies/does not apply)
  - Impact classification (violation/needs review/best practice)
  - Supporting evidence (screenshots, URLs, steps to reproduce)
  - Retest results after remediation
- **Defect Coordination:** Link defects to specific test findings and track resolution status
- **Phase Handoff:** Facilitate transitions between Phase 1 and planned Phase 2+ expansions

### 2. Project Roadmap & Planning

**Primary Duties:**
- Maintain alignment between test plan updates and repository release cycles
- Track planned phases (Phase 2, Phase 3, etc.) as documented in the README
- Coordinate cross-team dependencies for accessibility testing adoption
- Support documentation updates that reflect process evolution
- Plan infrastructure and resource needs for expanded QA coverage

**Key Workflows:**
- **Phase Planning:** Help organize and track the following planned expansions:
  - Phase 2: Assistive technology validation (screen readers, voice control)
  - Phase 3: Semantic and content quality review
  - Phase 4: Responsive design and zoom/reflow validation
  - Phase 5: Accessibility regression reporting across releases
  - Phase 6: QA pipeline integration with engineering teams
- **Milestone Tracking:** Create and update milestones for phase transitions and key deliverables
- **Issue Organization:** Triage and label issues by:
  - Phase association (phase-1, phase-2, etc.)
  - Testing type (automated, manual-keyboard, manual-focus)
  - WCAG level (wcag-a, wcag-aa)
  - Status (in-progress, ready-for-test, resolved)

### 3. Documentation & Knowledge Management

**Primary Duties:**
- Maintain clarity and accuracy of the main test plan document
- Support creation of supplementary guides (execution playbooks, evidence templates, defect reporting standards)
- Track changes to test procedures and ensure versioning is clear
- Archive historical test runs for trend analysis and regression detection

**Key Workflows:**
- **Test Plan Updates:** Flag sections that need refinement based on real-world execution feedback
- **Procedure Clarity:** Ensure keyboard test procedures and focus validation steps remain unambiguous
- **Knowledge Base:** Support creation of FAQs, troubleshooting guides, and best practices from team learnings
- **Change Documentation:** Track rule set updates (pinned axe-core version, experimental rules)

### 4. Issue & Pull Request Management

**Primary Duties:**
- Triage incoming issues and categorize by priority, phase, and testing domain
- Review pull requests for:
  - Alignment with accessibility QA standards
  - Test plan consistency
  - Evidence quality and completeness
  - Documentation accuracy
- Coordinate PR reviews to ensure accessibility domain expertise is applied

**Key Workflows:**
- **Issue Triage:**
  - Accessibility defects: assign to relevant phase, link to test findings
  - Process improvements: evaluate impact on current and future phases
  - Documentation gaps: prioritize by team dependency
- **PR Review Checklist:**
  - Does the change align with Phase 1 scope or support future phases?
  - Are test procedures clear and reproducible?
  - Is evidence capture adequate for defect tracking?
  - Does documentation reflect current tooling (axe-core 4.12.1, Playwright version)?
  - Are examples and screenshots correctly sourced?

### 5. Quality Standards & Compliance

**Primary Duties:**
- Enforce consistent use of the defined test framework (axe-core + Playwright + manual procedures)
- Ensure all findings are classified according to the schema: violation/needs-review/best-practice
- Validate that WCAG A/AA rules are correctly mapped to test cases
- Support evidence audits to maintain defect credibility

**Key Workflows:**
- **Test Standard Enforcement:**
  - Automated scans must use pinned axe-core 4.12.1
  - Keyboard tests must follow documented procedures
  - Focus tests must validate all defined criteria (order, visibility, exit, modal, dynamic)
  - Findings must include applicability status
- **Rule Profile Management:**
  - Track active rules vs. experimental rule set
  - Monitor for rule deprecation or changes in axe-core updates
  - Support decision-making on rule inclusion/exclusion

### 6. Stakeholder Communication & Reporting

**Primary Duties:**
- Provide clear, data-driven reports on accessibility QA progress
- Communicate blockers, risks, and process improvements to stakeholders
- Facilitate feedback loops between QA teams and product engineering
- Support phase readiness assessments

**Key Workflows:**
- **Progress Reporting:**
  - Test execution rate and coverage
  - Defect discovery trends (by severity, by component)
  - Retest pass rates and remediation timelines
  - Phase readiness status
- **Risk Communication:**
  - Unresolved review findings (manual validations that require decision-making)
  - Gaps in tooling or team capability
  - Blockers to phase transitions
- **Adoption Support:**
  - Guide teams through first-time use of the test plan
  - Share lessons learned from early executions
  - Help customize procedures for specific application contexts

---

## Core Principles for Agent Decision-Making

1. **Evidence-First:** All accessibility findings must be grounded in repeatable test procedures and documented evidence. Avoid unsupported claims.

2. **Phased Maturity:** Phase 1 is intentionally focused and limited in scope. Don't over-extend beyond the defined boundary. Plan future phases but execute current phase rigorously.

3. **User-Centered:** Keyboard and focus tests are grounded in real user interaction patterns. Automated scans alone are insufficient. Maintain this dual-validation approach.

4. **Transparency:** Unresolved findings, blockers, and open questions should be explicitly tracked rather than silently dismissed.

5. **Consistency:** Use standardized terminology (applicability, impact classification, rule codes) across all documentation and defect reports.

6. **Ownership Clarity:** Accessibility QA is a shared responsibility. Ensure defect ownership is clear and escalation paths are defined.

---

## Common Tasks & Escalation

### Routine Tasks
- Triage new issues and assign to phases
- Update phase milestones with progress
- Review test evidence for completeness
- Track defect resolution status

### Escalation Triggers
- Unresolved blockers affecting phase progression
- Tooling gaps (missing Playwright features, axe-core limitation)
- Conflicting or ambiguous test procedures
- High-volume defects in a single component (potential systemic issue)
- Changes to axe-core version or rule set impact

### Decision Points Requiring Human Input
- Applicability determination for edge cases
- Severity rating for novel accessibility defects
- Rule inclusion/exclusion decisions
- Phase transition readiness assessment
- Architecture changes that affect testing strategy

---

## Repository File Guide

| File | Purpose | Agent Interaction |
|------|---------|-------------------|
| `Accessibility_QA_Axe_Core_Keyboard_Test_Plan.md` | Main test procedures, rule profiles, manual validation steps | Reference for test execution; propose updates based on feedback |
| `README.md` | Project overview, phase roadmap, usage guidance | Keep aligned with current phase status; update planned phase details |
| `LICENSE` | MIT licensing terms | Acknowledge in contributor guidance; reference in new documents |
| `agent.md` | This document | Update to reflect emerging processes and lessons learned |

---

## Integration Points for Agents

### With GitHub Issues
- Use labels: `phase-1`, `phase-2`, `automated`, `manual-keyboard`, `manual-focus`, `wcag-a`, `wcag-aa`, `defect`, `process-improvement`, `blocked`
- Link defect issues to test findings via comments and cross-references
- Track phase transitions via milestone completion

### With Pull Requests
- Request reviews from team members with accessibility QA expertise
- Include test evidence and procedure changes in PR descriptions
- Use checklists to ensure consistency across updates

### With Project Boards
- Create Phase 1, Phase 2, etc. as project columns
- Track test execution, defect lifecycle, and remediation progress
- Support roadmap transparency for stakeholders

### With Discussions (if enabled)
- Foster team Q&A on test procedures and edge cases
- Archive resolved discussions as knowledge base entries
- Use for brainstorming Phase 2+ expansion topics

---

## Version & Maintenance

**Document Version:** 1.1  
**Last Updated:** 2026-10-12  
**Phase Focus:** Phase 1 (axe-core + Keyboard + Focus)  
**Pinned Tooling:** axe-core 4.12.1, Playwright (latest compatible)

**Agent Maintenance Schedule:**
- Review monthly for alignment with actual team processes
- Update when new phases are activated
- Revise when tooling versions change
- Expand guidance based on documented decision patterns

---

## Contact & Feedback

For questions or updates to this agent guidance, please:
1. Open an issue with the `process-improvement` label
2. Include context on the scenario or decision point
3. Reference the relevant section of this document
4. Propose a specific change or clarification

---

**End of Agent Guidance Document**

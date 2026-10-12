# Agent Guidance for QA and Project Management

This document provides structured guidance for AI agents tasked with managing quality assurance and project management processes within the **Phased Approach to Integrating Accessibility into QA** repository.

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

**Document Version:** 1.0  
**Last Updated:** 2026-10-11  
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

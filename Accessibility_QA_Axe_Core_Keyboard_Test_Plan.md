# Accessibility QA plan for axe core and keyboard testing

Version 1.0 · Revised 28 September 2026 · Baseline engine axe-core 4.12.1

This plan defines a first-stage integration of accessibility testing into QA using axe-core through Playwright, supported by keyboard and focus assessment. It groups automated rules by topic, identifies what each group can establish, and connects selected findings to independent manual tests. It applies across organisations and products.

The baseline contains **90 distinct rules: 63 from the pinned engine's main WCAG A/AA sections and 27 best-practice rules**. A separate **seven-rule experimental profile** provides additional assistance. The manual catalogue contains **six keyboard tests and eleven focus tests**. Counts describe rules and test definitions, not coverage of all accessibility requirements.

WCAG 2.2 A and AA are the conformance reference. Best practices remain explicit recommendations. Experimental checks are separate from the baseline and require validation and human review. Passing this plan does not establish complete WCAG conformance or an inclusive experience for every user.

## Navigation

- [Assessment protocol](#assessment-protocol)
- [Rule profiles and execution](#rule-profiles-and-execution)
- [Automated axe core checks](#automated-axe-core-checks)
- [Experimental checks](#experimental-checks)
- [Manual keyboard checks](#manual-keyboard-checks)
- [Recording results](#recording-results)
- [Guidance and retained source links](#guidance-and-retained-source-links)

## Assessment protocol

1. Agree and record the product, build, journeys, pages, components, test accounts and data, states, browsers, operating systems, viewports and zoom settings. Name any excluded scope and its reason.
2. Define a function inventory before keyboard testing. Include pointer and hover functions, drag/reorder actions, scrolling, custom widgets, embedded content, media controls, validation, correction, cancellation and submission. A pointer-based discovery pass may identify functions; the keyboard assessment itself uses keyboard input only.
3. Record the installed axe-core, @axe-core/playwright and Playwright versions. Pin dependencies and retain the lockfile or equivalent build evidence. Verify the actual engine version in raw results against this plan; review rule changes before using a different version.
4. Use the version-controlled rule profiles below. Record enabled rules, options, include/exclude selectors, frame boundaries, shadow-DOM limitations and all scan errors. Keep baseline and experimental outputs distinguishable.
5. Establish each agreed state, wait for an observable readiness condition and then scan. Do not assume initial-page scanning covers menus, dialogs, errors, lazy content or route changes. Use full-page scans for page-level checks; supplement them with component scans.
6. Use real keyboard sequences for evidence of reachability and operation. Programmatic focus or pointer interaction may prepare an isolated automated state, but cannot prove keyboard reachability. Record how each state was reached.
7. Perform each applicable manual test for the defined component and state. Record browser/OS keyboard-navigation settings and any specialist assistance. Follow relevant platform and component conventions, distinguishing recommended patterns from the precise WCAG requirement.
8. Retain all raw axe collections and resolve review findings. Record an independent manual result; an axe pass never passes a keyboard or focus test.
9. Record untested scope as outstanding. Apply the statuses and aggregation rules in Recording results. Retest confirmed defects in the affected and relevant adjacent states.

## Rule profiles and execution

| Profile | Contents | Treatment |
| --- | --- | --- |
| Baseline WCAG | 63 rules in the main A/AA sections of axe-core 4.12.1 | Review applicability and findings. Each rule provides only its documented partial evidence. |
| Baseline best practices | All 27 best-practice rules in the pinned inventory | Run alongside WCAG checks, but report separately. A rule failure is not automatically a WCAG failure. |
| Experimental | Seven experimental rules | Run as a separate assessment profile; validate behaviour before using findings in a release gate. Confirm any underlying defect manually. |
| Excluded categories | AAA and deprecated rules | Not included in these profiles. Record any later additions as an explicit scope/version change. |

The appendix provides exact JSON rule-ID lists. Configure the Playwright integration using an explicit rule list, such as AxeBuilder.withRules(profile), and explicitly enable target-size in the baseline configuration. Verify the effective configuration and returned results; do not rely only on WCAG tags or engine defaults to reproduce these profiles. Include wcag22aa if using tags in any supplementary configuration, but retain the explicit lists as the authoritative scope.

Store configuration with the test code. Retain a profile version or hash with every run. Check that every selected ID is recognised by the installed engine; do not silently drop unknown rules. Distinguish a selected rule that finds no applicable nodes from a rule that did not run or encountered an error. Test the integration with representative known failures before the first operational run, including target-size and a review-only result.

An exclusion removes coverage and must have a recorded reason, owner and review date. Prefer tracking a specific known finding over broadly excluding a container and its descendants. Capture frame and shadow-root paths where relevant, and record content the engine cannot inspect.

## Automated axe core checks

Every group below uses the common evidence and result model. Retain run/build/state identifiers, rule ID and classification, target path, node HTML where useful, axe impact, failure summary, complete raw JSON, review decision, reviewer and evidence links. Record all applicable nodes and required states before assigning a complete group pass. Best-practice and WCAG results remain separate within mixed groups.



### AUTO-01 Structure and relationships

**Outcome:** Specified list structures and required ARIA parent and child relationships satisfy the mapped rules.

**Procedure:** Run the mapped rules on each page and component state. Review every violation and incomplete result in context.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [aria-required-children](https://dequeuniversity.com/rules/axe/4.12/aria-required-children?application=axeAPI) | WCAG A/AA partial check |
| [aria-required-parent](https://dequeuniversity.com/rules/axe/4.12/aria-required-parent?application=axeAPI) | WCAG A/AA partial check |
| [definition-list](https://dequeuniversity.com/rules/axe/4.12/definition-list?application=axeAPI) | WCAG A/AA partial check |
| [dlitem](https://dequeuniversity.com/rules/axe/4.12/dlitem?application=axeAPI) | WCAG A/AA partial check |
| [list](https://dequeuniversity.com/rules/axe/4.12/list?application=axeAPI) | WCAG A/AA partial check |
| [listitem](https://dequeuniversity.com/rules/axe/4.12/listitem?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 1.3.1 Info and Relationships](https://www.w3.org/TR/WCAG22/#info-and-relationships).

**Evidence and limits:** These checks do not identify every missing semantic relationship or establish that the chosen structure conveys the intended meaning.

**Manual cross-references:** No direct keyboard-test coverage.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.



### AUTO-02 Data table relationships

**Outcome:** Explicit table header references satisfy the mapped rules; headers have associated data cells, and selected header text and scope checks pass.

**Procedure:** Scan populated and responsive tables. Review explicit headers references, header-to-data associations, empty header text, scope usage and duplicated caption/summary text. Do not infer complete header coverage from a clean scan.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [td-headers-attr](https://dequeuniversity.com/rules/axe/4.12/td-headers-attr?application=axeAPI) | WCAG A/AA partial check |
| [th-has-data-cells](https://dequeuniversity.com/rules/axe/4.12/th-has-data-cells?application=axeAPI) | WCAG A/AA partial check |
| [empty-table-header](https://dequeuniversity.com/rules/axe/4.12/empty-table-header?application=axeAPI) | Best practice |
| [scope-attr-valid](https://dequeuniversity.com/rules/axe/4.12/scope-attr-valid?application=axeAPI) | Best practice |
| [table-duplicate-name](https://dequeuniversity.com/rules/axe/4.12/table-duplicate-name?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 1.3.1 Info and Relationships](https://www.w3.org/TR/WCAG22/#info-and-relationships).

**Evidence and limits:** The checks do not establish that every data cell has a correct header, that header text describes its data, or that a table should have been used. Experimental td-has-header is also limited in applicability.

**Manual cross-references:** Keyboard-T01 where tables contain interactive controls; semantic table checks do not establish keyboard behaviour.

**Additional experimental assistance:** [table-fake-caption](https://dequeuniversity.com/rules/axe/4.12/table-fake-caption?application=axeAPI), [td-has-header](https://dequeuniversity.com/rules/axe/4.12/td-has-header?application=axeAPI). See the separate experimental profile; do not combine its result with the baseline.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.



### AUTO-03 Headings

**Outcome:** The page satisfies the selected heading presence, non-empty heading and heading-order best practices.

**Procedure:** Scan each page and route after dynamic content is loaded.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [heading-order](https://dequeuniversity.com/rules/axe/4.12/heading-order?application=axeAPI) | Best practice |
| [page-has-heading-one](https://dequeuniversity.com/rules/axe/4.12/page-has-heading-one?application=axeAPI) | Best practice |
| [empty-heading](https://dequeuniversity.com/rules/axe/4.12/empty-heading?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 1.3.1 Info and Relationships](https://www.w3.org/TR/WCAG22/#info-and-relationships); [WCAG 2.4.6 Headings and Labels](https://www.w3.org/TR/WCAG22/#headings-and-labels).

**Evidence and limits:** These are best-practice checks, not standalone tests of WCAG 1.3.1 or 2.4.6. Human review must assess meaningful hierarchy and descriptive headings.

**Manual cross-references:** No direct keyboard-test coverage.

**Additional experimental assistance:** [p-as-heading](https://dequeuniversity.com/rules/axe/4.12/p-as-heading?application=axeAPI). See the separate experimental profile; do not combine its result with the baseline.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.



### AUTO-04 Landmarks and bypass

**Outcome:** The page satisfies the selected landmark best practices and rule-detectable bypass checks; actual bypass operation is assessed separately.

**Procedure:** Scan the whole top-level document and every in-scope frame. Review landmark findings by document boundary. Complete Keyboard-T06 for repeated blocks, whether or not axe reports a skip link.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [bypass](https://dequeuniversity.com/rules/axe/4.12/bypass?application=axeAPI) | WCAG A/AA partial check |
| [skip-link](https://dequeuniversity.com/rules/axe/4.12/skip-link?application=axeAPI) | Best practice |
| [landmark-banner-is-top-level](https://dequeuniversity.com/rules/axe/4.12/landmark-banner-is-top-level?application=axeAPI) | Best practice |
| [landmark-contentinfo-is-top-level](https://dequeuniversity.com/rules/axe/4.12/landmark-contentinfo-is-top-level?application=axeAPI) | Best practice |
| [landmark-no-duplicate-main](https://dequeuniversity.com/rules/axe/4.12/landmark-no-duplicate-main?application=axeAPI) | Best practice |
| [landmark-one-main](https://dequeuniversity.com/rules/axe/4.12/landmark-one-main?application=axeAPI) | Best practice |
| [region](https://dequeuniversity.com/rules/axe/4.12/region?application=axeAPI) | Best practice |
| [landmark-main-is-top-level](https://dequeuniversity.com/rules/axe/4.12/landmark-main-is-top-level?application=axeAPI) | Best practice |
| [landmark-no-duplicate-banner](https://dequeuniversity.com/rules/axe/4.12/landmark-no-duplicate-banner?application=axeAPI) | Best practice |
| [landmark-no-duplicate-contentinfo](https://dequeuniversity.com/rules/axe/4.12/landmark-no-duplicate-contentinfo?application=axeAPI) | Best practice |
| [landmark-unique](https://dequeuniversity.com/rules/axe/4.12/landmark-unique?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 2.4.1 Bypass Blocks](https://www.w3.org/TR/WCAG22/#bypass-blocks).

**Evidence and limits:** Landmarks and structural bypass indicators do not establish usable keyboard bypass. Correct naming and meaning still need review; duplicate landmarks require interpretation in their document context.

**Manual cross-references:** Keyboard-T06; Focus-T07 for initially hidden bypass controls; Focus-T01 for the resulting navigation position.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.



### AUTO-05 Link names

**Outcome:** Selected links and image-map areas have a discernible accessible name.

**Procedure:** Scan every route and state containing links. Review empty, hidden and image-only link names.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [area-alt](https://dequeuniversity.com/rules/axe/4.12/area-alt?application=axeAPI) | WCAG A/AA partial check |
| [link-name](https://dequeuniversity.com/rules/axe/4.12/link-name?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 2.4.4 Link Purpose in Context](https://www.w3.org/TR/WCAG22/#link-purpose-in-context); [WCAG 4.1.2 Name Role Value](https://www.w3.org/TR/WCAG22/#name-role-value).

**Evidence and limits:** Name presence does not establish whether the link purpose is clear from its text and programmatically determined context. Meaning in isolation is a separate recommended best practice, not the A/AA baseline.

**Manual cross-references:** Keyboard-T01 for activation; naming does not prove operation.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-06 Page titles

**Outcome:** Each scanned document has a non-empty title.

**Procedure:** Scan every distinct page, route and state that changes page purpose. Wait for the expected title update and record the resulting title.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [document-title](https://dequeuniversity.com/rules/axe/4.12/document-title?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 2.4.2 Page Titled](https://www.w3.org/TR/WCAG22/#page-titled).

**Evidence and limits:** A human must confirm that the title identifies the page and is sufficiently distinctive.

**Manual cross-references:** Focus-T11 for route-change continuity; title presence does not prove focus management.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-07 Language metadata

**Outcome:** Page and passage language metadata uses rule-valid values.

**Procedure:** Scan each language version and representative passages containing a language change.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [html-has-lang](https://dequeuniversity.com/rules/axe/4.12/html-has-lang?application=axeAPI) | WCAG A/AA partial check |
| [html-lang-valid](https://dequeuniversity.com/rules/axe/4.12/html-lang-valid?application=axeAPI) | WCAG A/AA partial check |
| [html-xml-lang-mismatch](https://dequeuniversity.com/rules/axe/4.12/html-xml-lang-mismatch?application=axeAPI) | WCAG A/AA partial check |
| [valid-lang](https://dequeuniversity.com/rules/axe/4.12/valid-lang?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 3.1.1 Language of Page](https://www.w3.org/TR/WCAG22/#language-of-page); [WCAG 3.1.2 Language of Parts](https://www.w3.org/TR/WCAG22/#language-of-parts).

**Evidence and limits:** A fluent reviewer must compare declared language with the actual content and identify missing language changes.

**Manual cross-references:** No direct keyboard-test coverage.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-08 Component names and ARIA

**Outcome:** Supported controls satisfy the selected accessible-name, ARIA syntax, role and relationship checks.

**Procedure:** Scan native and custom controls in applicable default, focused, expanded, selected, checked, disabled and error states. Include dialogs, trees and presentational-role conflicts. Review actual names, roles and attributes for flagged nodes; repeat after state changes.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [aria-allowed-attr](https://dequeuniversity.com/rules/axe/4.12/aria-allowed-attr?application=axeAPI) | WCAG A/AA partial check |
| [aria-braille-equivalent](https://dequeuniversity.com/rules/axe/4.12/aria-braille-equivalent?application=axeAPI) | WCAG A/AA partial check |
| [aria-command-name](https://dequeuniversity.com/rules/axe/4.12/aria-command-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-conditional-attr](https://dequeuniversity.com/rules/axe/4.12/aria-conditional-attr?application=axeAPI) | WCAG A/AA partial check |
| [aria-deprecated-role](https://dequeuniversity.com/rules/axe/4.12/aria-deprecated-role?application=axeAPI) | WCAG A/AA partial check |
| [aria-hidden-body](https://dequeuniversity.com/rules/axe/4.12/aria-hidden-body?application=axeAPI) | WCAG A/AA partial check |
| [aria-hidden-focus](https://dequeuniversity.com/rules/axe/4.12/aria-hidden-focus?application=axeAPI) | WCAG A/AA partial check |
| [aria-input-field-name](https://dequeuniversity.com/rules/axe/4.12/aria-input-field-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-prohibited-attr](https://dequeuniversity.com/rules/axe/4.12/aria-prohibited-attr?application=axeAPI) | WCAG A/AA partial check |
| [aria-required-attr](https://dequeuniversity.com/rules/axe/4.12/aria-required-attr?application=axeAPI) | WCAG A/AA partial check |
| [aria-roles](https://dequeuniversity.com/rules/axe/4.12/aria-roles?application=axeAPI) | WCAG A/AA partial check |
| [aria-tab-name](https://dequeuniversity.com/rules/axe/4.12/aria-tab-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-toggle-field-name](https://dequeuniversity.com/rules/axe/4.12/aria-toggle-field-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-tooltip-name](https://dequeuniversity.com/rules/axe/4.12/aria-tooltip-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-valid-attr](https://dequeuniversity.com/rules/axe/4.12/aria-valid-attr?application=axeAPI) | WCAG A/AA partial check |
| [aria-valid-attr-value](https://dequeuniversity.com/rules/axe/4.12/aria-valid-attr-value?application=axeAPI) | WCAG A/AA partial check |
| [button-name](https://dequeuniversity.com/rules/axe/4.12/button-name?application=axeAPI) | WCAG A/AA partial check |
| [duplicate-id-aria](https://dequeuniversity.com/rules/axe/4.12/duplicate-id-aria?application=axeAPI) | WCAG A/AA partial check |
| [input-button-name](https://dequeuniversity.com/rules/axe/4.12/input-button-name?application=axeAPI) | WCAG A/AA partial check |
| [label](https://dequeuniversity.com/rules/axe/4.12/label?application=axeAPI) | WCAG A/AA partial check |
| [nested-interactive](https://dequeuniversity.com/rules/axe/4.12/nested-interactive?application=axeAPI) | WCAG A/AA partial check |
| [select-name](https://dequeuniversity.com/rules/axe/4.12/select-name?application=axeAPI) | WCAG A/AA partial check |
| [summary-name](https://dequeuniversity.com/rules/axe/4.12/summary-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-allowed-role](https://dequeuniversity.com/rules/axe/4.12/aria-allowed-role?application=axeAPI) | Best practice |
| [aria-dialog-name](https://dequeuniversity.com/rules/axe/4.12/aria-dialog-name?application=axeAPI) | Best practice |
| [aria-text](https://dequeuniversity.com/rules/axe/4.12/aria-text?application=axeAPI) | Best practice |
| [aria-treeitem-name](https://dequeuniversity.com/rules/axe/4.12/aria-treeitem-name?application=axeAPI) | Best practice |
| [presentation-role-conflict](https://dequeuniversity.com/rules/axe/4.12/presentation-role-conflict?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 4.1.2 Name Role Value](https://www.w3.org/TR/WCAG22/#name-role-value); [WCAG 2.5.3 Label in Name](https://www.w3.org/TR/WCAG22/#label-in-name).

**Evidence and limits:** Syntax, naming and static relationships do not prove correct purpose, state, behaviour or assistive-technology operation. The baseline rules do not test visible-label inclusion under 2.5.3; experimental assistance remains partial and requires a separate outcome.

**Manual cross-references:** Keyboard-T01, Keyboard-T03, Focus-T01, Focus-T07 and Focus-T10. aria-hidden-focus and nested-interactive indicate selected risks; they do not pass these tests.

**Additional experimental assistance:** [label-content-name-mismatch](https://dequeuniversity.com/rules/axe/4.12/label-content-name-mismatch?application=axeAPI). See the separate experimental profile; do not combine its result with the baseline.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-09 Frames and embedded content

**Outcome:** Frames have rule-valid names; frame scan coverage is recorded separately and any untested frame remains outstanding.

**Procedure:** Run the selected frame checks and ensure frame scanning is enabled in the integration. Record each frame URL or identifier, scan status and reason for any gap. Scan inaccessible frames separately where possible. Run AUTO-06 within embedded documents for document titles.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [frame-tested](https://dequeuniversity.com/rules/axe/4.12/frame-tested?application=axeAPI) | Best practice |
| [frame-title](https://dequeuniversity.com/rules/axe/4.12/frame-title?application=axeAPI) | WCAG A/AA partial check |
| [frame-title-unique](https://dequeuniversity.com/rules/axe/4.12/frame-title-unique?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 2.4.2 Page Titled](https://www.w3.org/TR/WCAG22/#page-titled); [WCAG 4.1.2 Name Role Value](https://www.w3.org/TR/WCAG22/#name-role-value).

**Evidence and limits:** Frame naming, engine injection and successful scan completion are separate facts. None proves embedded content usability. Missing frame coverage prevents a complete coverage claim; do not record inaccessible content as not applicable.

**Manual cross-references:** Keyboard-T01, Keyboard-T03 and Keyboard-T04 for embedded interaction.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-10 Forms and input purpose

**Outcome:** Supported form fields satisfy the selected autocomplete and label-pattern checks, alongside the basic naming checks in AUTO-08.

**Procedure:** Scan every form in empty, completed, validation-error and submitted states. Include AUTO-08 label and control-name checks. Review multiple-label findings in context; do not automatically remove additional labels. Verify autocomplete values and record separate human checks of purpose and instructions.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [autocomplete-valid](https://dequeuniversity.com/rules/axe/4.12/autocomplete-valid?application=axeAPI) | WCAG A/AA partial check |
| [form-field-multiple-labels](https://dequeuniversity.com/rules/axe/4.12/form-field-multiple-labels?application=axeAPI) | WCAG A/AA partial check |
| [label-title-only](https://dequeuniversity.com/rules/axe/4.12/label-title-only?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 1.3.5 Identify Input Purpose](https://www.w3.org/TR/WCAG22/#identify-input-purpose); [WCAG 3.3.2 Labels or Instructions](https://www.w3.org/TR/WCAG22/#labels-or-instructions).

**Evidence and limits:** Basic name checks are shared with AUTO-08. Automation cannot confirm that visible labels persist, instructions are sufficient or autocomplete accurately represents the field purpose. Multiple-label findings require contextual review.

**Manual cross-references:** Keyboard-T01, Focus-T05 and Focus-T11 for form operation and error paths.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-11 Images and graphical objects

**Outcome:** Supported non-text elements provide the alternative-text or naming mechanism required by the mapped rules, allowing appropriate empty alternatives for decorative images.

**Procedure:** Scan every content state containing non-text content, including loading and error states.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [aria-meter-name](https://dequeuniversity.com/rules/axe/4.12/aria-meter-name?application=axeAPI) | WCAG A/AA partial check |
| [aria-progressbar-name](https://dequeuniversity.com/rules/axe/4.12/aria-progressbar-name?application=axeAPI) | WCAG A/AA partial check |
| [image-alt](https://dequeuniversity.com/rules/axe/4.12/image-alt?application=axeAPI) | WCAG A/AA partial check |
| [image-redundant-alt](https://dequeuniversity.com/rules/axe/4.12/image-redundant-alt?application=axeAPI) | Best practice |
| [input-image-alt](https://dequeuniversity.com/rules/axe/4.12/input-image-alt?application=axeAPI) | WCAG A/AA partial check |
| [object-alt](https://dequeuniversity.com/rules/axe/4.12/object-alt?application=axeAPI) | WCAG A/AA partial check |
| [role-img-alt](https://dequeuniversity.com/rules/axe/4.12/role-img-alt?application=axeAPI) | WCAG A/AA partial check |
| [svg-img-alt](https://dequeuniversity.com/rules/axe/4.12/svg-img-alt?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 1.1.1 Non-text Content](https://www.w3.org/TR/WCAG22/#non-text-content).

**Evidence and limits:** A human must distinguish decorative and informative content and judge alternatives. Meter and progressbar name checks do not establish correct values, updates or announcements. An appropriate empty alternative is not a failure.

**Manual cross-references:** Keyboard-T01 where the element is interactive; non-text naming does not prove keyboard access.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-12 Colour contrast and link distinction

**Outcome:** Computable text contrast and selected link-in-text distinctions meet the rule thresholds.

**Procedure:** Establish each applicable default, hover, keyboard-focus, selected, disabled and error state before scanning, with actual backgrounds loaded. Preserve evidence of the state. Apply relevant inactive-control exceptions. Complete Focus-T08 for applicable focus-indicator contrast.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [color-contrast](https://dequeuniversity.com/rules/axe/4.12/color-contrast?application=axeAPI) | WCAG A/AA partial check |
| [link-in-text-block](https://dequeuniversity.com/rules/axe/4.12/link-in-text-block?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 1.4.1 Use of Color](https://www.w3.org/TR/WCAG22/#use-of-color); [WCAG 1.4.3 Contrast Minimum](https://www.w3.org/TR/WCAG22/#contrast-minimum).

**Evidence and limits:** Manually assess uncomputed contrast, gradients, background images and obscured text. These rules do not establish non-text or focus-indicator contrast, complete use-of-colour coverage or all state combinations.

**Manual cross-references:** Focus-T02 and Focus-T08; text contrast is not focus-indicator contrast.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-13 Text spacing and zoom restrictions

**Outcome:** No rule-detectable inline text-spacing or viewport zoom restrictions are found; the additional viewport scaling best practice is assessed separately.

**Procedure:** Scan each responsive layout for inline text-spacing and viewport metadata restrictions. Record viewport metadata, affected inline styles and scaling findings. Keep full resize, reflow and text-spacing override assessments in the outstanding wider-assessment register.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [avoid-inline-spacing](https://dequeuniversity.com/rules/axe/4.12/avoid-inline-spacing?application=axeAPI) | WCAG A/AA partial check |
| [meta-viewport](https://dequeuniversity.com/rules/axe/4.12/meta-viewport?application=axeAPI) | WCAG A/AA partial check |
| [meta-viewport-large](https://dequeuniversity.com/rules/axe/4.12/meta-viewport-large?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 1.4.4 Resize Text](https://www.w3.org/TR/WCAG22/#resize-text); [WCAG 1.4.10 Reflow](https://www.w3.org/TR/WCAG22/#reflow); [WCAG 1.4.12 Text Spacing](https://www.w3.org/TR/WCAG22/#text-spacing).

**Evidence and limits:** These rules do not perform full resize, zoom, reflow or text-spacing override assessments. WCAG 1.4.10 is related guidance, not coverage established by these rules. Experimental orientation checks do not establish complete orientation support.

**Manual cross-references:** Focus-T02, Focus-T03 and Focus-T11 in the agreed zoom and responsive states; automation does not pass these tests.

**Additional experimental assistance:** [css-orientation-lock](https://dequeuniversity.com/rules/axe/4.12/css-orientation-lock?application=axeAPI). See the separate experimental profile; do not combine its result with the baseline.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-14 Pointer target size

**Outcome:** Applicable targets satisfy the size or spacing conditions detected by the rule, with reviewed exception decisions.

**Procedure:** Explicitly enable target-size in the version-controlled profile. Scan relevant responsive sizes and open states. Retain configuration and execution evidence, measured targets and spacing, and reviewed exception decisions. A configured rule with no applicable targets is not evidence that all target types were checked.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [target-size](https://dequeuniversity.com/rules/axe/4.12/target-size?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 2.5.8 Target Size Minimum](https://www.w3.org/TR/WCAG22/#target-size-minimum).

**Evidence and limits:** target-size is disabled by default in the pinned 4.12.1 source. Confirm activation. Review rule applicability, size/spacing calculations and each claimed exception; the scan does not establish complete 2.5.8 coverage.

**Manual cross-references:** No direct keyboard-test coverage. Pointer size is distinct from keyboard operation.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-15 Audio and video markers

**Outcome:** Supported media satisfies the rule-detectable prerecorded caption-track and autoplay checks, with review findings resolved.

**Procedure:** Scan media elements and reachable third-party players in applicable states. Review caption-track findings and observe audible autoplay duration and available stop, pause or independent volume controls. Record prerecorded versus live content; assess live captions separately.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [no-autoplay-audio](https://dequeuniversity.com/rules/axe/4.12/no-autoplay-audio?application=axeAPI) | WCAG A/AA partial check |
| [video-caption](https://dequeuniversity.com/rules/axe/4.12/video-caption?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 1.2.2 Captions Prerecorded](https://www.w3.org/TR/WCAG22/#captions-prerecorded); [WCAG 1.2.4 Captions Live](https://www.w3.org/TR/WCAG22/#captions-live); [WCAG 1.4.2 Audio Control](https://www.w3.org/TR/WCAG22/#audio-control).

**Evidence and limits:** Caption-track checks do not establish accuracy, synchronisation or live captions. Autoplay findings need review against actual sound, duration and control behaviour. WCAG 1.2.4 is separate related guidance, not an automated result here.

**Manual cross-references:** Keyboard-T01 for media controls; AUTO-15 does not establish their operation.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-16 Moving and timed content

**Outcome:** No prohibited legacy movement or meta-refresh patterns are found under the mapped rules.

**Procedure:** Scan states with automatic movement, updating or refresh behaviour. Review matched legacy elements and meta-refresh values against the rule. Separately record untested scripted or CSS motion, time limits and flashing.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [blink](https://dequeuniversity.com/rules/axe/4.12/blink?application=axeAPI) | WCAG A/AA partial check |
| [marquee](https://dequeuniversity.com/rules/axe/4.12/marquee?application=axeAPI) | WCAG A/AA partial check |
| [meta-refresh](https://dequeuniversity.com/rules/axe/4.12/meta-refresh?application=axeAPI) | WCAG A/AA partial check |

**WCAG and related guidance:** 
[WCAG 2.2.1 Timing Adjustable](https://www.w3.org/TR/WCAG22/#timing-adjustable); [WCAG 2.2.2 Pause Stop Hide](https://www.w3.org/TR/WCAG22/#pause-stop-hide); [WCAG 2.3.1 Three Flashes or Below Threshold](https://www.w3.org/TR/WCAG22/#three-flashes-or-below-threshold).

**Evidence and limits:** These checks do not assess all CSS/scripted motion, adjustable timing, essentiality or flash thresholds. WCAG 2.3.1 is related guidance only and requires a separate assessment.

**Manual cross-references:** Keyboard-T01 for available timing or movement controls; the existence and adequacy of those controls require wider assessment.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


### AUTO-17 Keyboard risk patterns

**Outcome:** No failures are found in the selected frame, scroll-region and image-map keyboard risk checks; keyboard best-practice findings are assessed separately.

**Procedure:** Scan frames, scrollable regions, image maps, access keys and tabindex patterns. Review findings, then complete Keyboard-T01 to Keyboard-T04 and Focus-T01 where applicable. A positive tabindex is a best-practice finding; assess the actual order before assigning a WCAG failure.

**Baseline rules:**

| Rule and guidance | Classification |
| --- | --- |
| [frame-focusable-content](https://dequeuniversity.com/rules/axe/4.12/frame-focusable-content?application=axeAPI) | WCAG A/AA partial check |
| [scrollable-region-focusable](https://dequeuniversity.com/rules/axe/4.12/scrollable-region-focusable?application=axeAPI) | WCAG A/AA partial check |
| [server-side-image-map](https://dequeuniversity.com/rules/axe/4.12/server-side-image-map?application=axeAPI) | WCAG A/AA partial check |
| [accesskeys](https://dequeuniversity.com/rules/axe/4.12/accesskeys?application=axeAPI) | Best practice |
| [tabindex](https://dequeuniversity.com/rules/axe/4.12/tabindex?application=axeAPI) | Best practice |

**WCAG and related guidance:** 
[WCAG 2.1.1 Keyboard](https://www.w3.org/TR/WCAG22/#keyboard).

**Evidence and limits:** Automation detects selected risks and best-practice patterns. It does not demonstrate keyboard operability, logical focus order or absence of traps. Experimental hidden-content findings prompt state exploration; they are not automatic failures.

**Manual cross-references:** Keyboard-T01 to Keyboard-T04 and Focus-T01. No automated pass transfers to these manual tests.

**Additional experimental assistance:** [focus-order-semantics](https://dequeuniversity.com/rules/axe/4.12/focus-order-semantics?application=axeAPI), [hidden-content](https://dequeuniversity.com/rules/axe/4.12/hidden-content?application=axeAPI). See the separate experimental profile; do not combine its result with the baseline.

**Result:** Record per rule, node and state using the common result model, then summarise WCAG and best-practice results separately.


## Experimental checks

Run these seven rules separately. Their inclusion is deliberate, but their output is assistance rather than complete coverage. A confirmed WCAG defect remains a defect even when first identified by an experimental rule; document the human confirmation and applicable criterion. Do not automatically fail a release solely because an unreviewed experimental finding exists.

| Rule and guidance | Topic | Procedure and limit |
| --- | --- | --- |

| [css-orientation-lock](https://dequeuniversity.com/rules/axe/4.12/css-orientation-lock?application=axeAPI) | AUTO-13 | Scan relevant styles and layouts, then rotate the supported viewport/device and assess operation in both orientations. Partial assistance for 1.3.4; review essential-orientation exceptions. |
| [focus-order-semantics](https://dequeuniversity.com/rules/axe/4.12/focus-order-semantics?application=axeAPI) | AUTO-17 | Inspect focusable elements flagged for unsuitable roles. Compare their intended function with actual operation. Best-practice assistance, not a complete focus-order assessment. |
| [hidden-content](https://dequeuniversity.com/rules/axe/4.12/hidden-content?application=axeAPI) | AUTO-17 | Use findings to identify hidden states that may require opening and scanning. Hidden content is not inherently a failure. Connect applicable states to Focus-T07 and Focus-T09. |
| [label-content-name-mismatch](https://dequeuniversity.com/rules/axe/4.12/label-content-name-mismatch?application=axeAPI) | AUTO-08 | Compare the visible text label with the accessible name for each relevant control. Partial assistance for 2.5.3; review controls and naming mechanisms outside the rule coverage. |
| [p-as-heading](https://dequeuniversity.com/rules/axe/4.12/p-as-heading?application=axeAPI) | AUTO-03 | Review visually styled paragraphs and decide whether they function as headings. Partial assistance for semantic structure under 1.3.1. |
| [table-fake-caption](https://dequeuniversity.com/rules/axe/4.12/table-fake-caption?application=axeAPI) | AUTO-02 | Review apparent captions and their programmatic association. Partial assistance for 1.3.1, requiring contextual interpretation. |
| [td-has-header](https://dequeuniversity.com/rules/axe/4.12/td-has-header?application=axeAPI) | AUTO-02 | Check applicable non-empty data cells in tables covered by this rule, then verify correct header association manually. Its size/applicability limits mean a clean scan is not complete table coverage. |

Experimental interpretation guidance: [WCAG 1.3.4 Orientation](https://www.w3.org/WAI/WCAG22/Understanding/orientation.html), [WCAG 2.5.3 Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html) and [WCAG 1.3.1 Info and Relationships](https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html).

## Manual keyboard checks

Use keyboard-only browser interaction for the assessed journey. Cover the agreed initial, focused, expanded, selected, error and submitted states, responsive layouts and temporary content. Obtain specialist support for unfamiliar composite controls. Document the expected key model before testing it. Standard component patterns support predictable use but must not be confused with the narrower normative wording of a criterion.

Each test below requires expected and actual results, exact key sequences, component/state identifiers and evidence. Screenshots can show visible focus but do not alone prove keyboard reachability or operation. Use a focus trace, recording or repeatable interaction log where relevant. Results use the common six-status model.


### Keyboard-T01 Keyboard operation

**Method:** Assisted manual. Human judgement determines the result.

**Outcome:** Users can complete every non-exempt function through a keyboard interface.

**Applies to:** All functionality, including embedded components and complete error and correction paths.

**Procedure:** Put the pointer aside. Use Tab and Shift+Tab between controls, arrows within appropriate composite controls, and Enter or Space only where appropriate. Complete data entry, correction, cancellation and submission. Record any valid path-dependent exception. Include drag/reorder alternatives, hover-revealed functionality, scroll regions, media controls, custom widgets and embedded content. A path-dependent exception concerns the underlying function, not the chosen input technique; dragging to an endpoint is not by itself exempt. Record any alternative keyboard workflow and its discoverability.

**Expected result:** Every non-exempt function produces the same intended result without pointer or touchscreen input.

**Mapping and guidance:** [WCAG 2.1.1 Keyboard](https://www.w3.org/TR/WCAG22/#keyboard). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding keyboard](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html).

**Automation relationship:** AUTO-17 provides selected keyboard-risk evidence; AUTO-08 provides related semantics and naming risks. Neither passes this test.

**Evidence:** Function inventory, exact key sequence, expected and actual result, state transitions and exception evidence. Record the common status and any outstanding scope.


### Keyboard-T02 Keystroke timing

**Method:** Manual. Human judgement determines the result.

**Outcome:** Keyboard operation does not depend on precise timing between individual keystrokes.

**Applies to:** Every keyboard-operated function.

**Procedure:** Repeat each function while varying the delay between individual keystrokes. Inspect instructions and behaviour for any demand to press a key within a particular interval; distinguish separate content time limits. Also vary key-hold duration and rapid repeated presses. Check whether a long hold or fast sequence is required. Document the sampled timings and distinguish these restrictions from content/session time limits.

**Expected result:** The function remains operable without requiring a particular speed of individual keystrokes.

**Mapping and guidance:** [WCAG 2.1.1 Keyboard](https://www.w3.org/TR/WCAG22/#keyboard). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding keyboard](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html).

**Automation relationship:** No direct axe test.

**Evidence:** Functions tried, varied key timing, observed result and any separate time-limit reference. Record the common status and any outstanding scope.


### Keyboard-T03 Leaving components

**Method:** Manual. Human judgement determines the result.

**Outcome:** Users can leave every component using the keyboard.

**Applies to:** Every component that can receive keyboard focus, including dialogs, embedded content and composite widgets.

**Procedure:** Move focus into the component, operate it normally, then use its documented standard exit. Confirm focus can leave using keyboard input only. A modal may contain focus while open if a working keyboard close route is available. For modal dialogs, also complete Focus-T10. Assess forward and reverse traversal, a working close route and focus after closing. Expected containment inside an open modal is not itself a keyboard trap.

**Expected result:** Focus can be moved away from the component using only a keyboard interface.

**Mapping and guidance:** [WCAG 2.1.2 No Keyboard Trap](https://www.w3.org/TR/WCAG22/#no-keyboard-trap). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding no keyboard trap](https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html).

**Automation relationship:** AUTO-17 and AUTO-08 can identify related risks only; no axe rule establishes absence of traps.

**Evidence:** Entry and exit key sequence, focus destinations, screen recording and close route. Record the common status and any outstanding scope.


### Keyboard-T04 Non-standard exit instructions

**Method:** Manual. Human judgement determines the result.

**Outcome:** Users are told how to leave a component when it needs a non-standard keyboard command.

**Applies to:** Components that cannot be exited with unmodified Tab, arrow keys or another standard method.

**Procedure:** Locate the instructions before relying on the unusual exit. Follow them exactly and confirm that they describe a working keyboard method. Confirm instructions are discoverable and accessible to the affected keyboard user before entry or at the point of need. Record their location and availability.

**Expected result:** The non-standard exit method is explained to the user and works as described.

**Mapping and guidance:** [WCAG 2.1.2 No Keyboard Trap](https://www.w3.org/TR/WCAG22/#no-keyboard-trap). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding no keyboard trap](https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html).

**Automation relationship:** No direct axe test.

**Evidence:** Instruction text, where and when it is available, keys used and resulting focus destination. Record the common status and any outstanding scope.


### Keyboard-T05 Character-only shortcuts

**Method:** Manual. Human judgement determines the result.

**Outcome:** Users avoid accidental activation of character-only shortcuts.

**Applies to:** Shortcuts made only from letters, punctuation, numbers or symbols.

**Procedure:** For each shortcut, identify the safeguard: turn it off, remap it to include a non-printable key, or restrict it to component focus. Demonstrate the selected safeguard and check ordinary text entry for unintended activation. Test with the relevant component focused and unfocused, including text entry in unrelated fields. After changing a shortcut setting, verify that it takes effect and record its scope.

**Expected result:** Each applicable shortcut can be disabled, remapped with a non-printable key, or activated only while its component has focus.

**Mapping and guidance:** [WCAG 2.1.4 Character Key Shortcuts](https://www.w3.org/TR/WCAG22/#character-key-shortcuts). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding character key shortcuts](https://www.w3.org/WAI/WCAG22/Understanding/character-key-shortcuts.html).

**Automation relationship:** accesskeys in AUTO-17 addresses a different risk and does not establish 2.1.4 compliance.

**Evidence:** Shortcut inventory, selected safeguard, settings and activation or non-activation recording. Record the common status and any outstanding scope.


### Keyboard-T06 Bypass repeated blocks

**Method:** Assisted manual; scripted interactions may support repeatability, with human review of meaning and evidence.

**Outcome:** Users can bypass repeated content and continue at the intended destination.

**Applies to:** Pages with blocks repeated across a set of pages.

**Procedure:** Identify repeated blocks and the mechanism relied on to bypass them. Starting at the page entry point, reach and operate the mechanism using the keyboard. Check the destination and the next forward and reverse navigation steps. If another accessibility-supported mechanism is relied on, record and demonstrate it; do not require one particular skip-link implementation.

**Expected result:** The repeated blocks can be bypassed and subsequent navigation continues meaningfully. Missing axe findings are not evidence that a mechanism exists.

**Mapping and guidance:** [WCAG 2.4.1 bypass blocks](https://www.w3.org/TR/WCAG22/#bypass-blocks); [Understanding guidance](https://www.w3.org/WAI/WCAG22/Understanding/bypass-blocks.html). Coverage is limited to the stated conditions.

**Automation and related tests:** AUTO-04 bypass and skip-link offer partial assistance; Focus-T07 covers hidden skip controls.

**Evidence:** Repeated blocks, mechanism, key sequence, destination, next focus stop and any relied-on user-agent or assistive-technology configuration. Record the common status and outstanding scope.


### Focus-T01 Logical focus order

**Method:** Manual. Human judgement determines the result.

**Outcome:** Users encounter controls in a sequence that preserves meaning and operation.

**Applies to:** Sequentially navigable pages where order affects meaning or operation.

**Procedure:** Traverse the task with Tab, Shift+Tab and appropriate composite-control keys. Repeat relevant state changes and reverse traversal. Compare the observed order with the task's meaning; do not impose a blanket screen-coordinate order. Include route changes, validation, insertion and removal of focused items; also complete Focus-T11. For aria-activedescendant widgets, record both DOM focus and active-item movement rather than assuming focus moves to every item.

**Expected result:** Focusable components receive focus in an order that preserves meaning and operability.

**Mapping and guidance:** [WCAG 2.4.3 Focus Order](https://www.w3.org/TR/WCAG22/#focus-order). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding focus order](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html).

**Automation relationship:** AUTO-17 tabindex and experimental focus-order-semantics provide best-practice assistance only.

**Evidence:** Expected sequence rationale, actual focus trace, keys used and state-transition captures. Record the common status and any outstanding scope.


### Focus-T02 Visible focus

**Method:** Manual. Human judgement determines the result.

**Outcome:** Keyboard users can see which component has focus.

**Applies to:** Every keyboard-operable interface and relevant component state.

**Procedure:** Reach every control using the keyboard. Without moving the pointer, identify the focused control in each state. Record missing or ambiguous indicators; do not add an unsupported numerical area threshold. Test keyboard focus across applicable backgrounds and states. Complete Focus-T08 separately for contrast. Do not impose the AAA focus-indicator area requirement as a 2.4.7 AA condition.

**Expected result:** A mode of operation provides a visible keyboard focus indicator.

**Mapping and guidance:** [WCAG 2.4.7 Focus Visible](https://www.w3.org/TR/WCAG22/#focus-visible). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding focus visible](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html).

**Automation relationship:** AUTO-12 does not assess focus-indicator contrast or prove visible focus.

**Evidence:** Focus sequence and screenshots or video showing the actual focused component. Record the common status and any outstanding scope.


### Focus-T03 Focus not entirely obscured

**Method:** Manual. Human judgement determines the result.

**Outcome:** Keyboard users can locate focused components when overlays are present.

**Applies to:** Focused components near sticky headers, banners, dialogs or other authored overlays.

**Procedure:** Navigate by keyboard with each relevant overlay open, including at scroll boundaries and responsive layouts. Confirm each focused component is not entirely hidden. Apply the criterion's notes for user-opened and repositionable content. For user-repositionable content, test its initial positions as required by the criterion. For user-opened content, verify and record whether the focused component can be revealed without advancing keyboard focus. Test sticky headers, footers and banners at relevant scroll positions and zoom settings. Partial obscuration can pass this minimum criterion while still failing another focus requirement or best practice.

**Expected result:** A focused component is not entirely hidden by author-created content, subject to the stated notes.

**Mapping and guidance:** [WCAG 2.4.11 Focus Not Obscured Minimum](https://www.w3.org/TR/WCAG22/#focus-not-obscured-minimum). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding focus not obscured minimum](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html).

**Automation relationship:** No direct axe test. Use the linked Understanding guidance for the exact notes.

**Evidence:** Focused selector, overlay and state, screenshots and any reveal sequence relied upon. Record the common status and any outstanding scope.


### Focus-T04 Context changes on focus

**Method:** Manual. Human judgement determines the result.

**Outcome:** Moving focus does not unexpectedly take users elsewhere.

**Applies to:** Every user-interface component that can receive focus.

**Procedure:** Move focus onto each component without activating it. Observe navigation, new windows, viewport movement and focus changes. Distinguish a content update from a change of context. Normal scrolling that brings the focused component into view is not automatically a context change. Record unexpected navigation, windows or focus relocation and distinguish ordinary content updates.

**Expected result:** Receiving focus does not initiate a change of context.

**Mapping and guidance:** [WCAG 3.2.1 On Focus](https://www.w3.org/TR/WCAG22/#on-focus). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding on focus](https://www.w3.org/WAI/WCAG22/Understanding/on-focus.html).

**Automation relationship:** No direct axe test.

**Evidence:** Focus-only sequence and before and after URL, window, viewport and focus evidence. Record the common status and any outstanding scope.


### Focus-T05 Context changes on input

**Method:** Manual. Human judgement determines the result.

**Outcome:** Changing an input does not cause an unannounced context change.

**Applies to:** Controls whose setting can be changed without a separate submit action.

**Procedure:** Inspect advice available before changing each setting. Change the control value or selection without invoking a separate submit or navigation control. Observe any context change. If one occurs, confirm that advance advice explained it. Include checkboxes, radio buttons, selects and editable fields. The action used to change a value is part of this test; only a separate submit or navigation action is excluded from the observation.

**Expected result:** Changing a setting does not automatically change context unless the user was advised beforehand.

**Mapping and guidance:** [WCAG 3.2.2 On Input](https://www.w3.org/TR/WCAG22/#on-input). Direct criterion reference; this test addresses the stated condition only.

**Interpretation:** [Understanding on input](https://www.w3.org/WAI/WCAG22/Understanding/on-input.html).

**Automation relationship:** AUTO-10 and AUTO-08 do not establish predictable context changes.

**Evidence:** Advance instruction, exact input action and before and after context evidence. Record the common status and any outstanding scope.


### Focus-T06 Focus after temporary content closes

**Method:** Assisted manual. Human judgement determines the result.

**Outcome:** Users continue from an appropriate location after temporary content closes.

**Applies to:** Temporary content that moved focus, including cases where its trigger remains, is removed or is superseded by the next workflow step.

**Procedure:** Open temporary content using the keyboard, interact inside it and close it through each supported keyboard route. Inspect the actual focused element and continue the task. Compare the destination with the documented workflow. Include Escape, close buttons and other supported keyboard closure routes. If the trigger has disappeared, identify a logical fallback. Where a workflow intentionally proceeds to another destination, record why that destination is appropriate even if the trigger still exists.

**Expected result:** Focus returns to the invoking control or another justified destination that preserves logical order and workflow.

**Mapping and guidance:** [WCAG 2.4.3 Focus Order](https://www.w3.org/TR/WCAG22/#focus-order). Supporting pattern; assess actual failure against the relevant criterion.

**Interpretation:** [Understanding focus order](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html).

**Automation relationship:** No direct axe test. Focus return is an implementation pattern supporting logical order, not a standalone WCAG criterion.

**Evidence:** Trigger identifier; each open/close sequence; resulting focus destination; removed-trigger or workflow exception rationale; evidence that the task can continue. Record the common status and any outstanding scope.


### Focus-T07 Visually hidden controls revealed on focus

**Method:** Manual. Human judgement determines the result.

**Outcome:** Keyboard users can see interactive controls intentionally revealed on focus.

**Applies to:** Visually hidden interactive controls intended to become available on keyboard focus, at page load or after dynamic changes.

**Procedure:** Identify intentionally hidden focus-revealed controls. Reach each through normal keyboard navigation and confirm that it and its focus indication become visible. Separately check that closed or inactive content does not unexpectedly receive focus. Repeat after dynamic changes as well as initial load. Distinguish an intentionally focus-revealed control from content inside a closed or inactive component, which should not unexpectedly enter keyboard navigation.

**Expected result:** The intended control becomes visible and identifiable on focus; inactive hidden content does not create unexpected focus stops.

**Mapping and guidance:** [WCAG 2.4.7 Focus Visible](https://www.w3.org/TR/WCAG22/#focus-visible). Supporting pattern; assess actual failure against the relevant criterion.

**Interpretation:** [Understanding focus visible](https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html).

**Automation relationship:** AUTO-04 skip-link and AUTO-08 aria-hidden-focus provide limited related evidence; experimental hidden-content is a state-discovery aid only.

**Evidence:** Initial and focused captures, key sequence and actual focus target. Record the common status and any outstanding scope.


### Focus-T08 Focus indicator contrast

**Method:** Assisted manual; scripted interactions may support repeatability, with human review of meaning and evidence.

**Outcome:** Users can distinguish applicable author-defined focus indicators from adjacent colours.

**Applies to:** Author-defined focus indications where contrast is required under 1.4.11.

**Procedure:** Reach the control by keyboard and capture its actual focus state. Identify the visual information needed to recognise that state. Measure applicable indicator contrast against adjacent colours across backgrounds and states, including gradients or images. Record relevant unmodified user-agent, inactive-component or essential-presentation exceptions.

**Expected result:** Applicable visual information identifying the focused state meets 3:1 contrast against adjacent colours, subject to the criterion exceptions. A visible indicator is still assessed separately under Focus-T02. Do not add an AAA area threshold or assume this requires comparison between the same pixels in focused and unfocused states.

**Mapping and guidance:** [WCAG 1.4.11 non text contrast](https://www.w3.org/TR/WCAG22/#non-text-contrast); [Understanding guidance](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html). Coverage is limited to the stated conditions.

**Automation and related tests:** AUTO-12 color-contrast does not assess this condition; Focus-T02 addresses visibility.

**Evidence:** Focused captures, measured colours and ratios, tool/method, tested backgrounds and exception rationale. Record the common status and outstanding scope.


### Focus-T09 Additional content on keyboard focus

**Method:** Assisted manual; scripted interactions may support repeatability, with human review of meaning and evidence.

**Outcome:** Users can perceive and, where required, dismiss additional content without losing their place.

**Applies to:** Author-controlled additional content shown and hidden on receiving and removing hover or focus, subject to 1.4.13 exceptions.

**Procedure:** Use keyboard focus to reveal applicable tooltips, submenus and nonmodal popups. Confirm persistence until the trigger is removed, the content is dismissed or the information is no longer valid. Where dismissal is required, dismiss it without moving focus. Record input-error, non-obscuring and unmodified user-agent exceptions. If pointer hover also triggers it, perform a separate pointer subcheck that moves onto the additional content, or record hoverability as outstanding.

**Expected result:** Applicable dismissal and persistence conditions pass. Record hoverability separately; do not claim a complete 1.4.13 pass when that applicable condition is untested. Focus-revealed skip controls themselves are assessed in Focus-T07, not as additional content here.

**Mapping and guidance:** [WCAG 1.4.13 content on hover or focus](https://www.w3.org/TR/WCAG22/#content-on-hover-or-focus); [Understanding guidance](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html). Coverage is limited to the stated conditions.

**Automation and related tests:** AUTO-08 aria-tooltip-name checks naming only; Keyboard-T01 establishes access to hover-available functions.

**Evidence:** Trigger, keys, persistence observations, dismissal route, exception decisions, pointer-subcheck result or outstanding scope. Record the common status and outstanding scope.


### Focus-T10 Modal focus entry and containment

**Method:** Assisted manual; scripted interactions may support repeatability, with human review of meaning and evidence.

**Outcome:** Keyboard users enter, operate and close a modal without accidentally interacting with its background.

**Applies to:** Modal dialogs, including nested dialogs where implemented.

**Procedure:** Open the modal using the keyboard. Verify initial focus is a sensible element inside it, considering lengthy or structured content. Traverse forward and backward and use the component key model. Confirm background content is inactive while the modal is active. Check a keyboard close route, including Escape where the agreed pattern supports it. Close nested dialogs one level at a time and complete Focus-T06.

**Expected result:** Initial focus supports understanding and operation; focus remains within the active modal during traversal; the user can close it and continue logically. Record pattern deviations separately from confirmed criterion failures.

**Mapping and guidance:** [WCAG 2.4.3 focus order](https://www.w3.org/TR/WCAG22/#focus-order); [Understanding guidance](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html). Supporting implementation assessment, not a standalone additional WCAG requirement.

**Automation and related tests:** Keyboard-T01, Keyboard-T03 and Focus-T06 cover related conditions. AUTO-08 name/ARIA findings do not establish modal behaviour.

**Evidence:** Initial destination rationale, forward/reverse trace, background-interaction check, closure routes and returned focus. Record the common status and outstanding scope.


### Focus-T11 Focus continuity after dynamic changes

**Method:** Assisted manual; scripted interactions may support repeatability, with human review of meaning and evidence.

**Outcome:** Keyboard users retain a meaningful position as content and workflow change.

**Applies to:** Route changes, validation, insertion, deletion, replacement, asynchronous updates and removal of the focused element.

**Procedure:** Trigger each change using the keyboard. Record focus before and after, active-descendant changes where relevant, and the next navigation step. Check the user can find relevant content and continue. If focus moves, assess its destination; if it remains, assess whether that is appropriate. Do not demand focus movement for every update.

**Expected result:** Focus is retained or moved intentionally to a logical location, and task completion remains possible. Content replacement does not cause unintended loss of position. Record status-message announcements as a separate assessment where needed.

**Mapping and guidance:** [WCAG 2.4.3 focus order](https://www.w3.org/TR/WCAG22/#focus-order); [Understanding guidance](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html). Supporting implementation assessment, not a standalone additional WCAG requirement.

**Automation and related tests:** Focus-T01 and Keyboard-T01 cover order and operation. AUTO-06 checks document titles, not focus continuity.

**Evidence:** Before/after element and active-item identifiers, state transition, destination rationale, subsequent key sequence and any separate announcement-assessment reference. Record the common status and outstanding scope.


## Recording results

### Result statuses

| Status | Use |
| --- | --- |
| Not run | The planned assessment has not been performed. Record its outstanding scope. |
| Blocked | Access, environment, engine error or prerequisites prevent assessment. Record cause, owner and next action. |
| Needs review | An axe incomplete result, interpretation or evidence decision remains unresolved. Do not treat as a pass. |
| Fail | An applicable rule condition or manual acceptance condition has a confirmed failure. Record WCAG or best-practice classification separately. |
| Pass | The defined applicable condition was assessed with adequate evidence and no unresolved failure or review finding. Scope remains explicit. |
| Not applicable | The condition does not apply to the assessed scope, with a recorded reason. Inaccessible or excluded content is not automatically not applicable. |

### Raw automation results and review

Preserve violations, passes, incomplete and inapplicable collections without rewriting the original output. A raw pass covers only the engine's tested condition. Incomplete findings become Needs review until resolved; capture the reviewer, decision and evidence alongside the original result. Engine errors and missing frame coverage remain visible. Multiple-label, duplicate-ID/ARIA and other review findings require contextual decisions rather than mechanical remediation.

An empty violations collection is not an overall accessibility pass. Retain raw output and attach it to the Playwright result. For reproducible defects, retain a stable finding fingerprint using rule ID, target and state, rather than relying on a screenshot or an entire changing raw JSON snapshot alone.

### Required record fields

| Field group | Required information |
| --- | --- |
| Identity | Run ID, plan/profile version, test ID, rule ID if relevant, finding ID and timestamp. |
| Product | Product, build/commit, environment, URL/route, journey, component and state. |
| Test environment | OS, browser and version, viewport, zoom, relevant keyboard-navigation settings and any assistive technology relied upon. |
| Automation | axe-core, integration and Playwright versions; effective rule profile; options; includes/exclusions; frame/shadow boundaries; execution errors. |
| Assessment | Method, applicability, expected result, actual result, exact keys/state setup and six-status result. |
| Classification | Applicable WCAG criterion and level where established, best-practice or experimental status, raw axe impact and independently assessed defect priority. |
| Evidence | Raw JSON reference, target/frame/shadow path, relevant node HTML, screenshots/recording/focus trace, contrast measurements where relevant and review notes. |
| Decisions | Exception or not-applicable rationale, reviewer, review date, excluded or untested scope and follow-up owner. |
| Remediation | Defect reference, owner, agreed action, retest build/date, retest evidence and result. |

### Group summaries and coverage

Keep WCAG, best-practice and experimental summaries separate. Preserve per-rule, per-node and per-state records. Use this precedence for a derived group status: Fail, Blocked, Needs review, Not run, Pass, Not applicable. This is a reporting convention, not a severity ranking. Always show counts and outstanding coverage alongside the derived status, so a confirmed failure does not hide blocked or untested work.

A group can pass only when all required scope has been assessed, applicable conditions pass, and there are no unresolved failures, review findings, blocks or not-run items. A mix of Pass and justified Not applicable may summarise as Pass; an entirely non-applicable group summarises as Not applicable. Record coverage independently as Complete or Incomplete, with required, assessed and outstanding items listed. A failing group can still have complete assessment coverage.

Do not transfer an automated result to a manual test or transfer one tested component's result to another untested instance or state without an explicit, justified sampling decision. If sampling is used, report the sampled scope and limits; do not imply exhaustive testing.

### Exceptions and release decisions

An accepted defect remains Fail with a separate acceptance decision, owner and review date. A valid criterion exception is different: record why the exception applies before determining the result. Do not conflate an exception, a false positive, an accepted defect and an untested item.

Define release gates locally before execution. Report confirmed WCAG defects, recommended best-practice improvements and experimental findings distinctly. Completion of this first-stage plan does not establish conformance. Before any broader accessibility statement, complete the remaining applicable assessments and retain the scope and evidence underpinning that statement.

### Wider assessments outside this stage

Maintain a named outstanding-assessment register covering applicable work beyond this plan: semantic accuracy, meaningful alternatives, assistive-technology operation and announcements, full zoom/reflow/text-spacing testing, media quality and live captions, flashing and timing, pointer/gesture and dragging requirements, and other applicable WCAG criteria. These examples are not a complete conformance checklist. Keyboard support does not by itself satisfy pointer-specific requirements.

## Guidance and retained source links

The original rule destinations are retained in the automated catalogue. Original WCAG destinations are retained with their criterion fragments. Some are now explicitly related guidance rather than claims of tested coverage. The original internal navigation purposes are retained in Navigation.

- [axe-core repository](https://github.com/dequelabs/axe-core)
- [Pinned axe-core 4.12.1 rule inventory](https://github.com/dequelabs/axe-core/blob/v4.12.1/doc/rule-descriptions.md)
- [axe-core 4.12 rule guidance](https://dequeuniversity.com/rules/axe/4.12)
- [Keyboard Accessible Understanding Guideline 2.1](https://www.w3.org/WAI/WCAG22/Understanding/keyboard-accessible.html)
- [Navigable Understanding Guideline 2.4](https://www.w3.org/WAI/WCAG22/Understanding/navigable)
- [WCAG 2.2 specification](https://www.w3.org/TR/WCAG22/)
- [Playwright accessibility testing](https://playwright.dev/docs/accessibility-testing)
- [WAI-ARIA Authoring Practices component patterns](https://www.w3.org/WAI/ARIA/apg/patterns/)
- [Modal dialog pattern and focus return exceptions](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)

## Exact rule profiles

These are rule-ID data lists for the pinned engine, not complete executable Playwright scripts. Combine baseline lists for the 90-rule baseline, explicitly activate target-size, and run the experimental list separately. Keep outcomes classified by the list they came from.


### Baseline WCAG rules

```json
[
  "area-alt",
  "aria-allowed-attr",
  "aria-braille-equivalent",
  "aria-command-name",
  "aria-conditional-attr",
  "aria-deprecated-role",
  "aria-hidden-body",
  "aria-hidden-focus",
  "aria-input-field-name",
  "aria-meter-name",
  "aria-progressbar-name",
  "aria-prohibited-attr",
  "aria-required-attr",
  "aria-required-children",
  "aria-required-parent",
  "aria-roles",
  "aria-tab-name",
  "aria-toggle-field-name",
  "aria-tooltip-name",
  "aria-valid-attr",
  "aria-valid-attr-value",
  "autocomplete-valid",
  "avoid-inline-spacing",
  "blink",
  "button-name",
  "bypass",
  "color-contrast",
  "definition-list",
  "dlitem",
  "document-title",
  "duplicate-id-aria",
  "form-field-multiple-labels",
  "frame-focusable-content",
  "frame-title",
  "frame-title-unique",
  "html-has-lang",
  "html-lang-valid",
  "html-xml-lang-mismatch",
  "image-alt",
  "input-button-name",
  "input-image-alt",
  "label",
  "link-in-text-block",
  "link-name",
  "list",
  "listitem",
  "marquee",
  "meta-refresh",
  "meta-viewport",
  "nested-interactive",
  "no-autoplay-audio",
  "object-alt",
  "role-img-alt",
  "scrollable-region-focusable",
  "select-name",
  "server-side-image-map",
  "summary-name",
  "svg-img-alt",
  "target-size",
  "td-headers-attr",
  "th-has-data-cells",
  "valid-lang",
  "video-caption"
]
```


### Baseline best practice rules

```json
[
  "accesskeys",
  "aria-allowed-role",
  "aria-dialog-name",
  "aria-text",
  "aria-treeitem-name",
  "empty-heading",
  "empty-table-header",
  "frame-tested",
  "heading-order",
  "image-redundant-alt",
  "label-title-only",
  "landmark-banner-is-top-level",
  "landmark-contentinfo-is-top-level",
  "landmark-main-is-top-level",
  "landmark-no-duplicate-banner",
  "landmark-no-duplicate-contentinfo",
  "landmark-no-duplicate-main",
  "landmark-one-main",
  "landmark-unique",
  "meta-viewport-large",
  "page-has-heading-one",
  "presentation-role-conflict",
  "region",
  "scope-attr-valid",
  "skip-link",
  "tabindex",
  "table-duplicate-name"
]
```


### Experimental rules

```json
[
  "css-orientation-lock",
  "focus-order-semantics",
  "hidden-content",
  "label-content-name-mismatch",
  "p-as-heading",
  "table-fake-caption",
  "td-has-header"
]
```


## Changes in this revision

- Retained the original 74 rules and their guidance links; added 16 best-practice rules and a separate seven-rule experimental profile.
- Retained and refined Keyboard-T01 to Keyboard-T05 and Focus-T01 to Focus-T07. Added Keyboard-T06 and Focus-T08 to Focus-T11.
- Corrected outcomes, procedures, applicability and limits, including focus-return exceptions and partial WCAG mappings.
- Added explicit cross-references without transferring automated passes to manual tests.
- Removed client-specific requirements and wording; replaced unspecified inherited-document dependencies with self-contained procedures.
- Added six result statuses, separate coverage reporting, review decisions, profile control and reproducible evidence requirements.

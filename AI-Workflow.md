# AI Workflow

## Planning Phase Steps

1. Clone **3 repositories**.
   - your app repository
   - sol-components: https://github.com/nice-cxone/sol-components.git  
   - NICE: https://github.com/jenkins-ravtech1/NICE
2. Open a new **VS Code workspace** which includes the 3 repositories.
3. Check if in your package.json these libraries have updated to the below versions:
```
"@niceltd/cxone-client-platform-services": "^33.5.0",
"@niceltd/cxone-core-services": "^27.6.0",
```
4. In your app repo, run: `npx bmad-method install`

Installer Choices:
```
- PRD sharded into multiple files: **Yes**
- Architecture docs sharded into multiple files: **Yes**
- IDE configured: **GitHub Copilot**
- Copilot config: **Recommended defaults**
- Pre-built web bundles: **No**
```
5. Optional => Run: `npm install -g @kayvan/markdown-tree-parser`
6. Generate Copilot instructions.
7. `@analyst create-project-brief` (paste the prompt below)
```
MODE: YOLO
TITLE: NICE Angular Migration - Breeze to SOL - Analysis Only
OWNER: RavTech R\&D
METHODOLOGY: bmad-method

CONTEXT

We are migrating an Angular MFE from Breeze components to SOL components.

The workspace includes two local repositories named sol-components and NICE that provides the SOL design system and migrating instructions. You MUST look at these two local repositories before writing any code.

Module Federation architecture stays as is. Backend APIs and business logic remain unchanged.

Goal of this brief: produce a precise analysis to guide a safe, incremental UI component replacement.

You MUST learn deeply the the sol-components repository that make a successful migration. This repository is in the same workspace.

PROGRAM GOAL (APPENDIX A CONTEXT)

Program-level objective: by the end of the migration program, no Breeze components remain in any application listed in Appendix A.

This brief applies per application. For multi-application scope, provide an application inventory (Appendix A) and run one analysis brief per app.

STRICT SCOPE BOUNDARIES

Do NOT propose new features, product KPIs, budgets, or timelines.

Do NOT introduce AI tooling, training plans, or documentation programs.

Do NOT add new test coverage. Only identify where existing unit tests or Playwright E2E tests must be updated due to selector or behavior changes.

Accessibility: rely on SOL defaults for better a11y. No formal 100% audits required.

Performance: no synthetic benchmarks or optimizations beyond what SOL naturally provides.

Do NOT create backup files or temporary “bridge” components.

Rollback is out of scope: do not analyze, design, or propose rollback strategies, switch-back procedures, feature flags/toggles for rollback, dual-running modes, or downgrade paths. Treat this analysis as forward-only migration.

OBJECTIVE

Create an actionable analysis that inventories Breeze usage, maps each item to its SOL equivalent, and pinpoints the exact technical deltas required for migration without changing business behavior.

STAGES (ANALYSIS-ONLY INTERPRETATION)

Initial Application Analysis

Identify all Breeze components used (including deprecated ones).

Highlight custom components that require manual migration.

Test Strategy Validation (No Execution)

Document exact commands and prerequisites to run unit and E2E tests locally.

List current blockers (env, CI, data, auth) and required fixes; do not execute or implement fixes in this brief.

Cross-Discipline Review (PM/UX/Dev)

Review analysis outputs (customs, deprecated, deltas) and decide mitigation per component.

Agree on any SOL-driven look & feel adjustments (no redesigns). Publish a short summary of agreements and action items (as analysis artifacts).

References and Standards

Align recommendations with NICE development guidelines and SOL documentation.

ASSUMPTIONS

A key technical stakeholder is available to validate local setup steps.

Links, installation details, and access to repositories/documentation/permissions are provided before analysis starts.

Existing unit and integration (E2E) tests exist and were functional before migration.

Supporting teams (Product, UX, Dev, QA) are available for reviews per agreed SLAs; lack of availability may cause delays.

The latest SOL version remains stable and available during the analysis window; unexpected SOL changes may alter recommendations.

Security and compliance reviews remain the responsibility of the application team.

All applications in scope are technically ready for analysis (working dev environments, required permissions, repository access).

MANDATORY MIGRATION RULES (INCLUDE EXPLICITLY IN THE BRIEF)

Completeness: for each Breeze component type, confirm all occurrences are accounted for—none missed.

Hard replacement policy: even raw <button> elements must be replaced by sol-button.

Translation module change:

Replace:
import { TranslationModule } from '@niceltd/cxone-components/translation'

With:
import { TranslationModule } from '@niceltd/cxone-core-services'

Styling: identify missing styling and specify exactly how to include/port it (global styles, per-component SCSS, theming tokens). The brief must call out any required style imports or token replacements.

E2E discipline: do not wrap Playwright locators in try/catch; allow failures to surface. Recommend stable selectors (data-testid/role).

Dropdown conversion: cxone Single Select Dropdown and Multiselect Dropdown must be converted to the SOL Dropdown using the appropriate SOL properties.

the click event in sol-button is buttonClick, not click.

The new toaster comes from import { ToastrManagerModule } from '@niceltd/sol/toastr';

Include the correct styles import:

node\_modules/@niceltd/sol/src/styles/typefaces.css,

node\_modules/@niceltd/sol/src/styles/sol-core.scss
in
angular.json/projects/<MY_APP>/architect/build/options/styles

DELIVERABLES

Component Inventory

Table: Breeze component name, file path, usage context, complexity tag.

Breeze-to-SOL Mapping

Table: Breeze component → SOL component, import path from sol-components, API/event differences, data-shape adapters if needed.

Binding and Event Delta List

Where inputs, outputs, or handlers must change when switching to SOL.

Test Impact Report

Unit tests impacted: file paths and reason.

Playwright E2E specs impacted: locators to update, suggested stable selectors (data-testid or role-based).

Do not recommend try/catch around locators.

Grid-specific plan: identify specs impacted by virtualization; propose rowBuffer guidance and when to enable local Chromium memory flag.

Risk and Complexity Assessment

Rank migration complexity per component: low, medium, high.

Call out non-1:1 mappings and manual work items.

Phase Hints

Safe grouping of components for incremental migration, based on shared modules and risk.

Verification & Cleanup Plan (Analysis Checklist)

How to run all unit tests and the expectation that all must pass post-migration; list the exact code/test fixes required if deltas break tests.

Commands to run tests:

All tests: npm run test

Specific test: npm test -- --include="\*\*/<my-test-file>.spec.ts"

Dependency cleanup (if present and directly tied to Breeze→SOL transition):

Remove @niceltd/cxone-domain-components from package.json and eliminate all references/usages.

Remove @niceltd/sol from package.json and eliminate all references/usages if sol-components local repo is the sole SOL source.

Add to the clean up story:

Perform a thorough search and confirm that there are no remaining imports from @niceltd/cxone-components

In the cleanup story, all import like import { CheckboxModule as SolCheckboxModule } from '@niceltd/sol/checkbox'

Must be updated to import { CheckboxModule } from '@niceltd/sol/checkbox' and update also the use.

METHOD – BMAD-METHOD APPLIED

Use bmad-method to:
a) Scan codebase for Breeze imports, selectors, and templates.
b) Build a structured index of all Breeze components and their occurrences (enforce “no missing instance” rule).
c) Cross-reference sol-components to suggest the closest SOL equivalents.
d) Extract Angular binding signatures and event handlers to find required deltas.
e) Parse unit test files and Playwright tests to identify selectors coupled to Breeze markup and where they must change.
f) Identify styling dependencies/tokens and specify inclusion steps.

WHAT TO ANALYZE

UI layers only: components, templates, directives, styles tied to Breeze.

Data binding and event flows where SOL requires different prop or event names.

ag-grid or custom grid integration that must move to SOL grid patterns.

Modal, form controls, validation, toasts, navigation elements.

Test selectors that rely on Breeze CSS or structure.

Raw HTML controls (e.g., <button>) that must become SOL equivalents.

WHAT NOT TO ANALYZE (HARMONIZED OUT OF SCOPE)

Business logic, services, API contracts, backend code.

Product metrics, surveys, adoption KPIs.

Budget, hiring, or schedules.

Cross-MFE platform integration beyond current boundaries.

UI changes not directly required by the SOL migration (beyond SOL defaults).

Handling dependencies or UI components not part of Breeze→SOL, except the targeted cleanup listed in the Verification & Cleanup Plan.

Issues arising from missing elements in SOL, unless explicitly addressed as part of the migration plan.

Performance optimizations beyond regression prevention; no major refactors.

Refactoring of non-Breeze code not explicitly required for migration.

New feature requests.

Fixing legacy bugs unrelated to the migration (only migration-caused issues to be addressed).

Creating new automated tests. (If policy requires tests “directly related to a SOL component”, include them as recommendations only; no implementation in this brief.)

Handling infrastructure/deployment/CI failures not caused by the migration (owned by the application team).

Rollback planning/toggles/downgrade paths are excluded (no rollback analysis, no feature flags for rollback, no dual track).

OUTPUT FORMAT

Provide results as clearly labeled markdown sections:

Component Inventory

\| Breeze Component | File Path | Usage Context | Complexity |

Breeze-to-SOL Mapping

\| Breeze | SOL Equivalent | Import Path | API Deltas | Event Deltas | Data Adapter Needed |

Binding and Event Deltas

<path>: <delta description>

Test Impact Report

Unit: <test file> → reason and exact lines if possible

E2E: <spec file> → old locator → suggested SOL locator

Risks and Gaps

<component>: no direct SOL mapping. Proposed workaround.

Phase Hints

Phase 1: <group>, rationale

Phase 2: <group>, rationale

Verification & Cleanup Plan

Test commands, pass criteria, and dependency removals (including @niceltd/cxone-domain-components and, if applicable, @niceltd/sol).

QUALITY RULES

Maintain functional parity. Do not alter workflows.

Keep imports accurate to local sol-components.

Prefer stable test locators: data-testid or role based, not brittle CSS.

No backup files or temporary components; replacements are direct.

Enforce <button> → sol-button replacement.

Apply the translation module import swap exactly as specified.

Flag only necessary changes. Avoid scope creep.

ACCEPTANCE CRITERIA FOR THIS ANALYSIS

Every Breeze usage is accounted for with a proposed SOL target or a flagged gap—no missed instances.

Test impact lists concrete files and suggested new locators (no try/catch masking).

GRID: E2E plan includes rowBuffer guidance and when to enable the local Chromium memory flag.

MODAL: all dialog instances have a documented path replacing DynamicDialog\* with ModalService/MAT\_DIALOG\_DATA, with height removed and result subscription modeled.

FLOATING MENU: all legacy menus mapped to SOL (FloatingMenuModule, MatMenuModule, MenuItem usage) with menu behaviors (e.g., dynamic labels) specified.

Styling requirements are explicitly identified with inclusion steps.

Removal of @niceltd/cxone-domain-components and (if present) @niceltd/sol is included in cleanup.

Risks are clearly surfaced with actionable notes.

No rollback content is included (no strategies, toggles, or downgrade procedures).

No out-of-scope items appear in the output.

READY. Execute analysis now in YOLO mode and produce the brief in the specified markdown structure.
```
8. **New chat.**
9. `@pm create-doc prd yolo mode`
10. **New chat.**
11. `@architect create-doc architecture yolo mode`
12. **New chat.**
13. `@po execute-checklist-po yolo mode`
14. **New chat.**
15. `@po shard docs/prd.md to prd`

## Development Workflow (IDE Only)

1. **New chat.**
2. `@sm create-next-story <STORY_NUMBER>`
3. READ the story content.
4. Change status to **Approved**.
5. **New chat.**
6. `@dev implement-story <STORY_NUMBER>`
7. In the same chat, write:

       Perform a thorough search and confirm that all instances of the component have been fully migrated`.

8. Check in the story `.md` file that **all tasks are checked**.
9. **New chat.**
10. `@qa review-story <STORY_NUMBER>`
11. **IMPORTANT** Check that all unit tests passed successfully.
12. **The status of the story must be Done** _OR_ write:

        If the status of the story is Done, update it to Done.

13. **IMPORTANT** Commit your work before starting a new story.
14. Go back to step 1 to start the next story.

## Notes — Out of AI Scope

1. **Do not** update components in bulk with **PowerShell** (files can become corrupted). Proceed **one by one**.
2. Update `@niceltd/cxone-client-platform-services` **and** `@niceltd/cxone-core-services` **before** removing `@niceltd/cxone-components`.

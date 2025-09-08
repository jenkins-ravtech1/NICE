# AI Workflow

## Pre-planning
Run the app, the unit tests and the e2e to check if all works well.

## Planning Phase Steps

1. Clone **3 repositories**.
   - your app repository
   - sol-components: https://github.com/nice-cxone/sol-components.git  
   - Sol-Migration-Notes: https://github.com/jenkins-ravtech1/Sol-Migration-Notes.git
2. In VS Code, open your application folder. Then click on Add Folder to Workspace and select the two other folders. After that, you can save the workspace to your disk (make sure to save it alongside your application folder, not inside it).
3. Check if in your package.json these libraries have updated to at least the below versions:
```
"@niceltd/cxone-client-platform-services": "^33.5.0",
"@niceltd/cxone-core-services": "^27.6.0",
```
4. In your app repo, run: `npx bmad-method install`

Installer Choices:
```
- Type .
- BMad Agile Core System (v4.43.0) .bmad-core
- PRD sharded into multiple files: **Yes**
- Architecture docs sharded into multiple files: **Yes**
- IDE configured: **GitHub Copilot**
- Copilot config: **Recommended defaults**
- Pre-built web bundles: **No**
```
5. Optional => Run: `npm install -g @kayvan/markdown-tree-parser`
6. Generate Copilot instructions.
7. **New chat (ALWAYS use Sonnet 4).**

`@analyst create-project-brief` (paste the prompt below)
```
CONTEXT
- We are migrating an Angular MFE from Breeze components to SOL components.
- SOURCES YOU MUST READ BEFORE WRITING ANYTHING:
  1) "sol-components" — SOL design system source (authoritative for APIs, inputs/outputs, styles, tokens).
  2) "Sol-Migration-Notes" — NOTES-ONLY repo (Markdown/snippets/checklists) capturing mappings, decisions, and gotchas from prior manual SOL migrations. This is NOT the target app repo and MUST NOT be imported or referenced as code.
- Target application code = the CURRENT project repository you are analyzing (not "Sol-Migration-Notes").
- Invariants: Module Federation architecture stays as is. Backend APIs and business logic remain unchanged.
- Client-only scope: If the project includes a backend, IGNORE all server-side code. Do not analyze, modify, or document backend services. Treat backend APIs as fixed external contracts and reference them only to clarify current client usage.

USING THE NOTES (MANDATORY)
- Extract only stable rules: confirmed component mappings, required style imports, known event/name changes (e.g., buttonClick), selector patterns, common pitfalls.
- Conflict resolution order: 1) sol-components API/docs, 2) current app code behavior, 3) Sol-Migration-Notes. If a conflict exists, flag it in "Open Issues / Risks" with the exact file/heading from the notes.
- For every adopted rule from the notes, cite the note file path and heading (relative path), e.g., docs/sol/button.md#events.
- Convert any example selectors from the notes into robust recommendations (data-testid/ARIA roles), not brittle CSS chains.
- Do NOT treat the notes as source code; they are guidance and conventions only.

SCOPE (WHAT TO DO)
- Enumerate every Breeze component usage and map it to its SOL replacement.
- Specify exact import changes, template deltas, input/output handler changes, and styling includes.
- Identify ONLY the updates required to EXISTING unit tests and Playwright E2E tests due to selector/behavior changes (no new tests).
- Provide a safe grouping plan for incremental migration, and a per-component complexity rating (Low/Medium/High).

NON-GOALS (MUST NOT)
- Do NOT propose new features, KPIs, budgets, timelines, or process/training/documentation programs (including AI tooling).
- Do NOT add new test coverage beyond the minimum updates caused by the migration.
- Do NOT create backup files or temporary “bridge” components.
- Do NOT design rollback strategies, feature flags for rollback, dual-running modes, or downgrade paths. Forward-only analysis.
- Do NOT perform synthetic performance work beyond what SOL naturally provides.
- Do NOT run or require full accessibility audits; rely on SOL defaults.
- Do NOT analyze, change, or document any backend code, endpoints, DTOs, auth flows, or infrastructure.

GLOBAL HARD RULES
- Completeness: confirm that ALL Breeze occurrences are accounted for—none missed.
- Hard replacement: even raw `<button>` must become `<sol-button>`.
- Translation module swap:
  // Replace:
  import { TranslationModule } from '@niceltd/cxone-components/translation'
  // With:
  import { TranslationModule } from '@niceltd/cxone-core-services'
- SOL button event: `(buttonClick)` is the event, NOT `(click)`. Update handlers accordingly.
- Toaster: use `import { ToastrManagerModule } from '@niceltd/sol/toastr';`.
- Styles (angular.json → projects/<MY_APP>/architect/build/options/styles):
  - node_modules/@niceltd/sol/src/styles/typefaces.css
  - node_modules/@niceltd/sol/src/styles/sol-core.scss
- Alias cleanup: replace `import { CheckboxModule as SolCheckboxModule } from '@niceltd/sol/checkbox'`
  with `import { CheckboxModule } from '@niceltd/sol/checkbox'` and update usages accordingly.
- Dropdowns: Breeze Single-Select & Multi-Select → SOL Dropdown(s) with the correct SOL inputs/outputs.
- E2E discipline: do NOT wrap Playwright locators in try/catch; let failures surface. Prefer stable selectors (data-testid/roles).
- Grid/virtualization: call out specs impacted by virtualization; provide `rowBuffer` guidance and when a local Chromium memory flag is warranted during runs.

CLEANUP (REQUIRED)
- Remove `@niceltd/cxone-components` and `@niceltd/cxone-domain-components` from package.json and code.
- Add to cleanup story: perform a THOROUGH search ensuring NO remaining imports from `@niceltd/cxone-components`.

SEARCH HINTS (document what you executed)
- rg -n "@niceltd/cxone-components" src/
- rg -n "@niceltd/cxone-domain-components" src/
- rg -n "CheckboxModule as SolCheckboxModule" src/

TEST COMMANDS (document in the brief)
- All tests: `npm run test`
- Single test: `npm test -- --include="**/<my-test-file>.spec.ts"`
- Expectation: all tests pass post-migration; list exact fixes required if deltas break tests.

DELIVERABLE FORMAT (STRICT — USE EXACT SECTION ORDER & HEADINGS BELOW)

# Breeze→SOL Migration Brief (BMAD)
**App/Module:** <MY_APP>  
**Date:** <YYYY-MM-DD>  
**Author:** <AUTHOR>

## A. Component Mapping Table
| Breeze Component | SOL Replacement | Inputs (old → new) | Outputs/Events (old → new) | Template Changes (concise snippet) | Test Impact (selectors/flows) | Complexity (L/M/H) |
|---|---|---|---|---|---|---|

> Include rows, at minimum, for: Buttons, Dropdowns (single & multi), Modals/Dialogs, Floating Menus, Grid/Data Table interactions, Text inputs, Checkboxes, Radios/Toggles, Tabs, Toasts/Notifications, Tooltips, Pagination (if present).

## B. Cross-Cutting Deltas
- **Imports:** every global import change (with file paths).
- **Events/Handlers:** list all `(click)` → `(buttonClick)` changes and others.
- **Styling/Theming:** exact steps to include SOL global styles and any per-component SCSS/token changes.
- **i18n:** apply the `TranslationModule` swap everywhere applicable.
- **E2E/Selectors:** recommended `data-testid`/role selectors per component; no try/catch around locators.
- **Grid/Virtualization:** affected specs, proposed `rowBuffer` values, and when to use a higher local Chromium memory setting.

## C. Modal & Floating Menu Migration
- **Modal/Dialog:** document the replacement path for any legacy dynamic dialogs → SOL modal service. If Angular Material interop is used, pass data via `MAT_DIALOG_DATA`. Remove fixed `height`; show the result subscription pattern.
- **Floating Menus:** map legacy menus to SOL (`FloatingMenuModule` / `MatMenuModule` as applicable), including dynamic labels/enable/disable rules.

## D. Grouping & Plan
- **Incremental groups:** safe batches based on shared modules/dependencies and risk.
- **Complexity ranking:** L/M/H with a 1–2 sentence justification for each component/group.

## E. Test Impact
- **Commands to run tests** (as above).
- **Exact fixes** required where deltas break tests (selectors, roles, timing, virtualization notes).

## F. Verification Checklist
- [ ] All Breeze components replaced per table  
- [ ] All imports updated (i18n, SOL modules, CheckboxModule alias removed)  
- [ ] Styles added in `angular.json` (both SOL files)  
- [ ] Playwright selectors stabilized (no try/catch)  
- [ ] Grid virtualization guidance applied where needed  
- [ ] `@niceltd/cxone-components` and `@niceltd/cxone-domain-components` removed  
- [ ] Dropdowns converted to SOL with correct props  
- [ ] Toastr migrated to `@niceltd/sol/toastr`

## G. Open Issues / Risks / TODOs
- List unknowns or places needing UX/product confirmation.

WRITING RULES
- Be specific and terse. Include file paths and exact import lines/snippets.
- Use MUST / MUST NOT consistently. No vague language.
- If a fact is unknown, add a TODO with what evidence is missing.
```
8. **New chat.**
9. `@pm create-doc prd`
If the chat asks this kind of question:,
```
Select 1-9 or just type your question/feedback:

Proceed to next section
Expand or Contract for Audience - Adjust detail level and complexity for specific stakeholders
Explain Reasoning (CoT Step-by-Step) - Walk through my analysis process and assumptions
Critique and Refine - Review for flaws and suggest improvements
Analyze Logical Flow and Dependencies - Examine structure and sequencing
Assess Alignment with Overall Goals - Evaluate contribution to objectives
Identify Potential Risks and Unforeseen Issues - Brainstorm risks from PM perspective
Challenge from Critical Perspective - Play devil's advocate on current content
Tree of Thoughts Deep Dive - Explore multiple reasoning paths for goals definition
```
you can write it
```yolo mode```
It will run all the steps at once.

10. **New chat.**
11. `@architect create-doc architecture`
12. **New chat.**
13. `@po execute-checklist-po`
14. **New chat.**
15. `@po shard docs/prd.md to prd`

## Development Workflow (IDE Only)

1. **New chat.**
2. `@sm create-next-story`
3. READ the story content.
4. Change status to **Approved**.
5. **New chat.**
6. `@dev implement-story`
7. In the same chat, write the below prompt:

       Perform a thorough search and confirm that all instances of the component have been fully migrated.

8. Check in the story `.md` file that **all tasks are checked**.
9. **New chat.**
10. `@qa review-story`
11. **IMPORTANT** Check that all unit tests passed successfully.
12. **The status of the story must be Done** _OR_ write:

        If the status of the story is Done, update it to Done.

13. **IMPORTANT** Commit your work before starting a new story.
14. Go back to step 1 to start the next story.

## Notes — Out of AI Scope

1. **Do not** update components in bulk with **PowerShell** (files can become corrupted). Proceed **one by one**.
2. Update `@niceltd/cxone-client-platform-services` **and** `@niceltd/cxone-core-services` **before** removing `@niceltd/cxone-components`.

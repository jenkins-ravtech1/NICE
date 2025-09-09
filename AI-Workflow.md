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

**If at the end of the chat response, the chat asks this kind of question:**
```
Select 1-9 or just type your question/feedback ...
```
**you can write in the chat window
```yolo mode```
It will run all the steps at once.**

7. **New chat (ALWAYS use Sonnet 4).**

`@analyst create-project-brief` (paste the prompt below)
```
The goal of this project is to migrate the current Angular MFE application from Breeze components to SOL components.
- Sources you MUST read before writing any code:
  1) The "sol-components" folder that holds the migration documentation.
  2) "Sol-Migration-Notes" folder developer NOTES-ONLY capturing decisions, and gotchas from prior manual SOL migrations.
- Invariants: Module Federation architecture, Backend APIs and business logic stays as is and should NOT be changed.
- Client-only scope: If the project includes a backend, IGNORE all server-side code. Do not analyze, modify, or document backend services. Treat backend APIs as fixed external contracts and reference them only to clarify current client usage.
- Create a **Component Mapping Table** in Markdown with the following columns:
  | Breeze Component | SOL Replacement | Inputs (old → new) | Outputs/Events (old → new) | Template Changes (concise snippet) | Test Impact (selectors/flows) | Complexity (L/M/H) |
  The table MUST cover all cxone components affected by the migration to SOL.

NON-GOALS (MUST NOT)
- Do NOT propose new features, KPIs, budgets, timelines, or process/training/documentation programs (including AI tooling).
- Do NOT add new test coverage beyond the minimum updates caused by the migration.
- Do NOT create backup files.
- Do NOT design rollback strategies, feature flags for rollback, dual-running modes, or downgrade paths.
- Do NOT perform synthetic performance work beyond what SOL naturally provides.
- Do NOT run or require full accessibility audits; rely on SOL defaults.
- Do NOT analyze, change, or document any backend code, endpoints, DTOs, auth flows, or infrastructure.

GLOBAL HARD RULES
- Completeness: confirm that ALL Breeze occurrences are accounted for—none missed.
- Hard replacement: even raw `<button>` MUST become `<sol-button>`.
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
- Alias cleanup: MUST be done to all components, for example: replace `import { CheckboxModule as SolCheckboxModule } from '@niceltd/sol/checkbox'`
  with `import { CheckboxModule } from '@niceltd/sol/checkbox'` and update usages accordingly.
- Dropdowns: Breeze Single-Select & Multi-Select → SOL Dropdown(s) with the correct SOL inputs/outputs.
- Icons: replace all `<cxone-svg-sprite-icon>` with `<sol-icon>` — **do NOT** use `<sol-svg-sprite-icon>`. Update inputs/attributes to the SOL equivalents and remove any sprite-path assumptions per SOL API.
- E2E discipline: do NOT wrap Playwright locators in try/catch; let failures surface.

CLEANUP (REQUIRED)
- Remove `@niceltd/cxone-components` and `@niceltd/cxone-domain-components` from package.json and code.
- Remove any imports from package.json and code for migrated Breeze components to new SOL components.
- Perform a THOROUGH search ensuring NO remaining imports from `@niceltd/cxone-components`.

TEST COMMANDS
- Unit (all): `npm run test`
- Unit (single): `npm test -- --include="**/<my-test-file>.spec.ts"`
- E2E (all): `npx playwright test`  (or the repo’s `npm run e2e` if defined)
- E2E (filtered): `npx playwright test --config=playwright/sm/suite01/suite01.config.ts`
- Expectation: all tests pass post-migration; list exact fixes required if deltas break tests.
```
8. **New chat.**
9. `@pm create-doc prd`
10. **New chat.**
11. `@architect create-doc architecture`
12. **New chat.**
13. `@po execute-checklist-po`

If needed, write in the chat **Do NOT design rollback strategies**

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

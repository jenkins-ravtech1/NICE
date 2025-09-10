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
Q: Enter the full path to your project directory where BMad should be installed
A: Type . and Enter

Q: Select what to install/update (use space to select, enter to continue): (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to proceed)
A: Press Enter

Q: Will the PRD (Product Requirements Document) be sharded into multiple files?
A: Type y and press Enter

Q: Will the architecture documentation be sharded into multiple files?
A: Type y and press Enter

Q: Which IDE(s) do you want to configure? (Select with SPACEBAR, confirm with ENTER): (Press <space> to select, <a>
to toggle all, <i> to invert selection, and <enter> to proceed)
A: Search for GitHub Copilot and Press Enter

Q: How would you like to configure GitHub Copilot settings?
A: Copilot config: **Recommended defaults**

Q: Would you like to include pre-built web bundles? (standalone files for ChatGPT, Claude, Gemini)
A: Type n and press Enter
```
5. Optional => Run: `npm install -g @kayvan/markdown-tree-parser`
6. Open the Copilot toggle Chat.
Click on the settings icon, and choose the Generate Copilot instructions option.

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
Angular MFE Migration: Breeze to SOL Components

PROJECT OVERVIEW
Objective: Complete migration of the current Angular Micro Frontend (MFE) application from Breeze component library to SOL component library.

CRITICAL PREREQUISITES
Before writing ANY code, you MUST:
1. Read and analyze the sol-components folder (located in the root project folder) containing official migration documentation
2. Study thoroughly the Sol-Migration-Notes folder (located in the root project folder) containing developer notes, decisions, and gotchas from previous manual SOL migrations

SCOPE & BOUNDARIES

IN SCOPE:
- Client-side only: All Angular components, templates, styles, and client-side logic
- Component migration: Complete replacement of Breeze components with SOL equivalents
- Test updates: Minimal changes to existing tests necessitated by component migration
- Import cleanup: Full removal of deprecated Breeze dependencies

OUT OF SCOPE (STRICT):
- Backend services, APIs, endpoints, DTOs, auth flows, or infrastructure
- New features, enhancements, or functionality additions
- New test coverage beyond migration-required updates
- Performance optimization beyond SOL's built-in improvements
- Rollback strategies, feature flags, or dual-running modes
- Full accessibility audits (rely on SOL defaults)
- KPIs, budgets, timelines, or process documentation

INVARIANTS (DO NOT MODIFY)
1. Module Federation architecture must remain unchanged
2. Backend APIs treated as fixed external contracts
3. Business logic preservation required

MIGRATION RULES & REQUIREMENTS

1. Component Mapping Documentation
Create a comprehensive Component Mapping Table in Markdown format:

| Breeze Component | SOL Replacement | Inputs (old → new) | Outputs/Events (old → new) | Template Changes (concise snippet) | Test Impact (selectors/flows) | Complexity (L/M/H) |

Requirements:
- MUST include ALL cxone components affected by migration
- MUST verify completeness - no Breeze component can be missed

2. Hard Replacement Rules

Translation Module:
REMOVE: import { TranslationModule } from '@niceltd/cxone-components/translation'
REPLACE WITH: import { TranslationModule } from '@niceltd/cxone-core-services'

Button Components:
- ALL button elements must migrate: <button> → <sol-button>
- Event handler: (click) → (buttonClick)
  Before: <button (click)="handleClick()">
  After: <sol-button (buttonClick)="handleClick()">

Icon Components:
REMOVE: <cxone-svg-sprite-icon>
REPLACE WITH: <sol-icon> (NOT sol-svg-sprite-icon)
- Update all inputs/attributes per SOL API documentation
- Remove sprite-path assumptions

Dropdown Components:
- Single-Select Breeze → SOL Dropdown with appropriate configuration
- Multi-Select Breeze → SOL Dropdown with multi-select enabled
- Map all inputs/outputs correctly

Toaster Service:
import { ToastrManagerModule } from '@niceltd/sol/toastr';

Spinner Components:
If you need a spinner that turns on and off automatically when network requests start and finish, you should use the App Spinner in the CXone Domain Components.

3. Style Configuration
Update angular.json at path projects/<YOUR_APP>/architect/build/options/styles:
"styles": [
  "node_modules/@niceltd/sol/src/styles/typefaces.css",
  "node_modules/@niceltd/sol/src/styles/sol-core.scss",
  ... other styles
]

4. Import Cleanup Requirements

Alias Removal (MANDATORY):
REMOVE aliases: import { CheckboxModule as SolCheckboxModule } from '@niceltd/sol/checkbox'
USE direct imports: import { CheckboxModule } from '@niceltd/sol/checkbox'

Package Cleanup - Remove from package.json and all code:
- @niceltd/cxone-components
- Any Breeze-specific dependencies

5. Testing Discipline
- E2E tests: NO try/catch wrapping of Playwright locators
- Let test failures surface naturally for debugging
- Update selectors and flows only as required by component changes

VERIFICATION CHECKLIST

Pre-Migration:
- sol-components folder documentation reviewed
- Sol-Migration-Notes folder analyzed
- All Breeze components identified and mapped

During Migration:
- Component mapping table created and complete
- All Breeze imports replaced
- All button events updated to (buttonClick)
- All icons migrated to <sol-icon>
- Translation module source updated
- Styles configuration updated in angular.json
- Import aliases removed

Post-Migration:
- ZERO imports from @niceltd/cxone-components (perform exhaustive search)
- All tests updated and passing
- No Breeze components remaining in codebase
- Package.json cleaned of deprecated dependencies

SEARCH COMMANDS FOR VERIFICATION
Use these to ensure complete migration:

Find any remaining Breeze imports:
grep -r "@niceltd/cxone-components" --include="*.ts" --include="*.html"

Find any remaining cxone- prefixed components in templates:
grep -r "<cxone-" --include="*.html"

Find any (click) events that should be (buttonClick):
grep -r "sol-button.*\(click\)" --include="*.html"

FINAL NOTES
- Completeness is critical: Every single Breeze component must be accounted for
- No partial migrations: Components are either fully migrated or not touched
- Backend is untouchable: Treat all backend services as black boxes with fixed contracts
- Documentation focus: The Component Mapping Table is the primary deliverable
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

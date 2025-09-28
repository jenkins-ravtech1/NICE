# AI Workflow

## Pre-planning
Run the app, the unit tests and the e2e to check if all works well.

## Planning Phase Steps

1. Clone **3 repositories**.
   - your app repository
   - sol-components: https://github.com/nice-cxone/sol-components.git  
   - Sol-Migration-Notes: https://github.com/jenkins-ravtech1/Sol-Migration-Notes.git
2. In VS Code, open your application folder. Then click on Add Folder to Workspace and select the two other folders. After that, you can save the workspace to your disk (make sure to save it alongside your application folder, not inside it).
3. Check if in your package.json these libraries have updated to **at least** the below versions:
```
"@niceltd/cxone-domain-components": "^30.3.0",
"@niceltd/cxone-client-platform-services": "^33.5.0",
"@niceltd/cxone-core-services": "^27.7.0"
```
If you have the 
```cxone-admin-library-components```
library installed, it must be at least version 8.90.0.

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

FILE ORGANIZATION
All migration documentation and internal files created during this migration process MUST be stored in the docs folder.

CRITICAL PREREQUISITES
Before writing ANY code, you MUST:
1. Study thoroughly the sol-components folder (located in the root project folder) containing official migration documentation
2. Study thoroughly the Sol-Migration-Notes folder (located in the root project folder)mcontaining several markdown (.md) files documenting developer notes, migration decisions, pitfalls, and component-specific patterns from previous manual SOL migrations. Note that these files do not cover the full list of SOL components — only a subset. Exception: Do NOT read AI-Workflow.md

SCOPE & BOUNDARIES

IN SCOPE:
- Client-side only: All Angular components, templates, styles, and client-side logic
- Component migration: Complete replacement of Breeze components with SOL equivalents
- Test updates: Minimal changes to existing tests necessitated by component migration
- Import cleanup: Full removal of deprecated Breeze dependencies

OUT OF SCOPE (STRICT):
- Backend services, APIs, endpoints, DTOs, auth flows, or infrastructure
- New features, enhancements, or functionality additions
- You must NEVER introduce components that did not exist in the original codebase
- New test coverage beyond migration-required updates
- Performance optimization beyond SOL's built-in improvements
- Rollback strategies, feature flags, or dual-running modes
- Full accessibility audits (rely on SOL defaults)
- KPIs, budgets, timelines, or process documentation

INVARIANTS (DO NOT MODIFY)
1. Module Federation architecture must remain unchanged
2. Backend APIs treated as fixed external contracts
3. Business logic preservation required
4. Component count must remain the same - no new components should be created

MIGRATION RULES & REQUIREMENTS

1. Component Mapping Documentation
Create a detailed Component Mapping Table in Markdown with the following columns: Breeze Component | SOL Replacement | Complexity (L/M/H)
This step is MANDATORY and must be included in the brief.md file during the create-project-brief phase

Requirements:
- MUST include ALL cxone components affected by migration
- MUST verify completeness - no Breeze component can be missed
- MUST follow patterns documented in sol-components AND Sol-Migration-Notes repositories
- MUST NOT introduce any new components that didn't exist in the original codebase

2. Component-Specific Migration Patterns
ALWAYS consult the sol-components and the Sol-migration-notes repositories for:
- Detailed migration patterns for each component type
- Before/After code examples
- Edge cases and gotchas
- Specific rules for each SOL component

3. Style Configuration (PRIORITY: Complete in First Story/Sprint)
IMPORTANT: This step MUST be completed at the beginning of the development process as the first story of the migration

Update angular.json at path projects/YOUR_APP/architect/build/options/styles:
"styles": [
  "node_modules/@niceltd/sol/src/styles/typefaces.css",
  "node_modules/@niceltd/sol/src/styles/sol-core.scss",
  ... other styles
]

4. Import Cleanup Requirements

Alias Removal (MANDATORY):
- REMOVE aliases: import { CheckboxModule as SolCheckboxModule } from '@niceltd/sol/checkbox'
- USE direct imports: import { CheckboxModule } from '@niceltd/sol/checkbox'

Package Cleanup - Remove from package.json and all code:
- @niceltd/cxone-components
- Any Breeze-specific dependencies
- IMPORTANT: If there are Tagify or Tiles Breeze component, don't remove the @niceltd/cxone-components because there components have not equivalent in SOL

Import Removal (MANDATORY):
- REMOVE: import { NavigationModule } from '@niceltd/cxone-domain-components/navigation'
- This import must be completely eliminated from all files

5. Testing Discipline
- E2E tests: NO try/catch wrapping of Playwright locators
- Let test failures surface naturally for debugging
- Update selectors and flows only as required by component changes

VERIFICATION CHECKLIST

Pre-Migration:
- sol-components folder documentation reviewed
- Sol-Migration-Notes folder and ALL its markdown files analyzed
- Angular.json styles configuration updated (FIRST STORY)
- All Breeze components identified and mapped
- Verified no new components will be added

During Migration:
- Component mapping table created and complete
- All Breeze imports replaced following the sol-components repository and Sol-Migration-Notes guidelines
- All migrations follow patterns documented in sol-components repository and Sol-Migration-Notes markdown files
- Translation module source updated
- Navigation module imports REMOVED
- Styles configuration updated in angular.json
- Import aliases removed
- No new components introduced

Post-Migration:
- ZERO imports from @niceltd/cxone-components (perform an exhaustive search), except for the Tiles and Tagify components.- ZERO NavigationModule imports from @niceltd/cxone-domain-components/navigation
- All tests updated and passing
- No Breeze components remaining in codebase
- Package.json cleaned of deprecated dependencies
- All sol-components repository guidelines and Sol-Migration-Notes guidelines followed completely
- Component count remains the same as before migration

SEARCH COMMANDS FOR VERIFICATION
Use these to ensure complete migration (Windows Command Prompt):

Find any remaining Breeze imports:
findstr /s /i "@niceltd/cxone-components" *.ts *.html

Find any remaining cxone- prefixed components in templates:
findstr /s /i "<cxone-" *.html

Find buttons with cxone-btn class that need migration:
findstr /s /i "button.*cxone-btn" *.html

Find any (click) events on sol-button that should be (buttonClick):
findstr /s /i "sol-button.*(click)" *.html

Find any remaining NavigationModule imports (MUST return zero results):
findstr /s /i "NavigationModule.*@niceltd/cxone-domain-components/navigation" *.ts

FINAL NOTES
- Documentation is key: Always refer to the sol-components and Sol-Migration-Notes repositories.
- Completeness is critical: Every single Breeze component must be accounted for
- No new components: The migration should ONLY replace existing components, never add new ones
- No partial migrations: Components are either fully migrated or not touched
- Backend is untouchable: Treat all backend services as black boxes with fixed contracts
- NavigationModule elimination: This import must be completely removed from the codebase
- Button migration scope: ONLY button elements with cxone-btn class require migration to sol-button
- Documentation focus: The Component Mapping Table, the sol-components repository and Sol-Migration-Notes guidelines are the primary references
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
2. `@sm create-next-story` (optional, type the story number, for example `@sm create-next-story 2.1`)
3. READ the story content.
4. Change status to **Approved**.
5. **New chat.**
6. `@dev implement-story` (optional, type the story number, for example `@dev implement-story 2.1`)
7. In the same chat, write the below prompt (only applicable for stories that performed component migration):

       Perform a thorough search and confirm that all instances of the component have been fully migrated.

8. Check in the story `.md` file that **all tasks are checked**.
9. **The status of the story must be Ready For Review** _OR_ write:

       If the status of the story is Ready For Review, update it to Ready For Review.
10. **New chat.**
11. `@qa review-story` (optional, type the story number, for example `@qa review-story 2.1`)
12. **IMPORTANT** Check that all unit tests passed successfully.
13. **The status of the story must be Done** _OR_ write:

        If the status of the story is Done, update it to Done.

14. **IMPORTANT** Commit your work before starting a new story.
15. Go back to step 1 to start the next story.

## Notes — Out of AI Scope

1. **Do not** update components in bulk with **PowerShell** (files can become corrupted). Proceed **one by one**.
2. Update `@niceltd/cxone-client-platform-services` **and** `@niceltd/cxone-core-services` **before** removing `@niceltd/cxone-components`.

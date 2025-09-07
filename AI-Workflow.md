# Planning Phase – AI Agents

## Repositories

- sol-components: https://github.com/nice-cxone/sol-components.git
- NICE: https://github.com/jenkins-ravtech1/NICE

## Planning Phase Steps

1. Clone **3 repos** (the app repo, the `sol-components` repo, the **NICE** repo).  
   - https://github.com/nice-cxone/sol-components.git  
   - https://github.com/jenkins-ravtech1/NICE
2. Open a new **VS Code workspace** which includes the 3 repositories.
3. In your app repo, run: `npx bmad-method install`
4. Run: `npm install -g @kayvan/markdown-tree-parser`
5. **Generate Copilot instructions.**
6. `@analyst create-project-brief prompt`
7. **New chat.**
8. `@pm create-doc prd yolo mode`
9. **New chat.**
10. `@architect create-doc architecture yolo mode`
11. **New chat.**
12. `@po execute-checklist-po yolo mode`
13. **New chat.**
14. `@po shard docs/prd.md to prd`
15. **New chat.**

## Development Workflow (IDE Only)

1. **New chat.**
2. `@sm create-next-story <STORY_NUMBER>`
3. Check the story content.
4. Change status to **Approved**.
5. **New chat.**
6. `@dev implement-story <STORY_NUMBER>`
7. In the same chat, write: “perform a thorough search and confirm that all instances of the component have been fully migrated”
8. Check in the story `.md` file that **all tasks are checked**.
9. **New chat.**
10. `@qa review-story <STORY_NUMBER>`
11. **All unit tests must pass.**
12. **The status of the story must be Done** _OR_ write: “If the status of the story is Done, update it to Done.”
13. `@dev commit-work` **OR** commit your work manually (sometimes `git status` shows files that must **not** be pushed).

## Notes — Out of AI Scope

1. **Do not** update components in bulk with **PowerShell** (files can become corrupted). Proceed **one by one**.
2. Update `@niceltd/cxone-client-platform-services` **and** `@niceltd/cxone-core-services` **before** removing `@niceltd/cxone-components`.

## References

- sol-components: https://github.com/nice-cxone/sol-components.git
- NICE: https://github.com/jenkins-ravtech1/NICE
- AI brief prompt (SharePoint): https://niceonline.sharepoint.com/:t:/r/teams/SOLMIGRATION/Shared%20Documents/Developers/AI/AI%20brief%20prompt.txt?csf=1&web=1&e=XaXr72

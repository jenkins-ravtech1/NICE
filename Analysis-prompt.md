# AI Coding Assistant Prompt: CXone (Breeze) to SOL Component Migration Analysis

## Task Overview

Analyze the codebase to identify and plan the migration of CXone (Breeze) components to SOL components. Provide comprehensive analysis tables and effort estimates for the migration project.

## Analysis Requirements

### 1. CXone (Breeze) Components Inventory

**Task:** Identify all CXone (Breeze) components currently in use throughout the application.

**Output:** Create a summary table with the following structure:

| Component Type | Number of Occurrences | Number of Files Affected |
|---------------|----------------------|-------------------------|
| [Component]   | [Count]              | [File Count]           |

**Instructions:**
- Scan all source files for Breeze component imports and usage
- Group by component type (e.g., Button, Modal, Form, etc.)
- Count total instances and unique files containing each component

### 2. Deprecated Breeze Components Analysis

**Task:** Identify deprecated Breeze components as specified in `Deprecated-components.md`

**Output:** Create a table with the following structure:

| Breeze Deprecated Component Name | File Path |
|--------------------------------|-----------|
| [Component Name]               | [Path]    |

**Instructions:**
- Cross-reference with the deprecation list in `Deprecated-components.md`
- List all file locations using deprecated components
- Flag high-priority migrations

### 3. E2E Test Impact Analysis

**Task:** Identify all E2E test suites affected by the component migration.

**Output:** Create a comprehensive table with the following structure:

| Test Name | Test File Name | Tested Components |
|-----------|---------------|------------------|
| [Test]    | [File]        | [Component List] |

**Instructions:**
- Search the entire workspace for E2E test files (likely in `__tests__`, `*.test.*`, `*.spec.*`, or `/e2e/` directories)
- Identify tests that interact with Breeze components
- List specific components being tested in each suite
- Prioritize tests requiring updates based on component usage

### 4. Identify 3rd party component UI libraries

**Task:** Identify all 3rd party component UI libs that are not from the cxone Breeze package.

**Output:** Create a comprehensive table with the following structure:

| Component Name | Package Name | File Name |
|-----------|---------------|------------------|
| [Component]   | [Package]     | [File]           |

**Instructions:**
- Search the application codebase for 3rd party component UI libs that are not from the cxone Breeze 

### 5. Migration Epics and Story Effort Estimates

**Task:** Based on the PRD document located at #file:prd.md  add the below requirements to this analysis:

**Estimation Assumptions:** Account for the following manual developer tasks per story:
- Component migration review and fixes (visual inspection of rendered web pages)
- E2E test suite review, execution, and fixes
- PR submission and peer review comment resolution

**Output Format:**
- Epic breakdown with user stories
- Story points or hour estimates per story
- Dependencies and risk assessment

## Additional Considerations

- Note any custom Breeze component extensions or wrappers
- Identify potential breaking changes in the migration
- Flag components with complex state management or custom styling
- Document any SOL components that don't have direct Breeze equivalents

## Output Format

Create a markdown file named: "Analysis-Results.md" with clear section headers and properly formatted tables for easy tracking and project management integration.
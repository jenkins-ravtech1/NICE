---
title: Grid
---

# E2E

## Issue

When running E2E tests that check rows in the Sol-grid, you may
encounter an issue where some rows are not loaded due to lazy loading
behavior.

## Solution

To ensure all necessary rows are loaded for your tests, you can adjust
the `rowBuffer` property in the grid options. This increases the number of
additional rows loaded outside the current viewport.

**Tip:** Start with a small value for `rowBuffer` and gradually increase
it until all required rows are available during test execution.

### Warning

Increasing the number of loaded rows may lead to high memory usage,
which can cause Chromium to run out of memory.

To mitigate this, you can increase the available memory for Chromium by
adding the following configuration to your test suite setup (e.g., in
your `suite.config.ts`):

```typescript
config.use.launchOptions = {
  // Add this when running tests locally
  args: ["--js-flags=--max-old-space-size=4096"],
};
```

**Note:** This configuration is typically needed only when running tests
locally. It's recommended to comment it out by default and enable it
manually when necessary.

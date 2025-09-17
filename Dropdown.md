# CXOne vs SOL Dropdown Sorting Implementation

## Overview

This document outlines the differences between CXOne library dropdowns and SOL dropdowns regarding sorting functionality.

## CXOne Library Dropdowns

The CXOne library provides built-in `sortOrder` parameter support for both single and multi-select dropdowns:

### Single Select Dropdown
```html
<cxone-singleselect-dropdown [sortOrder]="..." />
```

### Multi Select Dropdown  
```html
<cxone-multiselect-dropdown [sortOrder]="..." />
```

## SOL Dropdown Limitation

**Important:** The `sortOrder` parameter does **NOT** exist in SOL dropdown components.

## SOL Dropdown Sorting Solution

If you need to return a sorted list when using SOL dropdowns, you **MUST** sort the list manually in the TypeScript file before returning it.

### Example Implementation

```typescript
this.languageList$.subscribe(resp => {
  this.languageList = resp.sort((a, b) =>
    a.name > b.name ? 1 : b.name > a.name ? -1 : 0
  );
});
```

## Key Takeaway

- **CXOne dropdowns**: Use built-in `[sortOrder]` parameter
- **SOL dropdowns**: Manually sort data in TypeScript before binding to the dropdown
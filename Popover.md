# Sol Popover Migration Guide

## Component Selection

When migrating from `<div [popover]="popoverTemplate"`, choose the appropriate Sol component:

- **Tooltip**: Use `@niceltd/sol/tooltip` when the popover opens only a **tooltip** on click
- **Floating Menu**: Use `@niceltd/sol/floating-menu` when it opens a **menu** (like a more actions menu)

## Floating Menu Implementation

### Required Imports

Import the necessary modules:

```typescript
import { FloatingMenuModule } from "@niceltd/sol/floating-menu";
import { MatMenuModule } from "@angular/material/menu";
```

Import the MenuItem interface:

```typescript
import { MenuItem } from "@niceltd/sol/floating-menu/src/lib/menu-items";
```

### HTML Template

```html
<sol-button
  class="menu-button"
  variant="basic"
  size="large"
  [icon]="{
        icon: spritePath + '#icon-more',
        height: '20px',
        width: '20px'
    }"
  #t="matMenuTrigger"
  (click)="openMenuPopover()"
  [matMenuTriggerFor]="menuTopBar.menu"
>
</sol-button>

<sol-floating-menu
  #menuTopBar="floatingMenu"
  [menuItems]="menuItems"
  [variant]="'narrow'"
></sol-floating-menu>
```

### Component Implementation

```typescript
menuItems: MenuItem[] = [];

initMenuItems() {
    this.menuItems = [
        {
            label: this.translationPipe.transform('intradayManager.topBar.menuItems.export'),
            callbackFn: (menuItem: MenuItem) => this.onMenuExportClicked(),
            showDivider: false
        },
        {
            label: this.widgetsCollapsed ? this.labelMenuExpandWidgets : this.labelMenuCollapseWidgets,
            callbackFn: (menuItem: MenuItem) => this.widgetsCollapsed ? this.onMenuExpandWidgetsClicked() : this.onMenuCollapseWidgetsClicked(),
            showDivider: true
        },
        {
            label: this.translationPipe.transform('intradayManager.topBar.menuItems.settings'),
            callbackFn: (menuItem: MenuItem) => this.disableSettingsMenu || this.onMenuSettingsClicked(),
            disabled: this.disableSettingsMenu,
            showDivider: false
        }
    ];
}

openMenuPopover() {
    this.menuItems[1].label = this.widgetsCollapsed ? this.labelMenuExpandWidgets : this.labelMenuCollapseWidgets;
}
```

## Key Features

- **Dynamic Labels**: Menu item labels can be updated dynamically based on component state
- **Callback Functions**: Each menu item can have its own callback function
- **Dividers**: Control divider display between menu items with `showDivider` property
- **Disabled State**: Menu items can be disabled conditionally
- **Variants**: Support for different menu variants (e.g., 'narrow')

## Migration Decision Tree

1. **Analyze your current popover behavior**:
   - Shows simple text/info → Use `@niceltd/sol/tooltip`
   - Shows actionable menu items → Use `@niceltd/sol/floating-menu`

2. **Replace accordingly**:
   - `<div [popover]="popoverTemplate"` → `<sol-tooltip>` or `<sol-floating-menu>`

3. **Update imports and implementation** based on chosen component
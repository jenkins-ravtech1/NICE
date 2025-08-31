---
title: Sol Floating Menu
---

[Confluence for Menu
Component](https://nice-ce-cxone-prod.atlassian.net/wiki/spaces/WFM/pages/368776383/Design+Document+SOL+Floating+Menu+Component)

## Import

Import modules:

```typescript
import { FloatingMenuModule } from "@niceltd/sol/floating-menu";
import { MatMenuModule } from "@angular/material/menu";
```

Import MenuItem Interface:

```typescript
import { MenuItem } from "@niceltd/sol/floating-menu/src/lib/menu-items";
```

## Example:

In the HTML file:

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

In the component file:

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

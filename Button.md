# Sol Button Migration Guide

## Migration Rules

### Core Migration Principles

- **ONLY** migrate `<button>` elements that have class `cxone-btn`
- **ALL** button elements with `cxone-btn` class must migrate: `<button class="cxone-btn...">` → `<sol-button>`
- Event handler migration: `(click)` → `(buttonClick)`
- Translation syntax: The translate directive **MUST** use the pipe syntax `{{ 'key' | translate }}` and **NOT** the attribute syntax `translate="key"`
- Icon handling: If a `cxone-btn` contains an icon element, the icon becomes a parameter in `sol-button` using the `[icon]` input

### Icon Migration Pattern

- Icons inside buttons are **NOT** separate elements in SOL
- Use the `[icon]` input property with an object containing icon path and className
- Format: `[icon]="{icon: spritePath + '#icon-name', className: 'icon-class'}"`

## Migration Examples

### Example 1: Text-Only Button

**Before:**
```html
<button class="cxone-btn btn-medium btn-secondary" 
        (click)="clearSelectedColumns()" 
        translate="automaticApprovalRuleConfig.clearSelectedButtonLabel">
</button>
```

**After:**
```html
<sol-button (buttonClick)="clearSelectedColumns()">
  {{ 'automaticApprovalRuleConfig.clearSelectedButtonLabel' | translate }}
</sol-button>
```

### Example 2: Button with Icon

**Before:**
```html
<button container="body" #moreActions="bs-popover"
        containerClass="role-popovers cxone-tooltip-container cxone-popover" 
        placement="bottom"
        outsideClick="true" 
        class="cxone-btn btn btn-secondary btn-default-my-schedule more more-settings-button"
        [popover]="moreSettings" 
        [disabled]="initiateScheduleData.isDirtyMode"
        [attr.aria-label]="'mySchedule.actionButton' | translate">
  <cxone-svg-sprite-icon iconName="icon-more" [spritePath]="spritePath" wrapperClass="more-icon">
  </cxone-svg-sprite-icon>
</button>
```

**After:**
```html
<sol-button container="body" #moreActions="bs-popover"
            containerClass="role-popovers cxone-tooltip-container cxone-popover" 
            placement="bottom"
            outsideClick="true" 
            variant="basic"
            [popover]="moreSettings" 
            [disabled]="initiateScheduleData.isDirtyMode"
            [ariaLabel]="'mySchedule.actionButton' | translate" 
            [icon]="{icon: spritePath + '#icon-more', className: 'more-icon'}">
</sol-button>
```

### Key Changes for Buttons with Icons

- Remove nested `<cxone-svg-sprite-icon>` element
- Add `[icon]` input with object containing icon path and className
- Convert `[attr.aria-label]` to `[ariaLabel]`
- Maintain all other attributes and bindings
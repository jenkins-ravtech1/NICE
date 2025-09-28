# Sol Icon Migration Guide

## Migration Rules

### Core Migration Principles
- **REMOVE:** `<cxone-svg-sprite-icon>`
- **REPLACE WITH:** `<sol-icon>` (NOT `sol-svg-sprite-icon`)
- Update all inputs/attributes per SOL API documentation
- Remove sprite-path assumptions

### Migration Pattern

**Before (Breeze):**
```html
<cxone-svg-sprite-icon 
  iconName="icon-name" 
  [spritePath]="spritePath" 
  wrapperClass="custom-class">
</cxone-svg-sprite-icon>
```

**After (SOL):**
```html
<sol-icon 
  [icon]="'icon-name'" 
  [className]="'custom-class'">
</sol-icon>
```

### Key Changes
- Component name: `cxone-svg-sprite-icon` → `sol-icon`
- Icon reference: `iconName` attribute → `[icon]` input
- Sprite path handling: Remove `[spritePath]` - SOL handles this internally
- CSS classes: 
  - **Breeze:** `wrapperClass` attribute
  - **SOL:** `[className]` input (NOT `class` or `[class]`)

### Important Notes
- Do NOT use `sol-svg-sprite-icon` - the correct component is `sol-icon`
- Sprite path management is handled internally by SOL components
- **Class attribute:** Use `[className]` for SOL (different from standard HTML `class` attribute)
- Consult SOL API documentation for additional icon properties and configurations
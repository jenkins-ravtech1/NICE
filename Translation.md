# Sol Translation Migration Guide

## Migration Rules

### Module Import Migration

**REMOVE:**
```typescript
import { TranslationModule } from '@niceltd/cxone-components/translation'
```

**REPLACE WITH:**
```typescript
import { TranslationModule } from '@niceltd/cxone-core-services'
```

### Key Changes

- Translation module has been moved from `@niceltd/cxone-components/translation` to `@niceltd/cxone-core-services`
- No changes to the module name (`TranslationModule`)
- No changes to how the module is used in imports array
- Translation pipe and service usage remains the same

### Important Notes

- This is a simple import path change only
- All translation functionality remains unchanged
- Existing translation keys and translation files are not affected
- The `translate` pipe syntax continues to work as before
# Sol Toaster Migration Guide

## Migration Rules

### Module Import Migration

**Import Statement:**
```typescript
import { ToastrManagerModule } from '@niceltd/sol/toastr';
```

### Module Setup

Add `ToastrManagerModule` to your module imports:

```typescript
@NgModule({
  imports: [
    // ... other imports
    ToastrManagerModule
  ]
})
export class YourModule { }
```

### Service Usage

```typescript
import { ToastrService } from '@niceltd/sol/toastr';

constructor(private toastrService: ToastrService) {}

// Usage examples
showSuccess() {
  this.toastrService.success('Success message', 'Title');
}

showError() {
  this.toastrService.error('Error message', 'Title');
}

showWarning() {
  this.toastrService.warning('Warning message', 'Title');
}

showInfo() {
  this.toastrService.info('Info message', 'Title');
}
```

### Key Changes

- Toaster functionality is now provided through SOL's `ToastrManagerModule`
- Import path: `@niceltd/sol/toastr`
- Service and methods remain similar to standard toaster implementations

### Important Notes

- Ensure SOL toaster styles are properly loaded in your application
- Check SOL documentation for advanced configuration options
- Previous toaster service imports should be removed and replaced with SOL toaster
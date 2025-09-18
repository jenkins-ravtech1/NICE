# Sol Spinner Migration Guide

## Migration Rules

### Core Migration Principles

If you need a spinner that turns on and off automatically when network requests start and finish, you should use the **App Spinner** in the **CXone Domain Components**.

### Usage Recommendation

- **Use Case:** Spinners that need to automatically activate/deactivate based on network activity
- **Component:** App Spinner from CXone Domain Components
- **Benefit:** Automatic management of spinner state tied to HTTP requests

### Import Statement

```typescript
import { AppSpinnerComponent } from '@niceltd/cxone-domain-components';
```

### Key Features

- Automatically shows during network requests
- Automatically hides when requests complete
- No manual state management required
- Integrated with CXone application's HTTP interceptors

### Important Notes

- For spinners that require manual control, consult SOL documentation for appropriate spinner components
- App Spinner is specifically designed for network request indicators
- Ensure CXone Domain Components module is properly imported in your module
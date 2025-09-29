# Sol Translation Migration Guide

## Migration Rules

### Core Migration Principles
- **GOAL:** Enable SOL component internal text translations (e.g., "All", "Clear All" in dropdowns)
- **PRESERVE:** Existing application translations that already work
- **INTEGRATE:** SOL translation system with existing CXone localization
- Follow official SOL documentation for CXone app integration

### Migration Pattern

**Problem:**
```html
<!-- SOL components showing untranslated text -->
<sol-dropdown>
  <!-- Shows "All" and "Clear All" in English only -->
</sol-dropdown>
```

**Solution:**
```typescript
// SOL components automatically show translated text
<sol-dropdown>
  <!-- Shows "Tous" and "Effacer tout" in French -->
</sol-dropdown>
```

### Required Changes

#### 1. Update angular.json (Asset Configuration)
```json
{
  "assets": [
    {
      "glob": "**/*.json",
      "input": "node_modules/@niceltd/sol/src/assets/strings",
      "output": "assets/strings/components/sol-components"
    }
  ]
}
```

#### 2. Update languageMetaData.json
**Before:**
```json
{
  "cxoneComponents": [
    "cxone-core-components/strings_en_US.json"
  ]
}
```

**After:**
```json
{
  "cxoneComponents": [
    "@niceltd/cxone-core-components/strings_en_US.json",
    "@niceltd/strings_en_US.json"
  ]
}
```

#### 3. Update app.module.ts
```typescript
import { localeInitializerProvider } from '@niceltd/sol/translation';

@NgModule({
  providers: [
    localeInitializerProvider, // Add SOL translation initialization
    // ... other providers
  ],
})
```

#### 4. Update localization.service.ts
```typescript
import { TranslationService } from '@niceltd/sol/translation';

@Injectable({ providedIn: 'root' })
export class LocalizationService {
  constructor(
    private i18next: I18NextService,
    private configService: ConfigurationService,
    private cxoneLocalizationInitializer: LocalizationInitializer,
    private solTranslationService: TranslationService // Add SOL service
  ) { }

  load(): Promise<void> {
    return new Promise((Resolve, X) => {
      // ... existing localization code ...
      
      this.cxoneLocalizationInitializer.load()
        .then(_ => {
          console.log("localization load complete at:", new Date().toUTCString());
          this.initializeSolTranslations(); // Initialize SOL translations
          Resolve();
        }).catch();
    });
  }

  /**
   * Initialise les traductions SOL avec les données chargées depuis le serveur
   */
  private initializeSolTranslations(): void {
    try {
      console.log('Initializing SOL translations...');
      
      // Use official SOL method as documented
      if (this.cxoneLocalizationInitializer && typeof this.cxoneLocalizationInitializer.getSolTranslations === 'function') {
        const solTranslations = this.cxoneLocalizationInitializer.getSolTranslations();
        
        if (solTranslations) {
          this.solTranslationService.setTranslations(solTranslations);
          console.log('SOL translations initialized successfully:', solTranslations);
        } else {
          console.warn('getSolTranslations() returned null or undefined');
        }
      } else {
        console.warn('getSolTranslations method not available on LocalizationInitializer');
      }
    } catch (error) {
      console.error('Error initializing SOL translations:', error);
    }
  }
}
```

### Key Translation Keys
SOL components use these translation keys internally:
- `clearAll` - "Clear All" button in dropdowns
- `selectAll` - "All" option in dropdowns  
- `clear` - "Clear" buttons
- `searchHere` - Search placeholders
- `noResult` - No results messages
- `loading` - Loading states
- And many more defined in the `Translations` interface

### Important Notes
- **CRITICAL:** Use `localizationInitializer.getSolTranslations()` method (requires @niceltd/cxone-core-services version 24.6+)
- **DO NOT:** Try to inject SOL services directly in app.component.ts constructor - use the localization service pattern
- **DO NOT:** Modify the main translation system - only extend it for SOL
- **PRESERVE:** All existing application translations must continue working
- **TIMING:** Initialize SOL translations AFTER the main localization load completes

### Testing Verification
1. Check console for "SOL translations initialized successfully" message
2. Verify SOL dropdown components show translated "All" and "Clear All" texts
3. Confirm existing application translations still work (`{{ 'textToSpeechApp.addNewTTS' | translate }}`)
4. Test in different languages if supported

### Troubleshooting
- **No translation:** Check if `getSolTranslations()` method exists on LocalizationInitializer
- **Console errors:** Verify @niceltd/cxone-core-services version is 24.6+
- **App translations broken:** Ensure SOL initialization happens AFTER main localization load
- **Missing assets:** Verify angular.json asset configuration is correct

### References
- SOL Translation Documentation: Storybook > Introduction > Translations > CXone Apps
- `@niceltd/sol/translation` - Translation module and service
- `translations.ts` - Interface defining all available translation keys
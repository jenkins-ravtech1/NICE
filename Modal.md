# Sol Modal Migration Guide

## General Changes

### Dialog Library Migration

In `component.ts`, replace the following imports and services:

**Before (PrimeNG):**
- `DynamicDialogConfig` 
- `DynamicDialogRef`
- `DialogService`

**After (Sol):**
- `MAT_DIALOG_DATA` from Angular Material
- `ModalService` from Sol

### Constructor Update

Replace the constructor with:

```typescript
constructor(
    private modalService: ModalService,
    @Inject(MAT_DIALOG_DATA) public data: DataTypeExample
) { }
```

### Property Changes

**Remove Height Property:** The `ModalService` does not support the `height` property, so remove it if it exists.

### Modal Implementation

When calling the modal, subscribe to the reference:

```typescript
const ref = this.modalService.open(ModalComponent, {
    width: '600px',
    data: {}
});

ref.subscribe((param: boolean) => {
    // Your code here
});
```

## Message Modal

Since Sol does not have a native message modal, a custom `MessageModalComponent` was created to maintain similar functionality.

### Implementation Options

#### Option 1: Multiple Instances

If you have **multiple** instances of the message modal within your application, use the following component:

```typescript
import { Component, Inject } from '@angular/core';
import { MAT_DIALOG_DATA } from '@angular/material/dialog';
import { ModalService } from '@niceltd/sol/modal';

interface MessageModalData {
  title?: string;
  message?: string;
  warnButtonLabel?: string;
  basicButtonLabel?: string;
  primaryButtonLabel?: string;
  type?: string;
  allowClose?: boolean;
}

@Component({
  template: `
     <sol-modal
      class="sol-message-modal"
      [title]="modalData?.title ?? ''"
      [message]="modalData?.message ?? ''"
      [basicButtonLabel]="modalData?.basicButtonLabel ?? ''"
      [primaryButtonLabel]="modalData?.primaryButtonLabel ?? ''"
      [warnButtonLabel]="modalData?.warnButtonLabel ?? ''"
      [type]="modalData?.type ?? ''"
      [allowClose]="modalData?.allowClose ?? true"
      (dismissed)="onDismiss()"
      (closed)="onClose()"
    ></sol-modal>
  `
})
export class MessageModalComponent {
  modalData: MessageModalData;
  
  constructor(
    private modalService: ModalService,
    @Inject(MAT_DIALOG_DATA) data: MessageModalData
  ) {
    this.modalData = data;
  }
  
  onDismiss() {
    this.modalService.close(false);
  }
  
  onClose() {
    this.modalService.close(true);
  }
}
```

#### Option 2: Single Instance

If there is **only one** instance, implement with a template reference:

**Template:**
```html
<ng-template #dialogRef>
  <sol-modal
    [title]="'Standard Modal'"
    [primaryButtonLabel]="'Confirm'"
    [basicButtonLabel]="'Cancel'"
    (closed)="onClose()"
    (dismissed)="onDismiss()"
  >
    <sol-modal-body>
      <p class="common-text">Content inside templateRef.</p>
    </sol-modal-body>
  </sol-modal>
</ng-template>
<sol-button (click)="openTemplateRef()">Open TemplateRef</sol-button>
```

**Component:**
```typescript
class LaunchTemplateRefComponent {
  @ViewChild('dialogRef')
  dialogRef!: TemplateRef<any>;

  constructor(private solModalService: ModalService) {}

  openTemplateRef() {
    this.solModalService.open(this.dialogRef, {
      width: '420px'
    });
  }

  onDismiss() {
    this.solModalService.close(false);
    console.log('modal dismissed');
  }
  
  onClose() {
    this.solModalService.close(true);
    console.log('modal closed');
  }
}
```

## Key Takeaways

- Replace PrimeNG dialog services with Sol's `ModalService`
- Update constructor to use Angular Material's `MAT_DIALOG_DATA`
- Remove unsupported `height` property
- Choose appropriate message modal implementation based on usage frequency
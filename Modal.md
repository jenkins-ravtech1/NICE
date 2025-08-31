---
title: Modal
---

# General Changes

- **Dialog Libraries**:

  In component.ts replace the following:

  - `DynamicDialogConfig` -> `MAT_DIALOG_DATA` from angular-material
  - `DynamicDialogRef` and `DialogService` -> `ModalService` from sol

- In the constructor replace to:

  ```typescript
  constructor(
      private modalService: ModalService,
      @Inject(MAT_DIALOG_DATA) public data: DataTypeExample
  ) { }
  ```

- **Height Property**:
  Remove `height` property if it exists, because the `ModalService` does not support it.

- In the function that call to the modal Subscribe from the ref.

  ```typescript
  const ref = this.modalService.open(ModalComponent, {
      width: '600px',
      data: {}
  });

  ref.subscribe((param: boolean) => {
      ...Your code
  });
  ```

# Message Modal

- Since Sol does not have a native message modal, a custom
  `MessageModalComponent` was created to maintain similar
  functionality.
  You can copy-paste it from this link:
  <https://github.com/nice-cxone/cxone-webapp-intraday/tree/CXWFM-53723-sol-migration/src/app/intraday-manager/components/message-modal>

**Note:** Use this **only if you have multiple** instances of the
message modal within your application. **If there is only one**
instance, apply with a template as shown here:
<https://na1.dev.nice-incontact.com/sol/?path=/docs/components-modal-overview--docs#custom-modal>.

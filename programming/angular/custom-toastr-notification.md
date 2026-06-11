# Custom Toastr notification



```typescript
  //------------------ New method to handle customer contact click ------------------
  customerContactClickHandler(weighingData: any): void {
    const customerName = weighingData?.customer || weighingData?.Customer;
    if (!customerName) {
      this.toastr.info('No customer name found for this record.', 'Info', {
        toastClass: 'custom-toastr-wide',
        timeOut: 5000
      });
      return;
    }

    const customerId = Number(weighingData?.customerId || weighingData?.CustomerId || 0);
    const showContactInfo = (contactList: any[]) => {
      const normalizedCustomerName = customerName.toString().trim().toLowerCase();
      const matchedContacts = (contactList || []).filter(
        (contact: any) =>
          (contact?.name || '').toString().trim().toLowerCase() === normalizedCustomerName
      );

      const selectedContact = matchedContacts.length > 0 ? matchedContacts[0] : null;
      const contactName = selectedContact?.name || 'N/A';
      const email = selectedContact?.email || 'N/A';

      this.toastr.warning(
        `Customer: ${customerName}\n Contact Name: ${contactName}\n Email: ${email}`,
        'Customer Contact',
        {
            toastClass: 'custom-toastr-wide',
        }
      );
    };

    this.contactService
      .getAllContacts()
      .pipe(takeUntil(this.destroy$))
      .subscribe({
        next: (contacts) => {
          showContactInfo(contacts);
        },
        error: () => {
          this.toastr.error('Failed to retrieve contact details.', 'Error');
        },
      });
  }

```

```scss
// Custom wider toastr notification
::ng-deep .custom-toastr-wide {
  width: 380px !important;
  background-color: #ffffff !important;
  max-width: 380px !important;
  padding: 10px !important;
  border-radius: 8px !important;}

::ng-deep .custom-toastr-wide .toast-message {
  line-height: 1.5 !important;
  background-color: #c6e8f6 !important;
  width: 360px !important;
  white-space: pre-line !important;
  word-break: break-word;
  padding: 20px !important;
  border-radius: 8px !important;
}
```

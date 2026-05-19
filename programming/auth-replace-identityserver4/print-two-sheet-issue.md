# Print() two sheet issue

{% code fullWidth="true" %}
```
// In MatDialog, print()

  print() {
    const dialogContent = document.querySelector('mat-dialog-container');
    const printWindow = window.open('', '', 'width=800,height=600');

    if (printWindow && dialogContent) {
      printWindow.document.write(`
      <html>
        <head>
          <title>Print docket</title>
          <style>
            body { margin: 0; font-family: Arial; }
            @media print {
              body { -webkit-print-color-adjust: exact; }
            }
          </style>
        </head>
        <body>
          ${dialogContent.innerHTML}
        </body>
      </html>
    `);
      printWindow.document.close();
      printWindow.focus();

      setTimeout(() => {
        printWindow.print();
        printWindow.close();
      }, 200);
    }
    // window.print();   //not use, use printWindow.print() replace
  }

```
{% endcode %}

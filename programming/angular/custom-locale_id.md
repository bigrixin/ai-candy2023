# Custom LOCALE\_ID



{% code fullWidth="true" %}
```
// Some.modules.ts

import { LOCALE_ID } from '@angular/core';

@NgModule({
  providers: [{provide: LOCALE_ID, useValue: 'en-AU' }]

})


```
{% endcode %}



{% code fullWidth="true" %}
```
// SomeComponent.ts

import { LOCALE_ID } from '@angular/core';



constructor(
    @Inject(LOCALE_ID) public locale: string,
 ){}


ngOnInit() {
   this.buildDtOptions(this.enableEdit, this.locale);
}



 buildDtOptions(edit: boolean, locale: any) {
        {
          data: 'gross', render: function (data: any, type: any, row: any) {
            return '<span style="color:SandyBrown">' + formatNumber(Number(row.gross), locale, '1.2-2') + '</span>'
          }
        },
        {
          data: 'weighDateTime', render: function (data: any, type: any, row: any) {
            return '<span style="color:SandyBrown">' +
              formatDate(((new Date(data)).setHours((new Date(data)).getHours() + (new Date(data).getTimezoneOffset()/-60))), 'dd/MM/yyyy hh:mm:ss a', locale)    // locale == 'en-AU'

          }
        },
}


```
{% endcode %}

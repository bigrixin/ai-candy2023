# Toastr - display multiline and position



{% code fullWidth="true" %}
```
// display multi line message in the toastr


::ng-deep #toast-container > div {
  width: 350px;
  white-space : pre-line !important;
}
  

infoDialog() {
      this.emailSettingService.getTimeZoneInfo()
      .subscribe({
        next: res => {
          let info = '';
          for(var index in res)
          {
            info = info +' \n ' + res[index];
              //console.log(res[index]);
          }
          this.toastr.info(info + ' \n\n');
          this.isLoading = false;
        },
        error: error => {
          console.log("Retrive EmailSetting list error: " + error);
          this.toastr.warning(error.status + "- retrive data error", 'Fail');
        }
      });
  }

```
{% endcode %}

{% code fullWidth="true" %}
```
// Change toastr position

this.toastr.toastrConfig.positionClass = 'toast-top-center';
this.toastr.clear(); 
this.toastr.warning('The type has been selected, so the value must be entered.', 'Warning');

this.toastr.toastrConfig.positionClass = 'toast-top-right';


positionClass:
'toast-top-right' (default)
'toast-bottom-right'
'toast-bottom-left'
'toast-top-left'
'toast-top-full-width'
'toast-bottom-full-width'
'toast-top-center'
'toast-bottom-center'
```
{% endcode %}

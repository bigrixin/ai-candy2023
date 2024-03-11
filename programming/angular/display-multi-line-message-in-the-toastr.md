# display multi line message in the Toastr



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

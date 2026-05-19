# Random Password and Toggle



{% code fullWidth="true" %}
```
// Some code Toggle Password Visibility (hide/show password)


  import { MatIconModule } from '@angular/material/icon';

 
  <input class="form-control"  formControlName="newPassword"  [type]="showPassword ? 'text' : 'password'" />

  <mat-icon matSuffix (click)="togglePasswordVisibility()"
      style="color: rgb(107, 66, 4); padding:3px 30px 2px 16px; cursor:pointer;">
             {{showPassword?'visibility':'visibility_off'}}
  </mat-icon>



  public showPassword: boolean = false;

   togglePasswordVisibility(): void {
    this.showPassword = !this.showPassword;
  }


```
{% endcode %}

{% code fullWidth="true" %}
```
// Random password

  <input autoComplete="new-password" cFormControl formControlName="Password" placeholder="Password"
    title="Password must be at least 6 characters" [type]="showPassword ? 'text' : 'password'" />
  <a class="pointer text-success" style="padding:6px 2px 0 20px ; cursor:pointer; font-size: x-large;"
    (click)="randomKey();"> <i class="fa fa-key " title="Password generator"></i></a>

randomKey() {
  this.randomPassword = Math.random().toString(36).slice(-8);
  this.frm.controls['Password'].setValue(this.randomPassword);
}


```
{% endcode %}

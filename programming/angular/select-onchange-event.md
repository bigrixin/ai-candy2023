# select onChange event

{% code fullWidth="true" %}
```
// it can be used for the first select action
// when selecting component change
1. (change)   
	<select class="form-control form-select" id="userName" formControlName="userName"
	  (change)="onUserChange($event)">
	  <option value="0">===> [ 1 - Please select a user ] </option>
	  <option *ngFor="let user of users">{{user.userName}} - ({{user?.email}}) - {{user.customerId}} - Email
		confirmed ({{user?.emailConfirmed}})
	  </option>
	</select>


	onUserChange(e: any) {
		let userName = e.target.value;
	 
	// dispaly role list
	this.userService.getRolesByuserName(userName ).subscribe({
		next: (data) => {
		  if (data.status == 200) {
			this.userRole = data['body'][0];
			this.frm.controls['role'].setValue(this.userRole);   ///<<<<<<
		  }
		},
		error: (error) => {
		  console.log(error);
		  this.toastr.warning(error.status + ' - Select User Fail', 'Fail');
		},
	});
		
	}

```
{% endcode %}

{% code fullWidth="true" %}
```
// it can be used for second select action
// when value changed
2. (ngModelChange)  

	<select class="form-control form-select" id="role" formControlName="role"
	  (ngModelChange)="onRoleChange($event)"  [ngModel] >         //<<<<<<< may need  [ngModel]  
	  <option value="0">===> [ 2 - Assign a Role for the above user ] </option>
	  <option *ngFor="let role of roles">{{role}}</option>
	</select>

  onRoleChange(role: any) {
      let selectedRole = role;
  }
```
{% endcode %}

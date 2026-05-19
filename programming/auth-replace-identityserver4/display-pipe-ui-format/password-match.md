---
description: password and repeat password match validation
---

# Password match

{% code fullWidth="true" %}
```
//Angular password match
-----------component.ts-----------------------------------------------

this.frm = this.fb.group({
 CompanyId: ['', Validators.required],

 Password: ['', [Validators.required,
	   	 	Validators.minLength(6),
		      Validators.maxLength(25),
			Validators.pattern('^(?=.*[0-9])(?=.*[a-zA-Z])([a-zA-Z0-9]+)$')]],
 RepeatPassword: ['', [Validators.required]],
}, { validators: this.passwordMatchingValidatior }
);


passwordMatchingValidatior: ValidatorFn = (control: AbstractControl): ValidationErrors | null => {
	const password = control.get('Password');
	const confirmPassword = control.get('RepeatPassword');
	return password?.value === confirmPassword?.value ? null : { notmatched: true };
};
```
{% endcode %}

{% code fullWidth="true" %}
```
// html

<c-input-group class="mb-2">
	<span cInputGroupText>
	  <svg cIcon name="cilLockLocked"></svg> <span style="color: red;"> *</span>
	</span>
	<input autoComplete="new-password" cFormControl formControlName="Password" placeholder="Password"
	  title="Must contain at least 1 digit, 1 lowercase, 1 uppercase letter and at least 6 characters long."
	  type="password" />
	</c-input-group>
	<c-input-group class="mb-4">
	<span cInputGroupText>
	  <svg cIcon name="cilLockLocked"></svg> <span style="color: red;"> *</span>
	</span>
	<input autoComplete="new-password" cFormControl formControlName="RepeatPassword"
	  title="Must match password"
	  placeholder="Repeat password" type="password" />
</c-input-group>
```
{% endcode %}

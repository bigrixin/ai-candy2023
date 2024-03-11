# Auth get user info by email



{% code fullWidth="true" %}
```
// Some code

	[HttpGet("role/getby/email/{email}")]
	public async Task<IActionResult> GetRoleByUserEmail(string email)
	{
		if (!string.IsNullOrEmpty(email))
		{
			var user = await _userManager.FindByEmailAsync(email);
			if (user != null)
			{
				var role = await _userManager.GetRolesAsync(user);
				return Ok(role);
			}

		}
		return BadRequest();
	}


-----------------------

  onUserChange(e: any) {
    // console.log(e.target.value);
    let displaiedUser = e.target.value;
  ///  displaiedUser = displaiedUser.substring(0, displaiedUser.indexOf('-') - 1).trim(); ///need to update

    if (displaiedUser != '') {
      let companyid = displaiedUser
        .substring(displaiedUser.lastIndexOf('-') - 2, displaiedUser.lastIndexOf('-') - 1)
        .trim();

      let email =displaiedUser
        .substring(displaiedUser.indexOf('(') +1, displaiedUser.indexOf(')'))
        .trim();

      // dispaly role list
      this.userService.getRolesByEmail(email).subscribe({
        next: (data) => {
          if (data.status == 200) {
            this.userRole = data['body'][0];
            this.frm.controls['role'].setValue(this.userRole);
          }
        },
        error: (error) => {
          console.log(error);
          this.toastr.warning(error.status + ' - Select User Fail', 'Fail');
        },
      });
    }
  }

-------user.service------------

  // get roles by email
  getRolesByEmail(email: string): Observable<any> {
    return this.http.get<HttpResponse<Object>>(this.rootURL + 'api/account/role/getby/email/' + email, { observe: 'response' })
      .pipe(map((res: any) => res));
  }


  // assign roles
  assignRole(assignRole: AssignRole): Observable<any> {
    var currentUserName = localStorage.getItem('Email');
    if (currentUserName != null) {
      assignRole.currentUserName = currentUserName;
      let delectedUser = assignRole.userName;
      if (delectedUser != '') {
        let email =delectedUser
          .substring(delectedUser.indexOf('(') +1, delectedUser.indexOf(')'))
          .trim();
        assignRole.userName = email;    
      }
    }
    return this.http.post<HttpResponse<Object>>(this.rootURL + 'api/account/assignroles', assignRole, { observe: 'response' })
      .pipe(map(res => res));
  }
```
{% endcode %}

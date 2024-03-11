# Angular add reCAPTCHA v3 (Google)

```
// Some code

1. Install “ng-recaptchaModule”
   https://github.com/DethAriel/ng-recaptcha
  
   npm i ng-recaptcha@9        (for angular 13))
  
   reCAPTCHA v3:
   https://github.com/DethAriel/ng-recaptcha#example-basic-v3

2. Registering on Google reCAPTCHA 

   https://www.google.com/recaptcha/admin/create
   
   
   Site KEY: 6LfjuxUlAAAAAGTu5kYWgs6ryH8zNvOHfi6uAGoy
  

3. Add constants

   reCAPTCHA_site_key: '6LfjuxUlAAAAAGTu5kYWgs6ryH8zNvOHfi6uAGoy'
   

4. app\views\pages\login\login.component.ts
  
    import the ReCaptchaV3Service


	constructor {
	   private recaptchaV3Service: ReCaptchaV3Service,}
	  

    ngOnInit() {
		if (frm.invalid) {
		  for (const control of Object.keys(frm.controls)) {
			frm.controls[control].markAsTouched();
		  }
		  return;
		}

		this.recaptchaV3Service.execute('importantAction')
		  .subscribe({
			next: (token) => {
			  //this.handleToken(token);
			  console.debug(`Token [${token}] generated`);
			},
			error: (error) => {
			  console.debug(error);
			}
		  });
	  }
   }
	   
   
5. pages.module.ts (add / edit module)

 	import { RecaptchaFormsModule, RecaptchaModule } from 'ng-recaptcha';

	  imports: [
		RecaptchaModule,
		RecaptchaFormsModule
	  ],



6.	app.module.ts   (root)

	import { RECAPTCHA_V3_SITE_KEY,  RecaptchaV3Module, RecaptchaLoaderService } from "ng-recaptcha";
	import { constants } from './constants';


	imports: [
		BrowserModule,
		RecaptchaV3Module,
	]

	providers: [
	   RecaptchaLoaderService,
		{
		  provide: RECAPTCHA_V3_SITE_KEY,
		  useValue: constants.reCAPTCHA_site_key_v3
		},
	]

 

6. Fix error:

	npm ERR! peer @angular/common@">= 15.0.0" from ngx-captcha@13.0.0

		npm ERR! While resolving: nwi-web@4.0.0-alpha.3
		npm ERR! Found: @angular/common@13.0.3
		npm ERR! node_modules/@angular/common
		npm ERR!   @angular/common@"~13.0.0" from the root project
		npm ERR!
		npm ERR! Could not resolve dependency:
		npm ERR! peer @angular/common@">= 15.0.0" from ngx-captcha@13.0.0
		npm ERR! node_modules/ngx-captcha
		npm ERR!   ngx-captcha@"*" from the root project


	npm config set legacy-peer-deps true


    npm i ng-recaptcha@9 
```

---
description: v19 and v20
---

# Angular package.json

<mark style="color:$success;">**package.json  (v19)**</mark>&#x20;

{\
"name": "angular-v19-admin",\
"version": "1.0.0",\
"author": "CodedThemes",\
"license": "MIT",\
"scripts": {\
"ng": "ng",\
"start": "ng serve",\
"build": "ng build",\
"build-prod": "ng build --configuration production --base-href /angular/free/",\
"watch": "ng build --watch --configuration development",\
"test": "ng test",\
"lint": "ng lint",\
"lint:fix": "ng lint --fix",\
"prettier": "prettier --write ./src"\
},\
"private": false,\
"eslintConfig": {\
"extends": \[\
"eslint:recommended",\
"plugin:@typescript-eslint/recommended"\
],\
"parser": "@typescript-eslint/parser",\
"plugins": \[\
"@typescript-eslint"\
],\
"rules": {\
"@typescript-eslint/no-explicit-any": \[\
"error",\
{\
"fixToUnknown": true\
}\
]\
}\
},\
"dependencies": {\
"@angular/animations": "19.2.14",\
"@angular/cdk": "19.2.19",\
"@angular/common": "19.2.14",\
"@angular/compiler": "19.2.14",\
"@angular/core": "19.2.14",\
"@angular/forms": "19.2.14",\
"@angular/material": "^19.2.19",\
"@angular/platform-browser": "19.2.14",\
"@angular/platform-browser-dynamic": "19.2.14",\
"@angular/router": "19.2.14",\
"@ant-design/icons-angular": "19.0.0",\
"@ng-bootstrap/ng-bootstrap": "18.0.0",\
"@popperjs/core": "2.11.8",\
"angular-datatables": "^19.0.0",\
"apexcharts": "4.7.0",\
"bootstrap": "5.3.7",\
"datatables.net": "^2.3.3",\
"datatables.net-bs5": "^2.0.3",\
"jquery": "^3.6.0",\
"ng-apexcharts": "1.15.0",\
"ngx-scrollbar": "18.0.0",\
"ngx-toastr": "^19.0.0",\
"rxjs": "\~7.8.2",\
"tslib": "2.8.1",\
"zone.js": "\~0.15.1"\
},\
"devDependencies": {\
"@angular-devkit/build-angular": "19.2.15",\
"@angular-eslint/builder": "19.8.1",\
"@angular-eslint/eslint-plugin": "19.8.1",\
"@angular-eslint/schematics": "19.8.1",\
"@angular-eslint/template-parser": "19.8.1",\
"@angular/cli": "19.2.15",\
"@angular/compiler-cli": "19.2.14",\
"@eslint/eslintrc": "3.3.1",\
"@eslint/js": "9.29.0",\
"@schematics/angular": "^19.2.15",\
"@types/datatables.net": "^1.10.28",\
"@types/jquery": "^3.5.9",\
"@types/node": "24.0.4",\
"@typescript-eslint/eslint-plugin": "8.35.0",\
"@typescript-eslint/parser": "8.35.0",\
"eslint": "9.29.0",\
"prettier": "3.6.1",\
"typescript": "5.8.3"\
},\
"packageManager": "yarn@1.22.22+sha512.a6b2f7906b721bba3d67d4aff083df04dad64c399707841b7acf00f6b133b7ac24255f2652fa22ae3534329dc6180534e98d17432037ff6fd140556e2bb3137e"\
}

<mark style="color:$danger;">**package.json  (v20)**</mark>&#x20;

{\
"name": "angular-v20-admin",\
"version": "1.0.0",\
"author": "CodedThemes",\
"license": "MIT",\
"scripts": {\
"ng": "ng",\
"start": "ng serve",\
"build": "ng build",\
"build-prod": "ng build --configuration production --base-href /angular/free/",\
"watch": "ng build --watch --configuration development",\
"test": "ng test",\
"lint": "ng lint",\
"lint:fix": "ng lint --fix",\
"prettier": "prettier --write ./src"\
},\
"private": false,\
"eslintConfig": {\
"extends": \[\
"eslint:recommended",\
"plugin:@typescript-eslint/recommended"\
],\
"parser": "@typescript-eslint/parser",\
"plugins": \[\
"@typescript-eslint"\
],\
"rules": {\
"@typescript-eslint/no-explicit-any": \[\
"error",\
{\
"fixToUnknown": true\
}\
]\
}\
},\
"dependencies": {\
"@angular/animations": "20.2.1",\
"@angular/cdk": "20.2.0",\
"@angular/common": "20.2.1",\
"@angular/compiler": "20.2.1",\
"@angular/core": "20.2.1",\
"@angular/forms": "20.2.1",\
"@angular/material": "^20.2.0",\
"@angular/platform-browser": "20.2.1",\
"@angular/platform-browser-dynamic": "20.2.1",\
"@angular/router": "20.2.1",\
"@ant-design/icons-angular": "20.0.0",\
"@ng-bootstrap/ng-bootstrap": "19.0.0",\
"@popperjs/core": "2.11.8",\
"angular-datatables": "^19.0.0",\
"apexcharts": "4.7.0",\
"bootstrap": "5.3.7",\
"datatables.net": "^2.3.3",\
"datatables.net-bs5": "^2.0.3",\
"jquery": "^3.6.0",\
"ng-apexcharts": "1.16.0",\
"ngx-scrollbar": "18.0.0",\
"ngx-toastr": "^19.0.0",\
"rxjs": "\~7.8.2",\
"tslib": "2.8.1",\
"zone.js": "\~0.15.1"\
},\
"devDependencies": {\
"@angular-devkit/build-angular": "20.2.0",\
"@angular-eslint/builder": "20.2.0",\
"@angular-eslint/eslint-plugin": "20.2.0",\
"@angular-eslint/schematics": "20.2.0",\
"@angular-eslint/template-parser": "20.2.0",\
"@angular/cli": "20.2.0",\
"@angular/compiler-cli": "20.2.1",\
"@eslint/eslintrc": "3.3.1",\
"@eslint/js": "9.29.0",\
"@schematics/angular": "^20.2.1",\
"@types/datatables.net": "^1.10.28",\
"@types/jquery": "^3.5.9",\
"@types/node": "24.0.4",\
"@typescript-eslint/eslint-plugin": "8.35.0",\
"@typescript-eslint/parser": "8.35.0",\
"eslint": "9.29.0",\
"prettier": "3.6.1",\
"typescript": "5.8.3"\
},\
"packageManager": "yarn@1.22.22+sha512.a6b2f7906b721bba3d67d4aff083df04dad64c399707841b7acf00f6b133b7ac24255f2652fa22ae3534329dc6180534e98d17432037ff6fd140556e2bb3137e"\
}

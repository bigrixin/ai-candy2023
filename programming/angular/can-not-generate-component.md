# Can not generate component

{% code fullWidth="true" %}
```
// Some code

ng g c componentName 
>>> can not generate component

   ng --version

1. check command
    ng generate --help

    need include:
    component、service、module,  schematics

2. check schematics

    npm list @schematics/angular
    npm install --save-dev @schematics/angular


4. install angular
     npm uninstall -g @angular/cli
     npm install -g @angular/cli

5.  Install local project dependencies

    delete node_modules and package-lock.json
    npm uninstall --save @angular/cli
    npm install --save @angular/cli
    
6. ng update


7. 
angular.json

"schematics": {
  "@schematics/angular:component": {
    "standalone": true,
    "style": "scss"
  }
}

8.
npm cache clear --force
npm cache verify
```
{% endcode %}

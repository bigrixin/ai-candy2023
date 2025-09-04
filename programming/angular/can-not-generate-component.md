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
    npm install -g @angular/cli

5.  Install local project dependencies

    delete node_modules and package-lock.json
    npm install
```
{% endcode %}

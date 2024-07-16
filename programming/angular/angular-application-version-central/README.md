# Angular application version central

<pre data-full-width="true"><code>1. Add "prebuild" and "build" in package.json

  {
    "name": "application",
    "version": "1.0.1",
    "scripts": {
      "ng": "ng",
      "start": "ng serve",
     <a data-footnote-ref href="#user-content-fn-1"> "prebuild": "npm --no-git-tag-version version patch",</a>     &#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;
     <a data-footnote-ref href="#user-content-fn-2"> "build": "ng build --prod --aot",</a>                         &#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;
      "test": "ng test",
      "lint": "ng lint",
      "e2e": "ng e2e"
    },
  ...

2. Be sure to have included ‘@type/node’ as a dependency in your ‘package.json’ file.
 tsconfig.app.json

  ...
    "compilerOptions": {
      "outDir": "../out-tsc/spec",
      "module": "commonjs",
      "types": [
        "jasmine",
        <a data-footnote-ref href="#user-content-fn-3">"node"  </a> &#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;
      ]
    }
  ...
   
<strong>3. Add appVersion in environment.ts (‘src\environments\environment.prod.ts’)
</strong>
    export const environment = {
      appVersion: require('../../package.json').version + '-dev',
      production: false
    };

4. default-footer.component.ts
   import { environment } from 'src/environments/environment';
 
   currentVersion = environment.appVersion;

5. default-footer.component.html
   &#x3C;span> Version: {{ currentVersion }} &#x3C;/span>
   
6. npm run build
     The application version number will change to "1.0.2"
</code></pre>

[^1]: 

[^2]: 

[^3]: 

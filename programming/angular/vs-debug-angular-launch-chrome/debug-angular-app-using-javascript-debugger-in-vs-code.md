---
description: >-
  Since Debugger for Chrome is Deprecated, the JavaScript Debugger (Nightly)
  will be used for debug Angular app for Chrome
---

# Debug Angular app using JavaScript Debugger in VS Code



<pre data-full-width="true"><code>// Steps

1. Disable JavaScript Debugger

2. Install or Enable JavaScript Debugger (Nightly)

3. Run JavaScript Debugger using A or B method

     A: View-> Command Palette
	   > Debug: JavaScript Debug Terminal
		ng serve

     B: launch.json

         {
            "version": "0.2.0",
            "configurations": [
                {
                    "type": "chrome",
                    "request": "launch",
                    "name": "Launch Chrome against localhost",
                    // "url": "http://localhost:4200",
                    "webRoot": "${workspaceFolder}",
        
                    "sourceMaps": true,
                    "url": "http://[::1]:4200/",    ///&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C;&#x3C; load more faster
                }
            ]
        }

     C: package.json

          "scripts": {
            "ng": "ng",
            "start": "ng serve --host=127.0.0.1",
            ...


      C: F5 - Run Debugging

4. View-> Command Palette <a data-footnote-ref href="#user-content-fn-1">(Ctrl+Shit+P)</a>

   > Debug: Open Link
     http://localhost:4200

</code></pre>

[^1]: 

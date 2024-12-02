# VS Code: Auto add missing imports when save



<pre><code>
1. Ctrl+Shift+P

<strong>  open the option Preferences: Open User Settings (JSON):
</strong>  
   "editor.codeActionsOnSave": {
      "source.organizeImports": true,
      "source.addMissingImports": true
    },

2. Ctrl+Shift+P
   open the option Preferences: Open Keyboard Shortcuts (JSON):
  
    {
      "key": "ctrl+s",
      "command": "editor.action.sourceAction",
      "args": {
        "kind": "source.addMissingImports",
        "apply": "first"
      }
    }

3. Add modules, press [CTRL + S]
</code></pre>

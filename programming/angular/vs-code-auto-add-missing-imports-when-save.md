# VS Code: Auto add missing imports when save

1. Ctrl+Shift+P

open the option Preferences: Open User Settings (JSON):

"editor.codeActionsOnSave": { "source.organizeImports": true, "source.addMissingImports": true },

2. Ctrl+Shift+P open the option Preferences: Open Keyboard Shortcuts (JSON):

{ "key": "ctrl+s", "command": "editor.action.sourceAction", "args": { "kind": "source.addMissingImports", "apply": "first" } }

3. Add module, press \[CTRL + S]

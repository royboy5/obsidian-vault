## 1. 🔌 Install Plugin
* Run in terminal: 
```bash
{{INSTALL_CMD}}
```

## 2. 🏗️ Generate {{TYPE_LABEL}}
* Run generator:
```bash
{{COMMAND}}
```

## 3. ✅ Verify Targets
* Check `{{DIRECTORY}}/{{APP_NAME}}/project.json`
  * `serve` (Dev)
  * `build` (Prod)
  * `lint`

## 4. 🛠️ TSX Watch (Optional — Node only)
* Install package:
```bash
pnpm add -wD tsx
```

* Configure `project.json`:
```json
"serve": {
  "executor": "nx:run-commands",
  "options": {
    "command": "tsx watch src/main.ts",
    "cwd": "{{DIRECTORY}}/{{APP_NAME}}",
    "color": true
  }
}
```
<%*
// 1. Context Sniffer: check Templater's internal cache first, fallback to active workspace leaf
let activePath = "";
if (tp.config.active_file) {
    activePath = tp.config.active_file.path;
} else if (app.workspace.getActiveFile()) {
    activePath = app.workspace.getActiveFile().path;
}

let projectName = "";
// Capture the exact folder name immediately trailing "1 Projects/"
const projectMatch = activePath.match(/1 Projects\/([^\/]+)/);
if (projectMatch) {
    projectName = projectMatch[1];
}

// 2. Safety Fallback: If no active path context is found, offer a text prompt box
if (!projectName) {
    projectName = await tp.system.prompt("No active project folder detected. Enter target Project Name manually:");
}

// 3. User Configuration Prompts
const featureFolder = await tp.system.prompt(`Project [${projectName}] verified. Enter feature domain folder (e.g., identity-service):`);
const specTitle = await tp.system.prompt("Enter the Specification Title (e.g., User Authentication):");

// 4. File Path Assembly Metadata
const fileName = `Spec - ${specTitle}`;
const targetDirectory = `1 Projects/${projectName}/features/${featureFolder}`;
const creationDate = tp.date.now("YYYY-MM-DD");

// 5. Build raw Markdown layout entirely in memory
const specTemplateContent = `---
type: feature-spec
project: ${projectName}
domain: ${featureFolder}
status: todo
created: ${creationDate}
tags:
  - project/${projectName.toLowerCase().replace(/\s+/g, '-')}
  - domain/${featureFolder}
---

# Spec - ${specTitle}

*Project Dashboard: [[1 Projects/${projectName}/|📂 ${projectName}]]*
*Domain Module: [[1 Projects/${projectName}/features/${featureFolder}/${featureFolder}|📂 ${featureFolder}]]*
*Status: ⚪ Todo*

---

## 1. Summary & Objective
*A brief 2-3 sentence overview of what this feature slice achieves and why it is being built.*
- 

## 2. Technical Scope & Architecture
*Detail how this feature interacts with your specific services and database container layer.*
* **API Surface Domain:** \`apps/${featureFolder}-api\`
* **Local Subdomain Router:** \`https://${featureFolder}.${projectName.toLowerCase()}.local/api/\`
* **Database Container Target:** \`postgres-${featureFolder}\`

### 📊 Localized Data Flow Notes
* ---

## 3. Engineering Checklists (MVP)

### 🧱 Backend Implementation
- [ ] Create OpenAPI routing schema definition in \`packages/openapi/\`
- [ ] Run automated build task to compile shared TS / Dart bindings
- [ ] Initialize endpoint endpoints inside \`apps/${featureFolder}-api\`
- [ ] Set up migrations and schema synchronization routines

### 🖥️ Web Interface Implementation
- [ ] Design client view architectures matching specifications
- [ ] Hook view layout states to endpoints using type-safe contract interfaces

### 📱 Mobile UI Implementation
- [ ] Scaffold user views and input schemas inside cross-platform modules
- [ ] Bind state management dispatch sequences to backend API handlers

---

## 4. Architectural Edge Cases & Risks
* Document any potential data cross-contamination boundaries or unique performance footprints here.
- 

---

## 🧠 Related System Documents
* Master Project Board: [[1 Projects/${projectName}/Hudl-Up - Dev Board|📋 Master Dev Board]]
* Root Architectural Toolchain: [[1 Projects/${projectName}/Hudl-Up - Monorepo & Toolchain Spec|⚙️ Monorepo Spec]]
`;

// 6. CRUCIAL FIX: Swap argument order to (content, filename, open_new, target_folder)
await tp.file.create_new(specTemplateContent, fileName, true, targetDirectory);
_%>
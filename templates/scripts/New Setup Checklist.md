<%*
// 1. CONFIGURATION: Your list of available setup templates
// Key = What you see in the menu
// Value = The exact filename in "90 - Templates"
const setupTemplates = {
    "Python (AI & Data Science) 🐍": "Setup Python AI Checklist",
    "Node.js / TypeScript 🏗️": "Setup NodeJS Checklist",
    "Nx Monorepo 🦋": "Setup Nx Monorepo Checklist",
    "General / New Language 🗺️": "Setup General Checklist"
};

// 2. ROBUST FILE DETECTION
let activeFile = app.workspace.getActiveFile();
if (!activeFile) {
    const leaf = app.workspace.getMostRecentLeaf();
    if (leaf && leaf.view && leaf.view.file) {
        activeFile = leaf.view.file;
    }
}

if (!activeFile) {
    new Notice("⚠️ No active file. Click on your Project Canvas background first!");
    return;
}

// 3. SAFETY CHECK: PROJECTS ONLY
if (!activeFile.path.startsWith("1 Projects")) {
    new Notice("🚫 Error: You must be inside '1 Projects' to run this.");
    return; 
}

// 4. GET PROJECT ROOT & DOCS PATH
const pathParts = activeFile.path.split("/");
const projectRootPath = `${pathParts[0]}/${pathParts[1]}`;
const docsPath = `${projectRootPath}/docs`;

// 5. SHOW MENU
const options = Object.keys(setupTemplates);
const selectedLabel = await tp.system.suggester(options, options);
if (!selectedLabel) return; 

const templateName = setupTemplates[selectedLabel];

// 6. AUTO-GENERATE NAME (No Prompt)
// Logic: Cleans "Python (AI & Data Science) 🐍" -> "Setup - Python"
// You can customize this line if you prefer "LLM Course - Setup - Python"
const simpleName = selectedLabel.split(" ")[0]; // Grabs just the first word (Python, Node.js, Nx)
const fileName = `Setup - ${simpleName}`;

// 7. CREATE FOLDER & FILE
// Create docs folder if missing
if (!(await app.vault.adapter.exists(docsPath))) {
    await app.vault.createFolder(docsPath);
}
const docsFolder = app.vault.getAbstractFileByPath(docsPath);
const templateFile = tp.file.find_tfile(templateName);

if (templateFile) {
    // Check if file exists to avoid overwriting
    const newFilePath = `${docsPath}/${fileName}.md`;
    if (await app.vault.adapter.exists(newFilePath)) {
        new Notice(`⚠️ ${fileName} already exists! Opening it...`);
        // Just open the existing file
        const existingFile = app.vault.getAbstractFileByPath(newFilePath);
        await app.workspace.getLeaf(false).openFile(existingFile);
    } else {
        // Create and open new file
        await tp.file.create_new(templateFile, fileName, true, docsFolder);
        new Notice(`⚡ Created ${fileName}`);
    }
} else {
    new Notice(`❌ Error: Template '${templateName}' not found`);
}
%>
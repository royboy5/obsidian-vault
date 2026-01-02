<%*
// 1. CONFIGURATION
// Ensure this matches the filename of your template exactly
const templateName = "Setup Nx Monorepo Checklist";

// 2. ASK FOR MONOREPO NAME
const repoName = await tp.system.prompt("Enter Monorepo Name (e.g. my-org):");

// If user pressed Esc, stop
if (!repoName) return; 

// 3. DETERMINE LOCATION
// We try to put it in a 'docs' folder if it exists, otherwise current folder
const activeFile = app.workspace.getActiveFile();
let targetFolder = activeFile.parent;

if (activeFile) {
    // Check if a 'docs' folder exists in this project
    const docsPath = `${activeFile.parent.path}/docs/setup`;
    const docsExists = await app.vault.adapter.exists(docsPath);
    
    if (!docsExists) {
        // IF MISSING: Create it!
        await app.vault.createFolder(docsPath);
    }
    targetFolder = app.vault.getAbstractFileByPath(docsPath);
}

// 4. CREATE THE NOTE
const newFileName = `${repoName} - Nx Setup`;
const templateFile = tp.file.find_tfile(templateName);

if (templateFile) {
    // Check if file already exists
    const fullPath = `${targetFolder.path}/${newFileName}.md`;
    if (await app.vault.adapter.exists(fullPath)) {
        new Notice(`⚠️ File ${newFileName} already exists!`);
        // Open the existing file
        const existingFile = app.vault.getAbstractFileByPath(fullPath);
        await app.workspace.getLeaf(false).openFile(existingFile);
    } else {
        // Create and open
        await tp.file.create_new(templateFile, newFileName, true, targetFolder);
        new Notice(`✅ Created ${newFileName}`);
    }
} else {
    new Notice(`❌ Error: Could not find template '${templateName}'`);
}
%>
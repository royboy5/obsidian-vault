<%*
// 1. Get the path of the currently active file to determine the project folder
const activeFilePath = tp.file.path(true);
if (!activeFilePath.startsWith("1 Projects/")) {
    new Notice("Error: Please run this script from within the project.", 5000);
    return;
}
const projectFolder = activeFilePath.split('/').slice(0, 2).join('/');
const adrFolder = `${projectFolder}/ADRs`;

// Create ADR folder if it doesn't exist
try {
    await app.vault.createFolder(adrFolder).catch(() => {});
} catch (error) {
    // Folder might already exist, that's ok
}

// 2. Enter ADR title
const title = await tp.system.prompt("Enter ADR Title:");
if (!title) {
    return;
}

// 3. Find the next sequential number for the ADR.
const adrFiles = app.vault.getFiles().filter(file => file.path.startsWith(adrFolder));
let nextNumber = 1;
if (adrFiles.length > 0) {
    const highestNumber = adrFiles
        .map(file => parseInt(file.basename.split('-')[0]))
        .filter(num => !isNaN(num))
        .reduce((max, num) => Math.max(max, num), 0);
    nextNumber = highestNumber + 1;
}
const formattedNumber = String(nextNumber).padStart(3, '0');

// 4. Create the kebab-case filename.
const kebabCaseTitle = title.toLowerCase().replace(/\s+/g, '-');
const fileName = `${formattedNumber}-${kebabCaseTitle}`;
const newFilePath = `${adrFolder}/${fileName}.md`;

// 5. Create date, properties, and H2 Header
const createdDate = tp.date.now("YYYY-MM-DD HH:mm");
const adrProperties = `---
status: proposed
created: ${createdDate}
updated: ${createdDate}
tags:
  - adr
---`;
const adrTitle = `## ADR: ${title}`;

// 6. Get the contents from ADR Template
const templateFile = tp.file.find_tfile("ADR Template");
if (!templateFile) {
    new Notice("Error: ADR Template not found at Templates/adr-static.md", 5000);
    return;
}
const templateContent = await app.vault.read(templateFile);

// 7. Prepend properties and title to ADR content
const finalContent = `${adrProperties}\n\n${adrTitle}\n\n${templateContent}`;

// 8. Create ADR from template
try {
    await app.vault.create(newFilePath, finalContent);
    new Notice(`ADR "${fileName}" created successfully!`, 5000);
    
    // 9. Open the new file
    const newFile = app.vault.getAbstractFileByPath(newFilePath);
    if (newFile) {
        await app.workspace.getLeaf().openFile(newFile);
    }
} catch (error) {
    new Notice(`Error creating ADR: ${error.message}`, 5000);
}
%>
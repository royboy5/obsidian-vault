<%*
// 1. Get the path of the current active file to determine the notes folder.
const activeFilePath = tp.file.path(true);
if (!activeFilePath.startsWith("3 Resources/")) {
    new Notice("Error: Please run this script from within a resource.", 5000);
    return;
}
const resourceFolder = activeFilePath.split('/').slice(0, 2).join('/');
const resourceNotesFolder = `${resourceFolder}/notes`;

// Create notes folder if it doesn't exist
try {
    await app.vault.createFolder(resourceNotesFolder).catch(() => {});
} catch (error) {
    // Folder might already exist, that's ok
}

// 2. Get the title of the note
const resourceNoteTitle = await tp.system.prompt("Enter Resource Note Title:");
if (!resourceNoteTitle) {
    new Notice("Cancelled", 3000);
    return;
}

const newFilePath = `${resourceNotesFolder}/${resourceNoteTitle}.md`;
const createdDate = tp.date.now("YYYY-MM-DD HH:mm");

// 3. Create note properties and H2 title
const resourceNoteProperties = `---
created: ${createdDate}
updated: ${createdDate}
tags:
  - notes
---`;
const noteTitle = `## ${resourceNoteTitle}`;

// 4. Get content from Resource Note Template
const templateFile = tp.file.find_tfile("Resource Note Template");
if (!templateFile) {
    new Notice("Error: Resource Note Template not found", 5000);
    return;
}
const templateContent = await app.vault.read(templateFile);

// 5. Prepend properties and title to content
const finalContent = `${resourceNoteProperties}\n\n${noteTitle}\n\n${templateContent}`;

// 6. Create resource note with final content
try {
    await app.vault.create(newFilePath, finalContent);
    new Notice(`Resource Note "${resourceNoteTitle}" created successfully!`, 5000);
    
    // 7. Open new file
    const newFile = app.vault.getAbstractFileByPath(newFilePath);
    if (newFile) {
        await app.workspace.getLeaf().openFile(newFile);
    }
} catch (error) {
    new Notice(`Error creating note: ${error.message}`, 5000);
}
%>
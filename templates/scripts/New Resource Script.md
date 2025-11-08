<%*
// 1. Ask for resource name
const resourceName = await tp.system.prompt("Enter Resource Name:");

if (!resourceName) {
	return;
}

// 2. Define paths and date
const resourceFolder = `3 Resources/${resourceName}`;
const createdDate = tp.date.now("YYYY-MM-DD HH:mm");
const updatedDate = tp.date.now("YYYY-MM-DD HH:mm");

// 3. Create folders
await tp.app.vault.createFolder(resourceFolder);
await tp.app.vault.createFolder(`${resourceFolder}/notes`);

// 4. Get Resource MOC content from template
const resourceMocTemplate = tp.file.find_tfile("Resource MOC Template");
const resourceMocContent = await tp.app.vault.read(resourceMocTemplate);

// 5. Define new resource MOC name
const resourceFileName = `${resourceName} MOC`;

// 6. Create properties block
const resourceProperties = `---
created: ${createdDate}
updated: ${updatedDate}
tags:
  - resource
  - moc
---`;

// 7. Create Resource H2 Titie
const resourceTitle = `## ${resourceName} MOC`;

// 8. Prepend properties and title to resource moc content
const finalContent = `${resourceProperties}\n\n${resourceTitle}\n\n${resourceMocContent}`;

// 9. Create Resource MOC 
await tp.app.vault.create(`${resourceFolder}/${resourceName}.md`, finalContent);

new Notice(`Resource "${resourceName}" created successfully!`, 5000);
%>
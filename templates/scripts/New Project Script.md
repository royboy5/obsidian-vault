<%*
// 1. Ask for te new project's name
const projectName = await tp.system.prompt("Enter Project Name:");

if (!projectName) {
	return;
}

// 2. Define paths and date
const projectFolder = `1 Projects/${projectName}`;
const createdDate = tp.date.now("YYYY-MM-DD HH:mm");

// 3. Create folders
await tp.app.vault.createFolder(projectFolder);
await tp.app.vault.createFolder(`${projectFolder}/ADRs`);
await tp.app.vault.createFolder(`${projectFolder}/docs`);
await tp.app.vault.createFolder(`${projectFolder}/features`);

// 4. Get the content from Project Brief template
const briefTemplateFile = tp.file.find_tfile("Project Brief Template");
const briefContent = await tp.app.vault.read(briefTemplateFile);

// 5. Define new brief file's name
const briefFileName = `${projectName} - Project Brief`;

// 6. Create properties block
const briefProperties = `---
created: ${createdDate}
status: todo
tags: 
  - project
  - brief
---`

// 7. Create Project Brief H2 Title
const briefTitle = `## Project Brief: ${projectName}`;

// 8. Prepend properties and title to brief content
const finalBriefContent = `${briefProperties}\n\n${briefTitle}\n\n${briefContent}`;

// 9. Create the project brief with the correct name
await tp.app.vault.create(`${projectFolder}/${briefFileName}.md`, finalBriefContent);

// 10. Create the API Design Doc from template
const apiTemplateFile = tp.file.find_tfile("API Design Template");
const apiContent = await tp.app.vault.read(apiTemplateFile);
await tp.app.vault.create(`${projectFolder}/docs/API Design.md`, apiContent);

// 11. Create Project Canvas
await tp.app.vault.create(`${projectFolder}/${projectName} - Project Canvas.canvas`, "");

new Notice(`Project "${projectName}" created successfully!`, 5000);
%>
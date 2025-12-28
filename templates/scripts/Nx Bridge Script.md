<%*
// 1. CONFIGURATION
const configMap = {
    "React Frontend":   { file: "Nx React Setup",   path: "apps/client", tag: "React" },
    "Node Backend":     { file: "Nx Node Setup",    path: "apps/api",    tag: "Node.js" }
};

const stackOptions = Object.keys(configMap);

// 2. INPUTS
const selectedOption = await tp.system.suggester(stackOptions, stackOptions);
if (!selectedOption) return; // Exit if cancelled

const config = configMap[selectedOption];
const appName = await tp.system.prompt("What is the application name? (e.g., inventory-api)");

// 3. GENERATE CONTENT
// Fetch the snippet content
const templateFile = tp.file.find_tfile(config.file);
let snippetContent = await tp.file.include(templateFile);

// Replace placeholders in the snippet
snippetContent = snippetContent.replaceAll("{{APP_NAME}}", appName);
snippetContent = snippetContent.replaceAll("{{DIRECTORY}}", config.path);

// Construct the full note content
const finalContent = `# 🚀 Nx Setup: ${appName}

**Link:** [[Nx Monorepo MOC]]

${snippetContent}
`;

// 4. CREATE THE FILE
// Determine the folder: This uses the folder of the note you are currently in.
// If you want to force it to "10 - Projects", replace tp.file.folder(true) with "10 - Projects"
const currentFolder = tp.file.folder(true);
const newFileName = `Nx Setup - ${appName}`;

// Create the new note and open it immediately
await tp.file.create_new(finalContent, newFileName, true, currentFolder);
_%>
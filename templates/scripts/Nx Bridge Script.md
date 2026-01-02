<%*
// 1. CONFIGURATION
const configMap = {
    // --- APPLICATIONS (Needs Port) ---
    "React App": { 
        file: "Nx React Setup", 
        path: "apps", 
        tag: "React App",
        vars: { TYPE: "app", TYPE_LABEL: "Application", FLAGS: "--bundler=vite --style=css" }
    },
    "Node API": { 
        file: "Nx Node Setup", 
        path: "apps", 
        tag: "Node API",
        vars: { TYPE: "app", TYPE_LABEL: "API", FLAGS: "--framework=express" }
    },

    // --- SHARED LIBS (No Domain, No Port) ---
    "React Shared Lib": { 
        file: "Nx React Setup", 
        path: "libs/shared", 
        tag: "React Shared Lib",
        vars: { TYPE: "lib", TYPE_LABEL: "Library/UI", FLAGS: "--bundler=vite --style=css" }
    },
    "Node Shared Lib": { 
        file: "Nx Node Setup", 
        path: "libs/shared", 
        tag: "Node Shared Lib",
        vars: { TYPE: "lib", TYPE_LABEL: "Library/API", FLAGS: "--bundler=rollup --style=css" }
    },

    // --- DOMAIN LIBS (Prompts for Domain) ---
    "React Domain Lib": { 
        file: "Nx React Setup",
        needsDomain: true,   
        path: "libs",        
        tag: "React Domain Lib",
        vars: { TYPE: "lib", TYPE_LABEL: "Feature/UI", FLAGS: "--bundler=none --style=css" }
    },
    "Node Domain Lib": { 
        file: "Nx Node Setup", 
        needsDomain: true,
        path: "libs", 
        tag: "Node Domain Lib",
        vars: { TYPE: "lib", TYPE_LABEL: "Utility", FLAGS: "--bundler=tsc" }
    },

    // --- SPECIAL: STORYBOOK (Needs Port) ---
    "React Storybook Lib": { 
        file: "Nx React Storybook Setup", // Ensure you created this specific snippet note!
        path: "libs/shared", 
        tag: "UI Kit",
        needsPort: true, 
        vars: { TYPE: "lib", TYPE_LABEL: "UI Library" }
    }
};

const stackOptions = Object.keys(configMap);

// 2. INPUTS
const selectedOption = await tp.system.suggester(stackOptions, stackOptions);
if (!selectedOption) return; 

const config = configMap[selectedOption];
let finalDirectory = config.path;
let domainName = "";
let port = "";

// --- LOGIC A: ASK FOR DOMAIN ---
if (config.needsDomain) {
    domainName = await tp.system.prompt("Enter Domain Name (e.g., inventory, sales):");
    if (!domainName) return; 
    finalDirectory = `${config.path}/${domainName}`; 
}

// --- LOGIC B: ASK FOR PORT (For Apps or Storybook) ---
// We check if it's an App OR if the config explicitly requests a port
if (config.vars.TYPE === 'app' || config.needsPort) {
    const defaultPort = config.needsPort ? "4400" : "4200";
    port = await tp.system.prompt(`Assign a Port Number (default: ${defaultPort}):`) || defaultPort;
}

const appName = await tp.system.prompt("What is the name? (e.g., product-list)");

// 3. GENERATE CONTENT
const templateFile = tp.file.find_tfile(config.file);
let snippetContent = await tp.file.include(templateFile);

snippetContent = snippetContent.replaceAll("{{APP_NAME}}", appName);
snippetContent = snippetContent.replaceAll("{{DIRECTORY}}", finalDirectory);

// Handle the PORT placeholder (inject empty string if no port needed)
snippetContent = snippetContent.replaceAll("{{PORT}}", port ? `--port=${port}` : "");

// Replace dynamic vars (Using safe concatenation to avoid syntax errors)
for (const [key, value] of Object.entries(config.vars || {})) {
    snippetContent = snippetContent.replaceAll("{{" + key + "}}", value);
}

// 4. CONSTRUCT NOTE WITH PROPERTIES
const header = `---
status: todo
stack: "[[${config.tag}]]"
domain: "${domainName || 'Shared'}"
port: "${port}"
related_moc: "[[Nx Monorepo MOC]]"
created: ${tp.date.now("YYYY-MM-DD")}
tags:
  - nx-setup
---

# 🚀 Nx Setup: ${domainName ? domainName + '/' : ''}${appName}

`;

const finalContent = header + snippetContent;

// 5. CREATE FILE
const PROJECTS_FOLDER_NAME = "1 Projects"; 

// 1. Get the current active file's folder object
let activeFile = app.workspace.getActiveFile();
let currentFolder = activeFile ? activeFile.parent : null;
let projectRootPath = "";

// 2. Traverse UP the tree until we find the folder inside "10 - Projects"
if (currentFolder) {
    // Safety loop: stop if we hit the vault root
    while (currentFolder && currentFolder.parent) {
        // Check if the parent of the current folder is our "Projects" folder
        if (currentFolder.parent.name === PROJECTS_FOLDER_NAME) {
            projectRootPath = currentFolder.path;
            break; 
        }
        // Move up one level
        currentFolder = currentFolder.parent;
    }
}

// Fallback: If we couldn't find "10 - Projects" in the tree, 
// default to the current folder (or handle error)
if (!projectRootPath) {
    // e.g. You are running this in "Inbox" or somewhere else
    projectRootPath = tp.file.folder(true);
}

// 3. Construct target path
const targetFolder = `${projectRootPath}/docs/setup`;

// 4. Ensure folder exists
if (!app.vault.getAbstractFileByPath(targetFolder)) {
    await app.vault.createFolder(targetFolder);
}

// 5. Create File
const newFileName = `Nx Setup - ${domainName ? domainName + '-' : ''}${appName}`;
await tp.file.create_new(finalContent, newFileName, true, targetFolder);
_%>
<%*
let repoName = await tp.system.prompt("Repo name?", "my-app");
let cmd = `mkdir ${repoName} && cd ${repoName} && moon init`;
await navigator.clipboard.writeText(cmd);
new Notice("Copied: " + cmd);
tR = "";
-%>
<%*
try {
    const file = app.workspace.getActiveFile();
    
    if (!file) {
        new Notice("❌ No active file");
        return;
    }
    
    const content = await app.vault.read(file);
    const today = tp.date.now("YYYY-MM-DD HH:mm");
    
    // Check if 'updated' field exists
    if (!content.includes("updated:")) {
        new Notice("❌ No 'updated' field found in properties");
        return;
    }
    
    // Replace the updated line
    const updatedContent = content.replace(
        /updated: .+/,
        `updated: ${today}`
    );
    
    // Check if anything changed
    if (content === updatedContent) {
        new Notice("⚠️ No changes made - check your date format");
        return;
    }
    
    // Write back
    await app.vault.modify(file, updatedContent);
    
    new Notice("✓ Updated date refreshed to " + today);
    
} catch (error) {
    new Notice("❌ Error: " + error.message);
    console.error(error);
}
%>
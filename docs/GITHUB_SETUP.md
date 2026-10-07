# Publish the project on GitHub

1. Extract the ZIP. Open its `cloudora-account-takeover-investigation` folder.
2. Create a repository on GitHub named `cloudora-account-takeover-investigation`. Suggested description: "Simulated SOC account-takeover case study with KQL, persistence analysis, scoping, and a detection prototype."
3. Choose visibility. Use Add file > Upload files and upload the contents of the extracted project folder, preserving subfolders. Do not upload only the ZIP.
4. Commit the files. The root README should render as the repository home page.
5. Suggested topics: `kql`, `soc`, `incident-response`, `azure-data-explorer`, `microsoft-sentinel`, `cybersecurity-portfolio`.
6. After obtaining the logs, execute the walkthrough, add sanitized screenshots, update the evidence/report status, and commit those changes.

Optional CLI, from the project directory (replace YOUR_USERNAME):

```bash
git init
git add .
git commit -m "Add Cloudora simulated investigation case study"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/cloudora-account-takeover-investigation.git
git push -u origin main
```

Create an empty remote repository first when using this CLI path. Authenticate using GitHub's supported flow; never place a token in source files or the remote URL.

This package has not been published to a GitHub account. It excludes the original guide and training CSVs; review their permissions before redistributing them.

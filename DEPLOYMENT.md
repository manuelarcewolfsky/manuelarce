# Deployment Guide

This guide explains how to deploy your portfolio website to Azure.

## Option 1: Azure Web App (Traditional)

Use the workflow file: [.github/workflows/azure-static-web-app.yml](.github/workflows/azure-static-web-app.yml)

### Setup Steps:

1. **Create an Azure Web App** (if you haven't already):
   ```bash
   az webapp create --name your-app-name --resource-group your-rg --plan your-plan --runtime "NODE:18-lts"
   ```

2. **Configure GitHub Secrets**:
   - Go to GitHub: **Settings → Secrets and variables → Actions**
   - Add `AZURE_WEBAPP_NAME`: Your web app name
   - Add `AZURE_WEBAPP_PUBLISH_PROFILE`:
     - Download from Azure Portal → Your Web App → Get publish profile
     - Copy entire XML content as secret value

3. **Push to GitHub**:
   ```bash
   git add .
   git commit -m "Add deployment workflow"
   git push
   ```

4. **Delete the other workflow file** if using this option:
   ```bash
   rm .github/workflows/azure-static-web-apps.yml
   ```

---

## Option 2: Azure Static Web Apps (Recommended for Static Sites)

Use the workflow file: [.github/workflows/azure-static-web-apps.yml](.github/workflows/azure-static-web-apps.yml)

### Setup Steps:

1. **Create Azure Static Web App**:
   - Go to Azure Portal
   - Click "Create a resource" → Search for "Static Web Apps"
   - Click "Create"
   - Choose your subscription and resource group
   - Name your app (e.g., `manuelarce-portfolio`)
   - Select "Free" plan
   - For deployment details:
     - Sign in with GitHub
     - Select your repository: `manuelarce`
     - Branch: `main`
     - Build Presets: `Custom`
     - App location: `/`
     - Output location: leave empty

2. Azure will automatically:
   - Create the workflow file (you can replace it with the one provided)
   - Add `AZURE_STATIC_WEB_APPS_API_TOKEN` to your GitHub secrets

3. **Delete the other workflow file** if using this option:
   ```bash
   rm .github/workflows/azure-static-web-app.yml
   ```

---

## Quick Deploy Commands

After choosing your option and configuring secrets:

```bash
# Add all files
git add .

# Commit changes
git commit -m "Add Azure deployment configuration"

# Push to trigger deployment
git push origin main
```

## Verify Deployment

After pushing:
1. Go to your GitHub repository
2. Click **Actions** tab
3. Watch the deployment workflow run
4. Once complete, visit your Azure URL

---

## Troubleshooting

### Error: "MSBUILD : error MSB1003"
This means the workflow is trying to build a .NET project. You're deploying static HTML, so use one of the workflows provided above.

### Deployment succeeds but site shows default page
Ensure your `index.html` is in the root directory (not in a subdirectory).

### 404 errors
Check that your Azure Web App is configured to serve static files properly.

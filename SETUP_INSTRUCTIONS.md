# Setup Instructions for trusys-llm-security-scan-action Repository

This document explains how to set up the new `trusys-llm-security-scan-action` repository for GitHub Marketplace publishing.

## Step 1: Create the Repository on GitHub

1. Go to GitHub and create a **new public repository**:
   - **Name**: `trusys-llm-security-scan-action`
   - **Owner**: `spydra-tech` (or your organization)
   - **Visibility**: **Public** (required for Marketplace)
   - **Description**: "GitHub Action for scanning LLM security vulnerabilities"
   - **Do NOT** initialize with README, .gitignore, or license (we'll add these files)

## Step 2: Copy Files to the New Repository

Copy all files from the `trusys-llm-security-scan-action/` folder in this repo to the root of the new GitHub repository:

- `action.yml` (must be at the root)
- `README.md`
- `LICENSE`
- `.gitignore`

## Step 3: Initialize and Push

```bash
# Navigate to the new repo directory
cd trusys-llm-security-scan-action

# Initialize git (if not already done)
git init

# Add remote (replace with your actual repo URL)
git remote add origin https://github.com/spydra-tech/trusys-llm-security-scan-action.git

# Add files
git add action.yml README.md LICENSE .gitignore

# Commit
git commit -m "Initial commit: LLM Security Scan GitHub Action"

# Push to main branch
git branch -M main
git push -u origin main
```

## Step 4: Accept GitHub Marketplace Developer Agreement

Before you can publish, you need to accept the Marketplace Developer Agreement:

1. Go to your GitHub **Settings** → **Developer settings** → **GitHub Marketplace**
2. Or, if it's an organization: **Organization Settings** → **Developer settings** → **GitHub Marketplace**
3. Accept the **GitHub Marketplace Developer Agreement**

## Step 5: Create a Release and Publish to Marketplace

1. **On GitHub**, navigate to the new repository: `https://github.com/spydra-tech/trusys-llm-security-scan-action`

2. **Open `action.yml`** in the browser (click on the file)

3. You should see a **banner** at the top saying something like:
   > "Publish this Action to the GitHub Marketplace"
   > [Draft a release]

4. **Click "Draft a release"** (or go to **Releases** → **Draft a new release**)

5. **Fill in the release form**:
   - **Tag**: `v1.0.0` (or your version)
   - **Release title**: `LLM Security Scan Action v1.0.0`
   - **Description**: Brief description of the action

6. **Under "Release Action"** section, you should now see:
   - ☑️ **"Publish this Action to the GitHub Marketplace"** checkbox
   - **Primary Category**: Select "Security" or "Code quality"
   - **Another Category** (optional): Select a secondary category
   - **Action name**: Should auto-fill as "LLM Security Scan" (from `action.yml`)

7. **Check the checkbox** "Publish this Action to the GitHub Marketplace"

8. **Click "Publish release"**

## Step 6: Verify Publication

After publishing:

1. Go to [GitHub Marketplace](https://github.com/marketplace)
2. Search for "LLM Security Scan" or "trusys-llm-security-scan-action"
3. Your action should appear in the search results

## Step 7: Users Can Now Use It

Users can reference your action as:

```yaml
uses: spydra-tech/trusys-llm-security-scan-action@v1.0.0
# or
uses: spydra-tech/trusys-llm-security-scan-action@v1  # latest v1.x.x
```

## Important Notes

- **No workflow files**: This repository should **NOT** contain any `.github/workflows/*.yml` files. GitHub requires action-only repos for Marketplace publishing.
- **Single action.yml at root**: The `action.yml` must be at the repository root, not in a subfolder.
- **Public repository**: The repository must be public for Marketplace publishing.
- **Unique name**: The `name` in `action.yml` ("LLM Security Scan") must be unique on GitHub Marketplace.

## Updating the Action

To publish updates:

1. Make changes to `action.yml` or `README.md`
2. Commit and push to `main`
3. Create a new release with a new tag (e.g., `v1.0.1`)
4. Check "Publish this Action to the GitHub Marketplace" again
5. The Marketplace listing will update with the new version

## Troubleshooting

### Banner not appearing
- Ensure `action.yml` is at the **root** of the repository
- Ensure the repository is **public**
- Ensure you've accepted the **GitHub Marketplace Developer Agreement**

### Checkbox disabled
- Accept the Marketplace Developer Agreement (see Step 4)
- If it's an organization, an **owner** must accept the agreement

### Action not found in Marketplace search
- Wait a few minutes after publishing (indexing can take time)
- Search for the exact `name` from `action.yml` ("LLM Security Scan")
- Check that the release was successfully published

# PR Build Check Workflow

## Overview

A GitHub Actions workflow that automatically runs build checks on pull requests. This ensures all PRs are tested before merging into your main branch.

## What This Workflow Does

When someone creates a pull request to your `main` branch, this workflow will:

1. Check out the code
2. Set up Node.js v22 with npm caching
3. Install dependencies with `npm ci`
4. Run your build command with `npm run build`

## How to Add This to Your Project

### Step 1: Copy the Workflow File

1. In your repository, create the folder structure: `.github/workflows/`
2. Create a new file called `pr-build-check.yml` inside the `workflows` folder
3. Copy and paste the workflow code (shown at the bottom of this README)

### Step 2: Ensure Your Project is Ready

Your project needs:

- A `package.json` file with a `build` script
- Example `package.json`:
```json
{
  "name": "your-project",
  "scripts": {
    "build": "your-build-command-here"
  }
}
```

### Step 3: Test It

1. Create a test branch: `git checkout -b test-workflow`
2. Make any small change and commit it
3. Push the branch: `git push origin test-workflow`
4. Create a pull request to `main`
5. You should see the "PR Build Check" running in the PR

## Optional: Require This Check Before Merging

To make this build check mandatory before merging PRs:

1. Go to your repository **Settings** → **Branches**
2. Click "Add branch protection rule"
3. Set branch name pattern to: `main`
4. Check "Require status checks to pass before merging"
5. In the search box, type: **PR Build Check**
6. Select it and click "Save changes"

Now no one can merge a PR until the build passes!

## Common Build Scripts

Depending on your project type, add one of these to your `package.json`:

**React/Vite:**
```json
"scripts": {
  "build": "vite build"
}
```

**Next.js:**
```json
"scripts": {
  "build": "next build"
}
```

**TypeScript:**
```json
"scripts": {
  "build": "tsc"
}
```

**Custom:**
```json
"scripts": {
  "build": "your-build-command"
}
```

## Workflow File

The workflow is located at `.github/workflows/pr-build.yml`:

```yaml
name: PR Build Check
on:
  pull_request:
    branches: [main]

jobs:
  build:
    name: PR Build Check
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: '22'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run build
```

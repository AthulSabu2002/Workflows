# PR Build Check

## Overview

This repository contains a GitHub Actions workflow that automatically runs build checks on pull requests to the main branch.

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── pr-build.yml
├── package.json
├── package-lock.json
└── README.md
```

## Workflow Details

The workflow (`PR Build Check`) triggers on pull requests to the main branch and performs the following steps:

1. Checks out the code
2. Sets up Node.js v22 with npm caching
3. Installs dependencies with `npm ci`
4. Runs the build command with `npm run build`

## Setup Instructions

1. Create a `package.json` file with a `build` script
2. Ensure your project builds successfully with `npm run build`
3. Push the workflow file to `.github/workflows/pr-build-check.yml`

## Branch Protection

You can protect the main branch by requiring this workflow to pass before merging:

1. Go to **Settings** → **Branches** in your repository
2. Add a branch protection rule for `main`
3. Enable "Require status checks to pass before merging"
4. Search for and select the status check: **PR Build Check**
5. Save the protection rule

This ensures all pull requests must pass the build check before they can be merged into main.

## Requirements

- Node.js project with `package.json`
- Build script defined in `package.json`
- npm lockfile (`package-lock.json`)

## Workflow File

The workflow is located at `.github/workflows/pr-build-check.yml`:

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

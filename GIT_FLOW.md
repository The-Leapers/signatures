# Git Flow Guide

**⚠️ CRITICAL: Always work on the `develop` branch. Never commit directly to `main`.**

This repository follows a simplified Git Flow workflow with two main branches: `main` and `develop`.

## Branch Overview

### `main` Branch
- **Purpose**: Production-ready code
- **Deployment**: Automatically deployed to GitHub Pages
- **Protection**: **NEVER commit directly to this branch**
- **Updates**: Only receives code via **Pull Requests** from `develop`
- **Status**: Always stable and ready for production
- **Branch Protection**: Should be configured to require pull requests (see setup below)

### `develop` Branch
- **Purpose**: Development and testing
- **Usage**: **This is where ALL development work happens**
- **Workflow**: All new signatures, updates, and changes are made here first
- **Merging**: Changes are merged to `main` when ready for production
- **Sync**: Should be kept in sync with `main` (merge `main` into `develop` regularly)

## Workflow

### ⚠️ Always Start on Develop Branch

**Before starting any work, always ensure you're on the develop branch:**

```bash
git checkout develop
git pull origin develop
```

### Creating a New Signature

1. **Switch to develop branch** (if not already there)
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Create your feature branch** (optional, for larger changes)
   ```bash
   git checkout -b feature/new-signature-name
   ```
   Or work directly on `develop` for simple additions.

3. **Make your changes**
   - Copy `template.html` to create the new signature
   - Add profile image
   - Update `index.html` with the new link
   - Test locally

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add signature for [Name]"
   ```

5. **Push to develop**
   ```bash
   git push origin develop
   # or if using feature branch:
   git push origin feature/new-signature-name
   ```

6. **Test on develop branch**
   - Verify the signature looks correct
   - Check that all images load properly
   - Test the link in `index.html`

### Deploying to Production (Using Pull Requests)

**⚠️ IMPORTANT: Always use Pull Requests to merge `develop` into `main`. Never merge directly.**

1. **Ensure all changes are pushed to develop**
   ```bash
   git checkout develop
   git push origin develop
   ```

2. **Create a Pull Request on GitHub**
   - Go to: https://github.com/The-Leapers/signatures
   - Click the **"Pull requests"** tab
   - Click **"New pull request"**
   - Set **base branch**: `main`
   - Set **compare branch**: `develop`
   - Click **"Create pull request"**

3. **Fill out the Pull Request**
   - **Title**: Descriptive title (e.g., "Deploy new signatures to production")
   - **Description**: List what's being deployed:
     - New signatures added
     - Updates made
     - Any other changes
   - Add reviewers if needed
   - Click **"Create pull request"**

4. **Review and Merge**
   - Reviewers can review the changes
   - Check that all signatures display correctly
   - Once approved, click **"Merge pull request"**
   - Choose merge type (usually "Create a merge commit")
   - Click **"Confirm merge"**

5. **GitHub Pages will automatically rebuild**
   - Wait 1-2 minutes after merge
   - Visit https://the-leapers.github.io/signatures/
   - Verify the changes are live

6. **Update develop** (sync with main)
   ```bash
   git checkout develop
   git pull origin develop
   git merge main  # Sync any changes from main
   git push origin develop
   ```

### Alternative: Using GitHub CLI

If you have GitHub CLI installed, you can create a PR from the command line:

```bash
gh pr create --base main --head develop --title "Deploy to production" --body "Description of changes"
```

## Common Tasks

### Updating an Existing Signature

1. Checkout develop branch
   ```bash
   git checkout develop
   ```

2. Make your changes to the signature file in `active/`

3. Commit and push
   ```bash
   git add active/[signature-name].html
   git commit -m "Update signature for [Name]"
   git push origin develop
   ```

4. Test on develop, then create a Pull Request to merge to main when ready

### Archiving a Signature

1. Move the signature file from `active/` to `archive/`
2. Move the profile image from `active/` to `archive/`
3. Remove the link from `index.html`
4. Commit the changes
   ```bash
   git add .
   git commit -m "Archive signature for [Name]"
   git push origin develop
   ```

### Hotfix (Urgent Production Fix)

If you need to fix something directly in production:

1. **Create hotfix branch from main**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/description
   ```

2. **Make the fix and commit**
   ```bash
   git add .
   git commit -m "Hotfix: [description]"
   git push origin hotfix/description
   ```

3. **Create Pull Request for hotfix**
   - Go to GitHub and create a PR from `hotfix/description` to `main`
   - Mark it as urgent/hotfix in the description
   - Get quick approval and merge

4. **Merge hotfix to develop**
   ```bash
   git checkout develop
   git pull origin develop
   git merge hotfix/description
   git push origin develop
   ```

5. **Delete hotfix branch**
   ```bash
   git branch -d hotfix/description
   git push origin --delete hotfix/description
   ```

## Best Practices

1. **Always pull before starting work**
   ```bash
   git pull origin develop
   ```

2. **Write clear commit messages**
   - Use present tense: "Add signature" not "Added signature"
   - Be descriptive: "Add signature for John Doe" not "Update file"

3. **Test before merging to main**
   - Verify signatures display correctly
   - Check all image paths work
   - Test on GitHub Pages after deployment

4. **Keep commits focused**
   - One signature per commit when possible
   - Separate commits for different types of changes

5. **Regular Pull Requests**
   - Create PRs from develop to main regularly (weekly or after each signature)
   - Don't let develop get too far ahead of main
   - Always use Pull Requests, never merge directly to main

## Troubleshooting

### Merge Conflicts

If you encounter merge conflicts:

1. **Resolve conflicts in your editor**
2. **Stage the resolved files**
   ```bash
   git add [resolved-files]
   ```
3. **Complete the merge**
   ```bash
   git commit
   ```

### Undoing Changes

**Before committing:**
```bash
git checkout -- [file]  # Discard changes to a file
git reset HEAD [file]    # Unstage a file
```

**After committing (but before pushing):**
```bash
git reset --soft HEAD~1  # Undo commit, keep changes
git reset --hard HEAD~1  # Undo commit and discard changes
```

**After pushing:**
- Create a new commit to fix the issue
- Or use `git revert` to create a commit that undoes previous changes

## Setting Up Branch Protection (Repository Administrators)

To enforce the Pull Request workflow, set up branch protection rules for `main`:

1. **Go to Repository Settings**
   - Navigate to: https://github.com/The-Leapers/signatures/settings
   - Click **"Branches"** in the left sidebar

2. **Add Branch Protection Rule**
   - Click **"Add branch protection rule"**
   - Under **"Branch name pattern"**, enter: `main`
   - Enable the following options:
     - ✅ **Require a pull request before merging**
       - ✅ Require approvals (optional, set number if desired)
       - ✅ Dismiss stale pull request approvals when new commits are pushed
     - ✅ **Require status checks to pass before merging** (optional)
     - ✅ **Require branches to be up to date before merging**
     - ✅ **Do not allow bypassing the above settings** (recommended)
     - ✅ **Restrict who can push to matching branches** (optional, for extra security)

3. **Save the Rule**
   - Click **"Create"** or **"Save changes"**

This ensures that:
- No one can push directly to `main`
- All changes must go through Pull Requests
- Code can be reviewed before merging
- The production branch stays stable

## Quick Reference

```bash
# Switch branches
git checkout develop
git checkout main

# Create and switch to new branch
git checkout -b feature/branch-name

# Update from remote
git pull origin [branch-name]

# Push changes
git push origin [branch-name]

# Create Pull Request (use GitHub web interface or CLI)
# Never merge directly to main - always use Pull Requests

# View branch status
git branch -a
git status
```


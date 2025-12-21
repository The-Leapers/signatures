# Git Flow Guide

This repository follows a simplified Git Flow workflow with two main branches: `main` and `develop`.

## Branch Overview

### `main` Branch
- **Purpose**: Production-ready code
- **Deployment**: Automatically deployed to GitHub Pages
- **Protection**: Should only receive code via merges from `develop`
- **Status**: Always stable and ready for production

### `develop` Branch
- **Purpose**: Development and testing
- **Usage**: Where all new signatures and changes are developed
- **Merging**: Changes are merged to `main` when ready for production

## Workflow

### Creating a New Signature

1. **Switch to develop branch**
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

### Deploying to Production

1. **Merge develop to main**
   ```bash
   git checkout main
   git pull origin main
   git merge develop
   git push origin main
   ```

2. **GitHub Pages will automatically rebuild**
   - Wait 1-2 minutes
   - Visit https://the-leapers.github.io/signatures/
   - Verify the changes are live

3. **Update develop** (if needed)
   ```bash
   git checkout develop
   git merge main  # Sync any hotfixes
   git push origin develop
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

4. Test, then merge to main when ready

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
   ```

3. **Merge to main and develop**
   ```bash
   git checkout main
   git merge hotfix/description
   git push origin main
   
   git checkout develop
   git merge hotfix/description
   git push origin develop
   ```

4. **Delete hotfix branch**
   ```bash
   git branch -d hotfix/description
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

5. **Regular merges**
   - Merge develop to main regularly (weekly or after each signature)
   - Don't let develop get too far ahead of main

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

# Merge develop into main
git checkout main
git merge develop
git push origin main

# View branch status
git branch -a
git status
```


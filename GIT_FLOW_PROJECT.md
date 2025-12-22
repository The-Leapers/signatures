# Git Flow Guide - Email Signatures Project

**⚠️ CRITICAL: Always work on the `develop` branch. Never commit directly to `main`.**

This is a project-specific Git Flow guide for the Email Signatures repository. For general Git Flow concepts and advanced topics, see `GIT_FLOW.md`.

## Project Overview

- **Repository**: Email signatures for The Leapers team
- **Deployment**: GitHub Pages (https://the-leapers.github.io/signatures/)
- **Branch Strategy**: Simplified Git Flow (main + develop)
- **Versioning**: Semantic versioning (v1.0.0, v1.1.0, v2.0.0)

## Quick Start

### Daily Workflow

1. **Start working**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Make changes** (create/update signatures)

3. **Test locally**
   - Open `index.html` in browser
   - Check all signatures display correctly
   - Verify images load

4. **Commit and push**
   ```bash
   git add .
   git commit -m "Add signature for [Name]"
   git push origin develop
   ```

5. **Create Pull Request to main** when ready to deploy

## Project-Specific Workflows

### Creating a New Signature

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Copy template**
   ```bash
   cp template.html active/[firstname].html
   ```

3. **Add profile image**
   - Add `active/[firstname].png` (120px wide, PNG format)
   - Ensure image is optimized for web

4. **Update signature file**
   - Open `active/[firstname].html`
   - Update profile image `src` and `alt` text (line ~5)
   - Update full name (line ~10)
   - Update job title/position (line ~11)
   - Verify image paths use `../` for shared assets (logos, banners)

5. **Update index.html**
   - Add link to new signature in the list
   - Format: `<li><a href="active/[firstname].html">Full Name</a></li>`

6. **Test locally**
   ```bash
   # Open in browser
   open index.html
   # Or use a local server
   python3 -m http.server 8000
   # Visit http://localhost:8000
   ```
   - Verify signature displays correctly
   - Check all images load (profile, logos, banners)
   - Test link from index page
   - Verify responsive behavior

7. **Commit changes**
   ```bash
   git add active/[firstname].html active/[firstname].png index.html
   git commit -m "Add signature for [Full Name]"
   git push origin develop
   ```

8. **Create Pull Request** (see "Deploying to Production" below)

### Updating an Existing Signature

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Make changes**
   - Update signature HTML file in `active/`
   - Update profile image if needed (replace `active/[name].png`)
   - Update `index.html` if name/title changed

3. **Test locally**
   - Open signature file directly
   - Check from index page
   - Verify all images load

4. **Commit and push**
   ```bash
   git add active/[name].html active/[name].png index.html
   git commit -m "Update signature for [Name]: [what changed]"
   git push origin develop
   ```

5. **Create Pull Request** when ready

### Archiving a Signature

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Move files to archive**
   ```bash
   git mv active/[name].html archive/[name].html
   git mv active/[name].png archive/[name].png
   ```

3. **Update index.html**
   - Remove the link from the active signatures list

4. **Test**
   - Verify signature is removed from index
   - Confirm files are in archive folder

5. **Commit and push**
   ```bash
   git add .
   git commit -m "Archive signature for [Name]"
   git push origin develop
   ```

6. **Create Pull Request** when ready

### Updating Shared Assets (Logos, Banners)

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Update assets in root directory**
   - Replace logo files: `leapers.png`, `tezgah.png`, `totb.png`, `KATALOQ.png`
   - Replace banners: `banner1.png`, `banner2.png`
   - Archive old versions if needed: `archive/banner1Archived[date].png`

3. **Test all signatures**
   - Open multiple signature files
   - Verify all logos and banners display correctly
   - Check image paths are correct

4. **Commit and push**
   ```bash
   git add *.png archive/*.png
   git commit -m "Update shared assets: [description]"
   git push origin develop
   ```

5. **Create Pull Request** when ready

### Updating Contact Information

**⚠️ This affects all signatures - coordinate with team**

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Update template.html** first
   - Update contact information section
   - Verify formatting

3. **Update all active signatures**
   - Use find/replace across `active/*.html` files
   - Or update each file individually
   - Ensure consistency

4. **Test thoroughly**
   - Check multiple signatures
   - Verify phone numbers and addresses
   - Test on different email clients if possible

5. **Commit and push**
   ```bash
   git add active/*.html template.html
   git commit -m "Update contact information: [what changed]"
   git push origin develop
   ```

6. **Create Pull Request** with clear description of changes

## Testing Checklist

Before creating a Pull Request to `main`, verify:

### Visual Testing
- [ ] All active signatures display correctly
- [ ] Profile images load and display properly
- [ ] Company logos display correctly
- [ ] Banners display correctly
- [ ] Signature layout looks good
- [ ] Text is readable and properly formatted
- [ ] No broken images (check browser console)

### Functional Testing
- [ ] Index page lists all active signatures
- [ ] All links from index page work
- [ ] Image paths are correct (check for 404 errors)
- [ ] Shared assets load from root directory
- [ ] Profile images load from active folder

### Code Quality
- [ ] HTML is valid (use validator if needed)
- [ ] No broken image paths
- [ ] Consistent formatting across signatures
- [ ] Contact information is consistent
- [ ] No test/debug code left in files

### GitHub Pages Testing
- [ ] After merge, verify site at https://the-leapers.github.io/signatures/
- [ ] All signatures accessible via GitHub Pages
- [ ] Images load correctly on GitHub Pages
- [ ] No 404 errors in browser console

## Deploying to Production

### Pre-Deployment Checklist

- [ ] All changes tested locally
- [ ] All signatures verified
- [ ] Index page updated if needed
- [ ] No broken links or images
- [ ] Contact information is current
- [ ] README.md updated if structure changed

### Creating Pull Request

1. **Ensure all changes are on develop**
   ```bash
   git checkout develop
   git push origin develop
   ```

2. **Create Pull Request on GitHub**
   - Go to: https://github.com/The-Leapers/signatures
   - Click **"Pull requests"** → **"New pull request"**
   - Base: `main` ← Compare: `develop`
   - Click **"Create pull request"**

3. **Fill out Pull Request**
   - **Title**: `Deploy signatures: [brief description]`
   - **Description**: Use template and include:
     - New signatures added (if any)
     - Updated signatures (if any)
     - Archived signatures (if any)
     - Shared asset updates (if any)
     - Contact information changes (if any)
   - Add screenshots if visual changes
   - Request review

4. **Review and Merge**
   - Reviewer checks the changes
   - Verify signatures look correct
   - Once approved, merge the PR

5. **Tag the Release** (optional but recommended)
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.1.0 -m "Release v1.1.0: Add signatures for [names]"
   git push origin v1.1.0
   ```

6. **Verify Deployment**
   - Wait 1-2 minutes for GitHub Pages to rebuild
   - Visit: https://the-leapers.github.io/signatures/
   - Test a few signatures
   - Check browser console for errors

7. **Sync develop**
   ```bash
   git checkout develop
   git pull origin develop
   git merge main
   git push origin develop
   ```

## Versioning Strategy

For this project, we use **semantic versioning**:

- **Format**: `vMAJOR.MINOR.PATCH` (e.g., `v1.0.0`, `v1.1.0`, `v2.0.0`)
- **MAJOR**: Breaking changes or major updates (rare)
- **MINOR**: New signatures added, new features
- **PATCH**: Bug fixes, updates to existing signatures
- **When to tag**: After each merge to main
- **Tag message**: Brief description of what's in the release

### Creating Release Tags

```bash
# After merging PR to main
git checkout main
git pull origin main

# Create tag (increment version appropriately)
git tag -a v1.1.0 -m "Release v1.1.0: Add signatures for John and Jane"

# Push tag
git push origin v1.1.0
```

### Version Increment Guidelines

- **v1.0.0 → v1.1.0**: Adding new signatures, new features
- **v1.1.0 → v1.1.1**: Fixing bugs, updating existing signatures
- **v1.1.0 → v2.0.0**: Major structural changes, breaking changes

### Viewing Release History

```bash
# List all tags
git tag

# View specific release
git show v1.1.0

# View release notes between versions
git log --oneline v1.0.0..v1.1.0
```

## Hotfix Workflow

If a critical issue is found in production:

1. **Create hotfix branch from main**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/fix-broken-image
   ```

2. **Fix the issue**
   - Fix broken image path
   - Fix incorrect contact info
   - Fix any critical bug

3. **Test the fix**
   - Verify fix works locally
   - Test affected signatures

4. **Create Pull Request**
   - PR from `hotfix/fix-broken-image` to `main`
   - Mark as urgent
   - Get quick approval and merge

5. **Tag hotfix**
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.0.1 -m "Hotfix v1.0.1: Fix broken image in signature"
   git push origin v1.0.1
   ```

6. **Merge to develop**
   ```bash
   git checkout develop
   git merge hotfix/fix-broken-image
   git push origin develop
   ```

7. **Delete hotfix branch**
   ```bash
   git branch -d hotfix/fix-broken-image
   git push origin --delete hotfix/fix-broken-image
   ```

## Rollback Procedure

If a deployment causes issues:

### Method 1: Revert via Pull Request (Recommended)

1. **Create revert PR**
   - Create PR from previous working commit to main
   - Or use `git revert` to create a revert commit
   - Merge the revert PR

2. **Tag the rollback**
   ```bash
   git tag -a v1.0.1 -m "Rollback v1.0.1: Revert problematic changes from v1.1.0"
   git push origin v1.0.1
   ```

3. **Fix issues on develop**
   - Identify and fix the problems
   - Test thoroughly
   - Create new PR when ready

### Method 2: Quick Fix via Hotfix

1. **Create hotfix branch**
2. **Fix the issues**
3. **Deploy via PR**
4. **Tag the fix**

## Project-Specific Best Practices

1. **Always test locally first**
   - Open HTML files in browser
   - Check all images load
   - Verify links work

2. **Keep signatures consistent**
   - Use the same template structure
   - Maintain consistent formatting
   - Keep contact information synchronized

3. **Optimize images**
   - Profile images should be ~120px wide
   - Compress PNG files for web
   - Keep file sizes reasonable

4. **Update index.html**
   - Always update when adding/removing signatures
   - Keep list alphabetical or organized
   - Verify all links work

5. **Document changes**
   - Clear commit messages
   - Descriptive PR descriptions
   - Note any breaking changes

6. **Coordinate contact info updates**
   - Contact info changes affect all signatures
   - Coordinate with team before updating
   - Test thoroughly after updates

## Common Issues and Solutions

### Images Not Loading on GitHub Pages

**Problem**: Images show broken on GitHub Pages but work locally

**Solution**:
- Verify image paths use `../` for root directory assets
- Check that profile images are in `active/` folder
- Ensure file names match exactly (case-sensitive)
- Check file extensions (.png vs .PNG)

### Signature Not Appearing in Index

**Problem**: Added signature but link not showing

**Solution**:
- Verify `index.html` was updated
- Check HTML syntax (closing tags)
- Verify file path in link matches actual file location

### Styling Issues

**Problem**: Signature looks different than expected

**Solution**:
- Check inline styles are preserved
- Verify table structure is intact
- Test in different email clients if possible
- Check for missing closing tags

### Merge Conflicts

**Problem**: Conflicts when merging develop to main

**Solution**:
- Usually happens when multiple people edit same files
- Resolve conflicts manually
- Test after resolving
- Coordinate with team to avoid conflicts

## Quick Reference

```bash
# Start working
git checkout develop
git pull origin develop

# Create new signature
cp template.html active/[name].html
# Edit file, add image, update index.html

# Commit changes
git add active/[name].html active/[name].png index.html
git commit -m "Add signature for [Name]"
git push origin develop

# Create Pull Request (use GitHub web interface)
# Or with GitHub CLI:
gh pr create --base main --head develop --title "Deploy signatures"

# After PR merged, tag release
git checkout main
git pull origin main
git tag -a v1.1.0 -m "Release v1.1.0: [description]"
git push origin v1.1.0

# Sync develop
git checkout develop
git merge main
git push origin develop

# View tags
git tag
git show v1.1.0
```

## Project Structure Reference

```
.
├── active/              # Active signature files and images
│   ├── [name].html      # Signature HTML files
│   └── [name].png       # Profile images
├── archive/             # Archived signatures
├── .github/             # GitHub templates
├── index.html           # Landing page
├── template.html        # Signature template
├── *.png                # Shared assets (logos, banners)
├── README.md
├── GIT_FLOW.md          # General Git Flow guide
└── GIT_FLOW_PROJECT.md  # This file
```

## Additional Resources

- **General Git Flow**: See `GIT_FLOW.md` for comprehensive Git Flow guide
- **GitHub Pages Setup**: See `GITHUB_PAGES_SETUP.md`
- **Template Instructions**: See `template.html` for inline comments
- **Repository**: https://github.com/The-Leapers/signatures
- **Live Site**: https://the-leapers.github.io/signatures/

## Getting Help

- Check `GIT_FLOW.md` for general Git Flow questions
- Review `template.html` for signature creation help
- Check GitHub Pages setup in `GITHUB_PAGES_SETUP.md`
- Review commit history for examples: `git log --oneline`


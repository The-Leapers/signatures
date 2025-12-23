# Git Flow Guide

**⚠️ CRITICAL: Always work on the `develop` branch. Never commit directly to `main`.**

This repository follows Git Flow workflow with two main branches: `main` and `develop`, plus optional feature, release, and hotfix branches.

## Branch Overview

### `main` Branch
- **Purpose**: Production-ready code
- **Deployment**: Automatically deployed to production
- **Protection**: **NEVER commit directly to this branch**
- **Updates**: Only receives code via **Pull Requests** from `develop` or `release/*` branches
- **Status**: Always stable and ready for production
- **Tags**: All production releases are tagged here
- **Branch Protection**: Should be configured to require pull requests (see setup below)

### `develop` Branch
- **Purpose**: Development and integration
- **Usage**: **This is where ALL development work happens**
- **Workflow**: All new features, updates, and changes are made here first
- **Merging**: Changes are merged to `main` via Pull Requests when ready for production
- **Sync**: Should be kept in sync with `main` (merge `main` into `develop` regularly)

### Feature Branches
- **Purpose**: Develop new features
- **Naming**: `feature/feature-name` or `feature/issue-number-feature-name`
- **Source**: Created from `develop`
- **Destination**: Merged back to `develop` via Pull Request
- **Lifecycle**: Deleted after merging

### Release Branches
- **Purpose**: Prepare for a new production release
- **Naming**: `release/v1.0.0` (use semantic versioning)
- **Source**: Created from `develop` when ready to release (after features are merged)
- **Destination**: Merged to both `main` (for release) and `develop` (to sync fixes back)
- **Lifecycle**: Deleted after merging
- **Important**: Features merge to `develop` first, then release branch is created FROM `develop`. Do not merge features into release branches.

### Hotfix Branches
- **Purpose**: Fix critical production issues
- **Naming**: `hotfix/description` or `hotfix/v1.0.1`
- **Source**: Created from `main`
- **Destination**: Merged to both `main` (for fix) and `develop` (to sync)
- **Lifecycle**: Deleted after merging

## Version Numbering Strategy

### Semantic Versioning (Recommended)

Follow [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`

- **MAJOR** (v1.0.0 → v2.0.0): Breaking changes, incompatible API changes
- **MINOR** (v1.0.0 → v1.1.0): New features, backward-compatible
- **PATCH** (v1.0.0 → v1.0.1): Bug fixes, backward-compatible

### Tag Naming Convention

- **Format**: `v1.0.0` (preferred) or `1.0.0`
- **Always prefix with `v`** for consistency
- **Examples**: `v1.0.0`, `v1.2.3`, `v2.0.0-beta.1`

## Workflow

### ⚠️ Always Start on Develop Branch

**Before starting any work, always ensure you're on the develop branch:**

```bash
git checkout develop
git pull origin develop
```

### Feature Development

#### When to Use Feature Branches

**Use feature branches for:**
- New features that take multiple commits
- Work that needs code review before merging to develop
- Experimental features that might be abandoned
- Work by multiple developers on the same feature

**Work directly on develop for:**
- Small bug fixes
- Quick documentation updates
- Simple changes that don't need isolation

#### Creating a Feature Branch

**⚠️ IMPORTANT: Feature branches always merge into `develop`, never into release branches. Release branches are created FROM `develop` after features are merged.**

1. **Start from develop**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. **Create feature branch**
   ```bash
   git checkout -b feature/feature-name
   # or with issue number:
   git checkout -b feature/123-add-user-authentication
   ```

3. **Make your changes**
   - Write code
   - Add tests
   - Update documentation
   - Test locally

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add feature: [description]"
   ```

5. **Push feature branch**
   ```bash
   git push origin feature/feature-name
   ```

6. **Create Pull Request to develop** (NOT to release branches)
   - Go to GitHub repository
   - Click **"Pull requests"** → **"New pull request"**
   - Base: `develop` ← Compare: `feature/feature-name`
   - Fill out PR template (see below)
   - Request review
   - Merge after approval
   
   **Note**: Even if a release branch exists, merge features to `develop`. The release branch will be updated when it merges back to `develop`.

7. **Delete feature branch** (after merge)
   ```bash
   git checkout develop
   git branch -d feature/feature-name
   git push origin --delete feature/feature-name
   ```

### Understanding Branch Relationships

#### Feature Branches → Develop → Release Branches

**Feature branches ALWAYS merge into `develop`, never into release branches:**

```
feature/new-feature → develop → release/v1.0.0 → main
```

**Workflow:**
1. Create feature branch from `develop`
2. Develop and test feature
3. Merge feature branch → `develop` (via Pull Request)
4. When ready to release, create release branch FROM `develop`
5. Release branch contains all features that were in `develop`
6. Fix release-blocking bugs on release branch
7. Merge release branch → `main` (for production)
8. Merge release branch → `develop` (to sync fixes back)

**During Release Branch Phase:**
- ✅ New features continue to be merged to `develop` for the next release
- ✅ Only release-blocking fixes go on the release branch
- ❌ Do NOT merge new features into the release branch
- ✅ The release branch is isolated for final testing and preparation

#### Hotfixes Are Separate

**Hotfixes come from `main`, not from release branches:**

```
main → hotfix/critical-bug → main + develop
```

- Hotfixes are created from `main` when production has a critical issue
- They merge to both `main` (immediate fix) and `develop` (to prevent regression)
- Hotfixes are independent of release branches

### Release Process

#### Pre-Release Checklist

Before creating a release, ensure:

- [ ] All features for this release are merged to `develop`
- [ ] All tests pass
- [ ] Documentation is updated
- [ ] CHANGELOG.md is updated with new features/fixes
- [ ] Version numbers are updated in code/config files
- [ ] No known critical bugs
- [ ] Code review completed for all changes

#### Creating a Release Branch

**⚠️ IMPORTANT: Release branches are created FROM `develop` after all features for the release have been merged. Feature branches merge into `develop`, NOT into release branches.**

1. **Ensure develop is ready**
   ```bash
   git checkout develop
   git pull origin develop
   ```
   
   **Note**: At this point, `develop` should already contain all features that will be in this release. Feature branches merge to `develop` first, then you create the release branch from `develop`.

2. **Create release branch FROM develop**
   ```bash
   git checkout -b release/v1.0.0
   git push origin release/v1.0.0
   ```
   
   **What's in the release branch**: All features that were merged to `develop` are now in this release branch. The release branch is a snapshot of `develop` at this point.

3. **Finalize release**
   - Update version numbers in code
   - Update CHANGELOG.md
   - Fix any release-blocking bugs (commit directly to release branch)
   - Test thoroughly
   - Commit final changes
   ```bash
   git add .
   git commit -m "Prepare release v1.0.0"
   git push origin release/v1.0.0
   ```
   
   **During release preparation**:
   - ✅ Fix release-blocking bugs directly on the release branch
   - ✅ Update version numbers and documentation
   - ❌ Do NOT merge new features into the release branch
   - ✅ New features continue to be merged to `develop` for the next release

4. **Create Pull Request to main**
   - Base: `main` ← Compare: `release/v1.0.0`
   - Title: `Release v1.0.0`
   - Include release notes in description
   - Get approval and merge

5. **Tag the release** (after merging to main)
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.0.0 -m "Release v1.0.0"
   git push origin v1.0.0
   ```

6. **Merge release branch back to develop**
   ```bash
   git checkout develop
   git pull origin develop
   git merge release/v1.0.0
   git push origin develop
   ```

7. **Delete release branch**
   ```bash
   git branch -d release/v1.0.0
   git push origin --delete release/v1.0.0
   ```

#### Quick Release (Without Release Branch)

For smaller projects or quick releases:

1. **Create Pull Request from develop to main**
   - Include release notes
   - Get approval and merge

2. **Tag the release**
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.0.0 -m "Release v1.0.0"
   git push origin v1.0.0
   ```

3. **Sync develop**
   ```bash
   git checkout develop
   git merge main
   git push origin develop
   ```

### Deploying to Production (Using Pull Requests)

**⚠️ IMPORTANT: Always use Pull Requests to merge `develop` into `main`. Never merge directly.**

1. **Ensure all changes are pushed to develop**
   ```bash
   git checkout develop
   git push origin develop
   ```

2. **Create a Pull Request on GitHub**
   - Go to your repository
   - Click the **"Pull requests"** tab
   - Click **"New pull request"**
   - Set **base branch**: `main`
   - Set **compare branch**: `develop`
   - Click **"Create pull request"**

3. **Fill out the Pull Request**
   - **Title**: Descriptive title (e.g., "Release v1.2.0: New features and bug fixes")
   - **Description**: Use PR template (see below)
   - Add reviewers
   - Link related issues
   - Click **"Create pull request"**

4. **Review and Merge**
   - Reviewers review the changes
   - Run CI/CD checks if configured
   - Address review comments
   - Once approved, click **"Merge pull request"**
   - Choose merge type:
     - **Create a merge commit**: Preserves full history (recommended)
     - **Squash and merge**: Combines all commits into one
     - **Rebase and merge**: Linear history
   - Click **"Confirm merge"**

5. **Tag the Release** (if not using release branch)
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.2.0 -m "Release v1.2.0: [description]"
   git push origin v1.2.0
   ```

6. **Update develop** (sync with main)
   ```bash
   git checkout develop
   git pull origin develop
   git merge main  # Sync any changes from main
   git push origin develop
   ```

### Alternative: Using GitHub CLI

If you have GitHub CLI installed:

```bash
# Create PR
gh pr create --base main --head develop --title "Release v1.2.0" --body "Release notes here"

# Create and push tag
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

## Release Tags

### Creating Tags

#### Annotated Tags (Recommended)

Annotated tags store extra metadata and are recommended for releases:

```bash
# Create annotated tag
git tag -a v1.0.0 -m "Release v1.0.0: Initial release"

# Push tag to remote
git push origin v1.0.0

# Push all tags
git push origin --tags
```

#### Lightweight Tags

Lightweight tags are just pointers to commits:

```bash
git tag v1.0.0
git push origin v1.0.0
```

### Tagging Workflow

1. **After merging to main**
   ```bash
   git checkout main
   git pull origin main
   ```

2. **Create tag**
   ```bash
   git tag -a v1.0.0 -m "Release v1.0.0

   - Added feature X
   - Fixed bug Y
   - Updated documentation"
   ```

3. **Push tag**
   ```bash
   git push origin v1.0.0
   ```

### Viewing Tags

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"

# Show tag details
git show v1.0.0

# View tags on remote
git ls-remote --tags origin
```

### Tagging Hotfixes

Hotfixes should be tagged with patch version increment:

```bash
# Current version: v1.0.0
# Hotfix version: v1.0.1
git tag -a v1.0.1 -m "Hotfix v1.0.1: Fix critical security issue"
git push origin v1.0.1
```

### Updating/Deleting Tags

**⚠️ Be careful - only update tags before others have pulled them**

```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0

# Update existing tag (force push)
git tag -a -f v1.0.0 -m "Updated release message"
git push origin v1.0.0 --force
```

## Release Notes and Changelog

### CHANGELOG.md Format

Maintain a `CHANGELOG.md` file in the root directory:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- New feature X
- New feature Y

### Changed
- Improved performance of feature Z

### Fixed
- Fixed bug in authentication

## [1.0.0]

### Added
- Initial release
- User authentication
- Basic dashboard

### Changed
- Updated dependencies

### Fixed
- Fixed login redirect issue
```

### Updating Changelog

1. **Add entries under [Unreleased]** as you develop
2. **When releasing**, move entries to new version section
3. **Commit changelog** with release

### Release Notes in Pull Requests

Include release notes in PR description when merging to main:

```markdown
## Release v1.2.0

### Added
- New user dashboard
- Email notifications

### Changed
- Improved API response times
- Updated UI components

### Fixed
- Fixed memory leak in data processing
- Resolved authentication token expiration issue

### Breaking Changes
- API endpoint `/api/v1/users` now requires authentication
```

## Pre-Deployment Checklist

Before creating a Pull Request to `main`, verify:

### Code Quality
- [ ] All tests pass locally
- [ ] Code follows project style guide
- [ ] No console.logs or debug code left
- [ ] No commented-out code
- [ ] No TODO comments for this release

### Functionality
- [ ] All new features tested
- [ ] All bug fixes verified
- [ ] No known critical bugs
- [ ] Edge cases handled
- [ ] Error handling implemented

### Documentation
- [ ] README updated if needed
- [ ] API documentation updated
- [ ] CHANGELOG.md updated
- [ ] Code comments added where needed

### Configuration
- [ ] Version numbers updated
- [ ] Environment variables documented
- [ ] Dependencies updated and tested
- [ ] Build process works

### Security
- [ ] No sensitive data in code
- [ ] Dependencies scanned for vulnerabilities
- [ ] Authentication/authorization tested
- [ ] Input validation in place

### Deployment
- [ ] Database migrations tested (if applicable)
- [ ] Backward compatibility verified
- [ ] Rollback plan prepared
- [ ] Deployment steps documented

## Rollback Procedure

### Rolling Back a Release

If a release causes issues, rollback using one of these methods:

#### Method 1: Revert Merge Commit (Recommended)

1. **Identify the merge commit**
   ```bash
   git log --oneline main
   ```

2. **Revert the merge**
   ```bash
   git checkout main
   git pull origin main
   git revert -m 1 <merge-commit-hash>
   git push origin main
   ```

3. **Tag the rollback**
   ```bash
   git tag -a v1.0.1 -m "Rollback: Revert v1.1.0 due to critical bug"
   git push origin v1.0.1
   ```

4. **Update develop**
   ```bash
   git checkout develop
   git merge main
   git push origin develop
   ```

#### Method 2: Reset to Previous Tag

**⚠️ Use with caution - rewrites history**

1. **Reset to previous tag**
   ```bash
   git checkout main
   git reset --hard v1.0.0
   git push origin main --force
   ```

2. **Tag the rollback**
   ```bash
   git tag -a v1.0.1 -m "Rollback to v1.0.0"
   git push origin v1.0.1
   ```

#### Method 3: Create Hotfix

1. **Create hotfix branch from previous tag**
   ```bash
   git checkout v1.0.0
   git checkout -b hotfix/rollback-v1.1.0
   ```

2. **Fix the issues**
3. **Create PR and merge to main**
4. **Tag new version**

### Post-Rollback Steps

1. **Document the issue** in issue tracker
2. **Update CHANGELOG.md** with rollback entry
3. **Notify team** about the rollback
4. **Investigate root cause** of the issue
5. **Create fix** and test thoroughly before next release

## Hotfix (Urgent Production Fix)

If you need to fix something directly in production:

1. **Create hotfix branch from main**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/critical-bug-description
   ```

2. **Make the fix and commit**
   ```bash
   git add .
   git commit -m "Hotfix: Fix critical bug in [component]"
   git push origin hotfix/critical-bug-description
   ```

3. **Create Pull Request for hotfix**
   - Go to GitHub and create a PR from `hotfix/description` to `main`
   - Mark it as urgent/hotfix in the description
   - Get quick approval and merge

4. **Tag the hotfix release**
   ```bash
   git checkout main
   git pull origin main
   git tag -a v1.0.1 -m "Hotfix v1.0.1: Fix critical bug"
   git push origin v1.0.1
   ```

5. **Merge hotfix to develop**
   ```bash
   git checkout develop
   git pull origin develop
   git merge hotfix/critical-bug-description
   git push origin develop
   ```

6. **Delete hotfix branch**
   ```bash
   git branch -d hotfix/critical-bug-description
   git push origin --delete hotfix/critical-bug-description
   ```

## Pull Request Templates

### Creating PR Template

Create `.github/pull_request_template.md`:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Refactoring

## Testing
- [ ] Tests added/updated
- [ ] All tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] CHANGELOG.md updated (if applicable)

## Related Issues
Closes #123
Related to #456

## Screenshots (if applicable)
Add screenshots here

## Additional Notes
Any additional information
```

### PR Description Template

When creating PRs, use this structure:

```markdown
## Summary
Brief summary of changes

## Changes
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots
(if applicable)

## Related Issues
Closes #123
```

## Best Practices

1. **Always pull before starting work**
   ```bash
   git pull origin develop
   ```

2. **Write clear commit messages**
   - Use present tense: "Add feature" not "Added feature"
   - Be descriptive: "Add user authentication" not "Update code"
   - Follow conventional commits format (optional):
     - `feat: Add user authentication`
     - `fix: Resolve login redirect issue`
     - `docs: Update API documentation`
     - `refactor: Simplify data processing logic`

3. **Test before merging to main**
   - Run all tests
   - Test in staging environment if available
   - Verify deployment works

4. **Keep commits focused**
   - One logical change per commit
   - Separate commits for different types of changes
   - Don't mix feature work with bug fixes

5. **Regular Pull Requests**
   - Create PRs from develop to main regularly
   - Don't let develop get too far ahead of main
   - Always use Pull Requests, never merge directly to main

6. **Tag all releases**
   - Tag every merge to main
   - Use semantic versioning
   - Include release notes in tag message

7. **Keep branches in sync**
   - Regularly merge main into develop
   - Merge hotfixes to both main and develop
   - Delete merged branches

8. **Document changes**
   - Update CHANGELOG.md
   - Write clear PR descriptions
   - Document breaking changes

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

### Tag Issues

**Tag not showing on remote:**
```bash
git push origin --tags
```

**Wrong tag created:**
```bash
# Delete local tag
git tag -d v1.0.0
# Delete remote tag
git push origin --delete v1.0.0
# Create correct tag
git tag -a v1.0.0 -m "Correct message"
git push origin v1.0.0
```

## Setting Up Branch Protection (Repository Administrators)

To enforce the Pull Request workflow, set up branch protection rules for `main`:

1. **Go to Repository Settings**
   - Navigate to: `https://github.com/[org]/[repo]/settings`
   - Click **"Branches"** in the left sidebar

2. **Add Branch Protection Rule**
   - Click **"Add branch protection rule"**
   - Under **"Branch name pattern"**, enter: `main`
   - Enable the following options:
     - ✅ **Require a pull request before merging**
       - ✅ Require approvals (set number, e.g., 1)
       - ✅ Dismiss stale pull request approvals when new commits are pushed
       - ✅ Require review from Code Owners (if CODEOWNERS file exists)
     - ✅ **Require status checks to pass before merging**
       - Select required status checks (CI/CD pipelines)
     - ✅ **Require branches to be up to date before merging**
     - ✅ **Require conversation resolution before merging**
     - ✅ **Do not allow bypassing the above settings** (recommended)
     - ✅ **Restrict who can push to matching branches** (optional, for extra security)
     - ✅ **Allow force pushes** (usually disabled)
     - ✅ **Allow deletions** (usually disabled)

3. **Save the Rule**
   - Click **"Create"** or **"Save changes"**

This ensures that:
- No one can push directly to `main`
- All changes must go through Pull Requests
- Code must be reviewed before merging
- Required tests must pass
- The production branch stays stable

## Quick Reference

```bash
# Switch branches
git checkout develop
git checkout main

# Create and switch to new branch
git checkout -b feature/branch-name
git checkout -b release/v1.0.0
git checkout -b hotfix/description

# Update from remote
git pull origin [branch-name]

# Push changes
git push origin [branch-name]

# Create and push tag
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0

# List tags
git tag
git tag -l "v1.*"

# View tag details
git show v1.0.0

# Delete tag
git tag -d v1.0.0
git push origin --delete v1.0.0

# View branch status
git branch -a
git status
git log --oneline --graph --all

# Create Pull Request (use GitHub web interface or CLI)
gh pr create --base main --head develop --title "Title" --body "Description"

# Never merge directly to main - always use Pull Requests
```

## Additional Resources

- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

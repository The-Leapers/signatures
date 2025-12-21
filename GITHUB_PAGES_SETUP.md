# GitHub Pages Setup Guide

## Steps to Enable GitHub Pages

### 1. Navigate to Repository Settings
- Go to your GitHub repository: https://github.com/The-Leapers/signatures
- Click on the **Settings** tab (top navigation bar)

### 2. Enable GitHub Pages
- In the left sidebar, scroll down and click on **Pages**
- Under **Source**, select:
  - **Branch**: `main`
  - **Folder**: `/ (root)`
- Click **Save**

### 3. Access Your Site
After enabling, your site will be available at:
- **URL**: `https://the-leapers.github.io/signatures/`

Note: It may take a few minutes for the site to be available after enabling.

### 4. Custom Domain (Optional)
If you want to use a custom domain:
- Add a `CNAME` file in the root directory with your domain name
- Configure DNS settings as instructed by GitHub

## Repository Structure for GitHub Pages

Your repository is already set up correctly:
- ✅ `index.html` in the root directory (serves as the landing page)
- ✅ `active/` folder with signature files
- ✅ Shared assets (logos, banners) in root directory
- ✅ Image paths use relative paths that work with GitHub Pages

## Testing Locally

You can test the site locally before pushing:
```bash
# Using Python 3
python3 -m http.server 8000

# Or using Node.js (if you have http-server installed)
npx http-server
```

Then visit: http://localhost:8000

## Updating the Site

After making changes:
1. Commit your changes
2. Push to the `main` branch
3. GitHub Pages will automatically rebuild (usually within 1-2 minutes)

## Troubleshooting

- **Site not loading**: Check the Actions tab to see if the build succeeded
- **Images not showing**: Verify image paths are relative (using `../` for parent directory)
- **404 errors**: Ensure `index.html` is in the root directory


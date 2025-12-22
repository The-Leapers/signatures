# Leapers Email Signatures

This repository contains HTML email signature templates for The Leapers team members. The signatures are hosted on [GitHub Pages](https://the-leapers.github.io/signatures/) for easy access and sharing.

## Structure

- `active/` - Contains active signature files and their profile images
- `archive/` - Contains inactive/archived signature files and their images
- `template.html` - Template file with instructions for creating new signatures
- `index.html` - Landing page with links to all active signatures (served by GitHub Pages)
- Root directory - Contains shared assets (company logos, banners)

Each team member has their own HTML signature file (e.g., `ayca.html`, `burcak.html`, etc.) along with their profile image (PNG format). Active signatures are located in the `active/` folder.

## Contact Information

### London Office
22 Highbury Grove, London, United Kingdom, N5 2ER
Phone: +44 (20) 8720 9257

### İstanbul Office
Medeniyet Üniversitesi Teknopark. Akfırat Mahallesi Fatih Sultan Mehmet Bulvarı No 3 Daire 53 Tuzla 34959
Phone: +90 (850) 303 02 18

## Active Signatures

The following team members have active signatures:
- Ayça Ceren Akdemir
- Burçak Yıldırım Orhan
- Deniz İpek Bardan
- Semih Selçuklu
- Arda Yiğithan Orhan

View all active signatures:
- **GitHub Pages**: [https://the-leapers.github.io/signatures/](https://the-leapers.github.io/signatures/)
- **Local**: Open `index.html` in your browser
- **Direct**: Navigate to files in the `active/` folder

## Creating a New Signature

**⚠️ IMPORTANT: Always work on the `develop` branch for all development work.**

1. **Switch to develop branch**
   ```bash
   git checkout develop
   git pull origin develop
   ```

2. Copy `template.html` to create a new signature file
3. Rename the file to the person's first name (e.g., `john.html`)
4. Place the file in the `active/` folder
5. Add the person's profile image (PNG format, 120px wide) to the `active/` folder with the same name (e.g., `john.png`)
6. Update the template with:
   - Profile image filename in the `src` attribute
   - Person's full name (in both the image `alt` text and the name field)
   - Person's job title/position
7. Update `index.html` to add a link to the new signature
8. Commit and push to `develop` branch
9. After testing, merge `develop` to `main` to deploy to GitHub Pages

See `template.html` for detailed inline instructions and TODO comments.
See `GIT_FLOW_PROJECT.md` for project-specific workflow instructions.
See `GIT_FLOW.md` for general Git Flow concepts and advanced topics.

## Usage

Each HTML file can be copied and used directly in email clients that support HTML signatures. The signature files reference shared assets (logos and banners) from the root directory, so ensure the folder structure is maintained when deploying.

## GitHub Pages

This repository is configured to be hosted on GitHub Pages. The site is automatically built from the `main` branch whenever changes are pushed.

- **Live Site**: [https://the-leapers.github.io/signatures/](https://the-leapers.github.io/signatures/)
- **Setup Instructions**: See `GITHUB_PAGES_SETUP.md` for detailed setup and troubleshooting

## Git Workflow

**⚠️ IMPORTANT: All development work must be done on the `develop` branch. Never commit directly to `main`.**

This repository uses Git Flow with Pull Requests for managing changes:

- `main`: Production branch - contains stable, production-ready signatures (deployed to GitHub Pages)
  - **Only receives code via Pull Requests from `develop`**
  - **Never commit directly to this branch**
  - **Protected branch - requires Pull Request for all changes**
- `develop`: Development branch - used for all new signatures, updates, and testing
  - **This is where all development work happens**
  - **Always start here when creating or updating signatures**
  - **Changes are merged to `main` via Pull Requests**

**Workflow:**
1. Make changes on `develop` branch
2. Test and commit changes
3. Create a Pull Request from `develop` to `main`
4. Review and merge the Pull Request
5. GitHub Pages automatically deploys from `main`

See `GIT_FLOW_PROJECT.md` for project-specific workflow instructions.
See `GIT_FLOW.md` for general Git Flow concepts and advanced topics.

## Branches

- `main`: Production-ready signatures (deployed to GitHub Pages)
- `develop`: Development branch for testing and updates


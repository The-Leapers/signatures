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

1. Copy `template.html` to create a new signature file
2. Rename the file to the person's first name (e.g., `john.html`)
3. Place the file in the `active/` folder
4. Add the person's profile image (PNG format, 120px wide) to the `active/` folder with the same name (e.g., `john.png`)
5. Update the template with:
   - Profile image filename in the `src` attribute
   - Person's full name (in both the image `alt` text and the name field)
   - Person's job title/position
6. Update `index.html` to add a link to the new signature

See `template.html` for detailed inline instructions and TODO comments.

## Usage

Each HTML file can be copied and used directly in email clients that support HTML signatures. The signature files reference shared assets (logos and banners) from the root directory, so ensure the folder structure is maintained when deploying.

## GitHub Pages

This repository is configured to be hosted on GitHub Pages. The site is automatically built from the `main` branch whenever changes are pushed.

- **Live Site**: [https://the-leapers.github.io/signatures/](https://the-leapers.github.io/signatures/)
- **Setup Instructions**: See `GITHUB_PAGES_SETUP.md` for detailed setup and troubleshooting

## Git Workflow

This repository uses Git Flow for managing changes:

- `main`: Production branch - contains stable, production-ready signatures (deployed to GitHub Pages)
- `develop`: Development branch - used for testing and preparing new signatures

See `GIT_FLOW.md` for detailed workflow instructions.

## Branches

- `main`: Production-ready signatures (deployed to GitHub Pages)
- `develop`: Development branch for testing and updates


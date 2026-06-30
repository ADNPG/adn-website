# ADN Website

Public website project for ADN Property Group.

This repository is intended for:

- static website pages
- shared frontend assets
- Cloudflare Pages deployment
- GitHub-backed version control

Current structure:

- `index.html` will be the primary homepage when created
- `sell-your-house\` now contains the imported ADN seller landing page bundle
- `about\`, `contact\`, `privacy\`, and `terms\` are reserved for broader site pages
- `assets\` holds shared CSS, JavaScript, images, and icons

Recommended workflow:

1. Keep public website code here.
2. Keep MLCS backend/intake logic outside this repo.
3. Connect this folder to GitHub.
4. Connect GitHub to Cloudflare Pages for deployment.

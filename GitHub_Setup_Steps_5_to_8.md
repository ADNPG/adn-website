# GitHub Setup Steps 5 Through 8

Project folder:

`C:\Projects\ADN-Website`

These steps assume the ADN website files already exist in this folder and you want to connect that local project to a new GitHub repository.

## 5. Initialize Git Locally

Open PowerShell and run:

```powershell
cd C:\Projects\ADN-Website
git init
git branch -M main
```

What this does:

- `git init` creates a local Git repository in the project folder.
- `git branch -M main` makes sure the default branch is named `main`.

## 6. Make The First Commit

Set your Git identity first:

```powershell
git config user.name "Your Name"
git config user.email "you@example.com"
```

Then stage and commit everything:

```powershell
git add .
git commit -m "Initial ADN website structure and seller landing page import"
```

Useful check:

```powershell
git status
```

Expected result:

- the working tree should be clean after the commit

## 7. Connect The Local Repo To GitHub

First create an empty repository on GitHub:

1. Sign in to GitHub.
2. Click `New repository`.
3. Name it something like `adn-website`.
4. Keep it empty.
5. Do not add a README, `.gitignore`, or license from GitHub, because this project already has local files.

Then copy the GitHub repository URL and run:

```powershell
git remote add origin https://github.com/<your-username>/adn-website.git
```

Verify it:

```powershell
git remote -v
```

Expected result:

- `origin` should appear for both fetch and push

## 8. Push To GitHub

Run:

```powershell
git push -u origin main
```

What this does:

- uploads your local `main` branch to GitHub
- sets `origin/main` as the upstream branch

After that, future pushes can usually be:

```powershell
git push
```

## Common Problems

### Author identity unknown

Fix:

```powershell
git config user.name "Your Name"
git config user.email "you@example.com"
```

### Remote origin already exists

Check the current remote:

```powershell
git remote -v
```

Replace it if needed:

```powershell
git remote remove origin
git remote add origin https://github.com/<your-username>/adn-website.git
```

### GitHub authentication prompt during push

GitHub may ask you to authenticate in a browser or use a personal access token, depending on your Git setup.

## End State

After step 8:

- the local repo exists in `C:\Projects\ADN-Website`
- the GitHub repo contains the same files
- the project is ready to be connected to Cloudflare Pages

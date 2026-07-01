# ADN Website Cloudflare + GitHub Workflow

Last updated: 2026-06-30

## Goal

Use one GitHub-backed Cloudflare Pages project for the main ADN site, while keeping the seller experience strongly branded and easy to market.

Chosen structure:

- main company domain: `adnpropertygroup.com`
- main site Pages project: this repo, `ADNPG/adn-website`
- seller landing page content path: `https://adnpropertygroup.com/sell-your-house/`
- seller-facing entry hostname: `https://sell.adnpropertygroup.com`
- secondary seller-marketing domain: `https://adnpropertysolutions.com`

Important constraint:

- under this Option B setup, the canonical seller content still lives on the main domain path `/sell-your-house/`
- `sell.adnpropertygroup.com` and `adnpropertysolutions.com` are redirect entrypoints, not separate live content origins
- if you later want the seller page to truly live at `sell.adnpropertygroup.com` without redirecting to the main domain path, that is a different deployment shape and should be treated as a follow-up change

## Current local repo state

- GitHub repo already exists: `https://github.com/ADNPG/adn-website`
- local repo path: `C:\Projects\ADN-Website`
- seller page bundle currently lives at:
  - `C:\Projects\ADN-Website\sell-your-house\index.html`
  - `C:\Projects\ADN-Website\sell-your-house\site-config.js`

## Step 1: Add Both Domains To Cloudflare

You want both domains present in the same Cloudflare account:

- `adnpropertygroup.com`
- `adnpropertysolutions.com`

For each domain:

1. Log in to Cloudflare.
2. Select `Add a domain`.
3. Enter the domain name.
4. Choose the plan you want.
5. Let Cloudflare scan existing DNS records.
6. Review imported records carefully before continuing.
7. Update the domain at your registrar so the nameservers point to Cloudflare.
8. Wait for the zone to become active.

What matters here:

- the apex domain `adnpropertygroup.com` must be on Cloudflare nameservers for Pages apex-domain attachment
- `adnpropertysolutions.com` should also be on Cloudflare so redirects are easy to manage in the same account

Checkpoint:

- both zones show as active in Cloudflare

## Step 2: Connect The GitHub Repo To A Cloudflare Pages Project

This repo should back one Pages project for the main website.

In Cloudflare:

1. Go to `Workers & Pages`.
2. Select `Create application`.
3. Select `Pages`.
4. Select `Connect to Git`.
5. Authorize GitHub if prompted.
6. Choose the repository:
   - `ADNPG/adn-website`
7. Select branch:
   - `main`

Recommended Pages build settings for the current repo:

- Framework preset: `None`
- Build command: leave blank
- Build output directory: leave blank if Cloudflare accepts it, otherwise use `.`
- Root directory: leave blank

Reason:

- this repo is currently plain static HTML, not a framework build

Checkpoint:

- Cloudflare creates the Pages project
- the first deployment completes successfully

## Step 3: Attach The Main Domain To The Pages Project

Under Option B, the Pages project should serve the main company domain.

In the Pages project:

1. Open the project.
2. Go to `Custom domains`.
3. Select `Set up a domain`.
4. Add:
   - `adnpropertygroup.com`
5. Add:
   - `www.adnpropertygroup.com`

Expected result:

- Cloudflare will manage the required DNS records for the Pages project
- `www` can later be redirected to the apex

Important repo note:

- your seller page currently lives at `/sell-your-house/`
- the repo root does not yet have a public `index.html` homepage file
- that means the Pages project can be attached now, but the main root experience still needs a homepage before `adnpropertygroup.com` is production-ready as a full site

Practical interpretation:

- attaching the domain is fine now
- public launch of the main domain should wait until root homepage content exists

Checkpoint:

- `adnpropertygroup.com` and `www.adnpropertygroup.com` appear as verified custom domains on the Pages project

## Step 4: Configure Redirects For The Seller-Branded Entry Points

This is the part that gives you the strongest seller-brand presentation while still keeping Option B's single-project structure.

Target canonical seller URL:

- `https://adnpropertygroup.com/sell-your-house/`

Create these redirects:

1. `www.adnpropertygroup.com/*`
   - redirect to `https://adnpropertygroup.com/$1`
   - status code `301`

2. `sell.adnpropertygroup.com/*`
   - redirect to `https://adnpropertygroup.com/sell-your-house/`
   - status code `301`

3. `adnpropertysolutions.com/*`
   - redirect to `https://adnpropertygroup.com/sell-your-house/`
   - status code `301`

4. `www.adnpropertysolutions.com/*`
   - redirect to `https://adnpropertygroup.com/sell-your-house/`
   - status code `301`

Recommended implementation method:

- use Cloudflare `Single Redirects`

Why:

- you only need a small number of redirects
- these are zone-level redirects
- wildcard matching is supported

Where to configure:

1. Open the relevant zone in Cloudflare.
2. Go to `Rules`.
3. Go to `Redirect Rules` or `Single Redirects`.
4. Add the rules above.

Important Cloudflare requirement:

- redirect rules require proxied DNS records through Cloudflare

Checkpoint:

- entering `sell.adnpropertygroup.com` in a browser lands on the seller page path
- entering `adnpropertysolutions.com` in a browser lands on the seller page path
- entering `www.adnpropertygroup.com` lands on the apex main site domain

## Recommended DNS / routing shape summary

- live Pages site origin:
  - `adnpropertygroup.com`
- main root homepage:
  - future repo-root `index.html`
- seller page content:
  - `/sell-your-house/`
- seller-brand entry URLs:
  - `sell.adnpropertygroup.com`
  - `adnpropertysolutions.com`
- both seller-brand entry URLs redirect to:
  - `https://adnpropertygroup.com/sell-your-house/`

## What You Should Do Next After Steps 1 To 4

1. Create a repo-root homepage for the main ADN site:
   - `C:\Projects\ADN-Website\index.html`
2. Keep refining the seller page in:
   - `C:\Projects\ADN-Website\sell-your-house\index.html`
3. When the production intake endpoint is ready, update:
   - `C:\Projects\ADN-Website\sell-your-house\site-config.js`
4. Push changes to GitHub and let Cloudflare Pages auto-deploy.

## If You Later Want Even Stronger Seller Branding

If you later decide the seller page should truly live at:

- `https://sell.adnpropertygroup.com`

without redirecting to the main-domain path, move to a two-project setup:

- main Pages project for `adnpropertygroup.com`
- seller Pages project for `sell.adnpropertygroup.com`
- keep `adnpropertysolutions.com` redirecting to the seller Pages project

That is not this runbook. This runbook stays within Option B.

## Official references

- Cloudflare Pages custom domains:
  - https://developers.cloudflare.com/pages/configuration/custom-domains/
- Cloudflare Redirects:
  - https://developers.cloudflare.com/rules/url-forwarding/

# Host And Domain Setup

Target public URL: `https://sell.adnpropertygroup.com`

Status: domain target selected, host not yet chosen, production intake endpoint not yet inserted

## Recommended Path

Recommended first release path:

1. use a static host for the page
2. attach subdomain `sell.adnpropertygroup.com`
3. keep MLCS intake separate behind the form endpoint
4. set the production/public Apps Script intake URL in `site-config.js` immediately before launch

Recommended host:

- Cloudflare Pages

Why:

- strong fit for a single static HTML page
- straightforward custom-subdomain setup
- free SSL on the custom hostname
- clean separation between static page hosting and Apps Script intake backend

## Bundle Files To Publish

Use this folder as the host upload bundle:

- `index.html`
- `site-config.js`
- `DEPLOYMENT_README.md`
- `RELEASE_MANIFEST.md`

Before public launch:

- set `formEndpoint` in `site-config.js` to the intended production/public Apps Script web-app URL ending in `?route=seller-intake`

## Cloudflare Pages First-Pass Steps

1. Create a new Pages project for the ADN seller site.
2. Use direct upload or a repo-backed Pages project for this bundle.
3. Publish the current static bundle.
4. Add custom domain `sell.adnpropertygroup.com` to the Pages project.
5. In the DNS provider for `adnpropertygroup.com`, create the DNS record required by the host.
6. Wait for DNS and SSL issuance to complete.
7. Update `site-config.js` with the production/public Apps Script intake URL.
8. Upload the final bundle again or redeploy the Pages project.
9. Run one live public smoke test on the final domain.

## If You Do Not Use Cloudflare Pages

Equivalent shape on Netlify or Vercel:

- create a static site project
- add custom domain `sell.adnpropertygroup.com`
- create the required DNS record at the domain DNS provider
- set `site-config.js` with the live Apps Script intake URL
- redeploy and smoke test

## Launch Gate

Do not point traffic to `sell.adnpropertygroup.com` until all of the following are true:

- host is serving the static bundle on the final domain
- SSL is valid
- `site-config.js` contains the intended production/public intake URL
- the live Apps Script endpoint is enabled and verified for `POST`
- one live-domain smoke test lead has been submitted and confirmed through the reviewed queue

# Cloudflare Pages Launch Checklist

Target public URL: `https://sell.adnpropertygroup.com`

Host assumption: Cloudflare Pages

Backend assumption: production/public Google Apps Script web-app intake URL for `?route=seller-intake`

## Launch Goal

Publish the ADN seller landing page on `sell.adnpropertygroup.com` and wire the form to the intended live Apps Script intake endpoint.

## Files To Use

Working release bundle:

- `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\index.html`
- `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\site-config.js`
- `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\DEPLOYMENT_README.md`
- `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\HOST_DOMAIN_SETUP.md`

Optional upload bundle:

- `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1.zip`

## Step 1. Confirm The Live Apps Script Intake URL

Before touching Cloudflare Pages, confirm the intended live production/public Apps Script URL.

It should look like:

```text
https://script.google.com/macros/s/<DEPLOYMENT_ID>/exec?route=seller-intake
```

Requirements:

- it must be the intended live public web-app deployment
- the seller-intake route must be enabled
- the deployment must be the one you actually want public traffic to hit

Important:

- do not reuse the current non-production URL for launch
- do not rely on a browser `GET` to this URL as the health check
- seller-intake is a `POST` path in current source templates

## Step 2. Edit `site-config.js`

Open:

`C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\site-config.js`

Current file:

```js
window.ADN_SELLER_SITE_CONFIG = {
  formEndpoint: ""
};
```

Replace it with the real production/public Apps Script URL once you have it:

```js
window.ADN_SELLER_SITE_CONFIG = {
  formEndpoint: "https://script.google.com/macros/s/REPLACE_WITH_PRODUCTION_DEPLOYMENT_ID/exec?route=seller-intake"
};
```

Example only:

```js
window.ADN_SELLER_SITE_CONFIG = {
  formEndpoint: "https://script.google.com/macros/s/AKfycbxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/exec?route=seller-intake"
};
```

Check:

- keep the quotes
- keep the trailing semicolon
- do not add extra keys unless you intentionally extend the page contract

## Step 3. Rebuild The ZIP Bundle

If you are deploying from the ZIP artifact, rebuild it after editing `site-config.js`.

PowerShell:

```powershell
Compress-Archive -Path "C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1\*" `
  -DestinationPath "C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1.zip" `
  -Force
```

If you are uploading the folder contents manually in Cloudflare Pages, this step is optional.

## Step 4. Create The Cloudflare Pages Project

In Cloudflare:

1. Open `Workers & Pages`.
2. Create a new `Pages` project.
3. Choose the simplest static deployment path available to you:
   - direct upload of the folder contents
   - or repo-backed Pages project if you want ongoing versioned deploys

Recommended first pass:

- direct upload

Upload these files:

- `index.html`
- `site-config.js`

You may also keep the README/manifest files locally; they do not need to be publicly hosted.

## Step 5. Verify The Preview Deployment

Before adding the custom domain, test the Pages preview URL.

Check:

1. Page loads with the expected ADN styling.
2. Hero and form render correctly.
3. Mobile header still collapses cleanly.
4. Browser console is clean enough for launch.
5. Submit button changes to `Submitting...` when used.
6. `site-config.js` is actually being read by the page.

Quick browser check:

- open the preview deployment
- inspect the form element
- confirm the form `action` is the exact production/public Apps Script URL from `site-config.js`

## Step 6. Add The Custom Domain

In the Cloudflare Pages project:

1. Open project settings.
2. Add custom domain:

```text
sell.adnpropertygroup.com
```

3. Follow the Pages prompt for the required DNS record.

If the DNS for `adnpropertygroup.com` is already managed inside Cloudflare:

- Cloudflare will usually create or suggest the record for you

If DNS is managed elsewhere:

- copy the exact DNS target/value Cloudflare gives you
- create the requested DNS record at the current DNS provider

## Step 7. Wait For DNS And SSL

Do not send traffic yet.

Wait until:

- `sell.adnpropertygroup.com` resolves
- Cloudflare Pages shows the custom domain as active
- SSL is issued and valid

Public check:

```text
https://sell.adnpropertygroup.com
```

## Step 8. Run The Final Live Smoke Test

Once the custom domain is live:

1. Open `https://sell.adnpropertygroup.com`
2. Verify desktop layout
3. Verify mobile layout
4. Confirm the form action still points to the production/public Apps Script URL
5. Submit one clearly marked live smoke-test lead
6. Confirm the row lands in the reviewed public seller queue
7. Decide whether to keep or remove that test row through the approved queue path

Suggested smoke-test payload:

- seller name: `Launch Smoke Test`
- address: `TEST sell.adnpropertygroup.com launch verification`
- notes: `LIVE PUBLIC DOMAIN SMOKE TEST. Remove after verification.`

## Step 9. Launch Decision

Launch only when all are true:

- `sell.adnpropertygroup.com` serves the page over valid SSL
- `site-config.js` points at the intended production/public Apps Script intake URL
- one live smoke-test submission lands in the reviewed queue
- you are satisfied with desktop and mobile rendering

## Quick Rollback

If something is wrong after publish:

1. Remove or blank `formEndpoint` in `site-config.js`
2. Redeploy the Pages bundle
3. The page will still render, but submissions will stop

Emergency blank config:

```js
window.ADN_SELLER_SITE_CONFIG = {
  formEndpoint: ""
};
```

## Final Operator Notes

- keep the static host and the Apps Script intake endpoint as separate deployment concerns
- change `site-config.js` only when you intentionally want to change where leads are posted
- do not use raw browser GET checks to validate seller-intake health; use real POST submission plus queue confirmation

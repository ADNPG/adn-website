# ADN Property Group Seller Site V1

Location: `C:\Projects\MLCS\docs\Marketing\GoLive\ADN_HomeSolutions_Seller_Site_V1`

Status: non-production-verified static publishing bundle targeting `sell.adnpropertygroup.com`

## What This Bundle Is

This folder is the current public-site release bundle for the ADN seller landing page.

Contents:

- `index.html` - standalone static landing page
- `site-config.js` - endpoint configuration file for the live Apps Script intake URL
- `HOST_DOMAIN_SETUP.md` - host/domain setup note for `sell.adnpropertygroup.com`
- `CLOUDFLARE_PAGES_LAUNCH_CHECKLIST.md` - literal launch checklist for the recommended host path

This bundle is designed for a simple static host such as:

- an ADN-controlled website root or subfolder
- a static site bucket
- a landing-page host
- any host that can serve plain HTML directly

## Verified Current State

- page copy localized to Raleigh / Wake County
- desktop browser render checked
- narrow mobile browser render checked
- no unresolved `{{...}}` placeholder tokens remain
- standalone logo dependency replaced with embedded SVG brand mark
- non-production intake POST path verified separately against the current MLCS seller-intake route

## Current Endpoint Wiring

The page form no longer hardcodes the intake URL in `index.html`.

Instead, the live form target is read from:

`site-config.js`

Important:

- keep `site-config.js` empty until you are ready to insert the intended production/public Apps Script intake URL
- local source templates show `route=seller-intake` is handled on `doPost(e)`
- a plain browser `GET` to that URL is not the correct health check
- one controlled non-production POST submission was proved against the non-production endpoint, then cleaned up from the review queue

## Recommended Publish Shape

1. Upload this bundle to the intended ADN public host.
2. Serve it on `https://sell.adnpropertygroup.com`.
3. Keep MLCS behind the intake boundary and do not expose operator/dashboard language on the page.
4. Before public traffic, set `site-config.js` to the intended live intake endpoint and confirm that endpoint is the correct target for that host environment.

## Remaining Release Decisions

- confirm whether the current footer should remain service-area-only or later include a mailing/business address
- confirm the final production/public Apps Script intake URL before traffic is sent

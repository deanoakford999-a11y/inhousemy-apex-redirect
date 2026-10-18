# inhousemy-apex-redirect (stopgap)

Sends every address on the bare domain `inhousemy.com` to the same page on
`https://www.inhousemy.com`, keeping the path, query string (UTM tags) and `#hash`.

Why: GoDaddy domain forwarding only redirects the bare homepage, so links such as
`inhousemy.com/events/` returned 404. The proper fix (a server-side 301 via the
Netlify site `inhousemy-apex-redirect`) is blocked pending Netlify support.

How it works: GitHub Pages serves `404.html` for every unknown path and `index.html`
for `/`. Both run `location.replace(...)` in the `<head>`, with a meta-refresh to the
homepage and a `<noscript>` link as fallbacks. Pages are `noindex` with a canonical to
www. No external assets, no tracking. This is a browser-side redirect, not an HTTP 301.

DNS (GoDaddy): four `@` A records -> 185.199.108.153 / .109.153 / .110.153 / .111.153.
`www`, MX and TXT records are untouched. Steps: `/workspace/inhouse/dns/apex-github-pages-stopgap.md`.

## Retire it
1. Put the proper 301 redirect in place (Netlify or registrar), or roll back:
   in GoDaddy delete the four `@` A records above and re-enable domain forwarding to
   `https://www.inhousemy.com`.
2. Then, and only then, remove the custom domain under Settings > Pages and
   disable Pages / archive this repo. (Removing the domain while DNS still points at
   GitHub leaves it open to takeover.)

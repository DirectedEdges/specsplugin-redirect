# specsplugin.com → spectacular.systems

A redirect, and nothing else. This repository exists only so GitHub Pages will serve `specsplugin.com` with a certificate.

## Why it exists

The plugin was renamed from Specs 2 to **Spectacular** in 2026, when Figma shipped a feature of its own called Specs. The documentation site moved from `specsplugin.com` to [spectacular.systems](https://spectacular.systems), and the old domain needs to keep working — every link inside the shipped CLI, the Figma plugin, and the published npm packages points at it, and those are `https://`.

Two things ruled out the obvious approaches:

- **GitHub Pages serves one custom domain per repository.** `DirectedEdges/specs` serves `spectacular.systems`, so it cannot also serve the old domain. Hence a second repository.
- **DNSimple's `URL` record does redirect, with the path preserved, but HTTPS redirects are gated behind a paid plan tier.** HTTP worked; HTTPS presented no certificate at all. Since every link in the wild is `https://`, that was no use.

GitHub Pages issues a certificate for a custom domain at no cost, which is the whole reason this exists.

## How it works

`404.html` is the redirect. GitHub Pages serves it for every path it cannot find — which here is every path — so it is what actually runs, and it carries the path across to the new domain. `index.html` is the same file with a canonical link instead of `noindex`, and only ever serves the bare domain.

The redirect fires three ways, strongest first: a script that runs immediately and preserves the path; a zero-delay meta refresh for visitors without JavaScript, which lands on the homepage because it cannot see the path; and a visible link for anything honouring neither.

This is a client-side redirect, not a `301`. A server-side redirect would be better for search engines, but GitHub Pages does not offer one. A zero-delay meta refresh is treated close to a permanent redirect, which is the best available here.

## DNS

Set at DNSimple, which remains the registrar and DNS host for the domain:

| Type | Name | Content |
|---|---|---|
| A | `specsplugin.com` | `185.199.108.153`, `.109.153`, `.110.153`, `.111.153` |
| CNAME | `www.specsplugin.com` | `directededges.github.io` |

The custom domain is the apex, so GitHub redirects `www` to it.

Untouched, and deliberately so: the `MX` record carrying Google Workspace mail for the domain, the `google-site-verification` TXT, and the `api` CNAME.

## Changing it

Nothing here should need to change. If the destination ever moves again, it is the one hostname in `404.html`, `index.html`, and the link text.

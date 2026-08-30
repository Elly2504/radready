# RadReady legal pages and derived datasets

**Published and live: https://elly2504.github.io/radready/**

Two jobs:

1. The Terms of Use and Privacy Policy the app's paywall links to. App Store
   Guideline 3.1.2 requires both to resolve.
2. `data/` — the OpenStreetMap-derived dataset bundled inside the app, published
   under ODbL as §4.6 requires. See `data/README.md` for the derivation method.

The root URL is also the contact address in the app's User-Agent, which OSM
Overpass, NOAA/NWS and MET Norway all require to resolve.

Note: the repo-root `.gitignore` rule for the NicheFinder cache must stay
anchored as `/data/`. Unanchored, it silently swallows `site/data/` and the ODbL
offer never gets committed.

## Publishing

These are served from GitHub Pages at:

- https://elly2504.github.io/radready/privacy/
- https://elly2504.github.io/radready/terms/

Already set up. To republish after a change, from inside `site/`:

```bash
git add -A && git commit -m "Update" && git push
```

The original one-time setup was:

```bash
cd site
git init -b main
git remote add origin git@github.com:elly2504/radready.git
git add -A && git commit -m "RadReady legal pages"
git push -u origin main
gh repo edit elly2504/radready --visibility public
gh api -X POST repos/elly2504/radready/pages -f 'source[branch]=main' -f 'source[path]=/'
```

The repo must be public for Pages to serve on a free account. It contains only
these three pages — no app source.

If you later register a domain, point it at this Pages site and update
`AppConstants.privacyPolicyURL` / `termsOfUseURL` in
`RadReady/Core/Utilities/Constants.swift`; nothing else references the URLs.

# RadReady legal pages

Static pages backing the Terms of Use and Privacy Policy links in the app's
paywall. App Store Guideline 3.1.2 requires both to resolve.

## Publishing

These are served from GitHub Pages at:

- https://elly2504.github.io/radready/privacy/
- https://elly2504.github.io/radready/terms/

To publish, push the *contents of this directory* to the root of a public repo
named `radready`, then turn on Pages:

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

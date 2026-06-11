# apps.dvoryashin.com

Static site that serves **privacy policy** and **support** pages for the App Factory iOS apps,
hosted on **GitHub Pages** at `https://apps.dvoryashin.com/`.

This is a **public** repo. It contains only the public-facing HTML — no app-factory strategy,
ADRs, or business data (those stay in the private `app-factory` repo).

## Structure

```
CNAME                                 → apps.dvoryashin.com (custom domain)
.nojekyll                             → serve files as-is (no Jekyll)
index.html                            → landing page
dumpling-collector/
  privacy/index.html                  → https://apps.dvoryashin.com/dumpling-collector/privacy
  support/index.html                  → https://apps.dvoryashin.com/dumpling-collector/support
```

Directory-style paths (`privacy/index.html`) make the clean URLs work without a `.html`
suffix — that's what the App Store listing uses.

## Source of truth

The page **sources** live in the private app-factory repo at
`apps/<slug>/store/{privacy,support}.html`. When they change, copy them here:

```sh
STORE=…/app_factory/apps/dumpling-collector/store
cp "$STORE/privacy.html" dumpling-collector/privacy/index.html
cp "$STORE/support.html" dumpling-collector/support/index.html
git commit -am "update dumpling-collector pages" && git push
```

## One-time deploy

1. **Create the public repo and push** (GitHub CLI):
   ```sh
   gh repo create apps.dvoryashin.com --public --source=. --remote=origin --push
   ```
   (or create an empty public repo on github.com and `git remote add origin … && git push -u origin main`)

2. **Enable Pages:** repo → Settings → Pages → Source = `Deploy from a branch`,
   Branch = `main` / `/ (root)`. The `CNAME` file sets the custom domain automatically.

3. **Add the DNS record** at the `dvoryashin.com` registrar:
   ```
   Type: CNAME   Name: apps   Value: bigevilbanana.github.io
   ```

4. Wait for DNS to propagate, then in Pages enable **Enforce HTTPS** (cert is issued automatically).

5. Verify:
   - https://apps.dvoryashin.com/dumpling-collector/privacy
   - https://apps.dvoryashin.com/dumpling-collector/support

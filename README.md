# Tesla bot site template

A tiny, static GitHub Pages site for anyone setting up a personal **Tesla Fleet API** bot. It does two jobs:

1. **Hosts your public key** at `/.well-known/appspecific/com.tesla.3p.public-key.pem`, which Tesla fetches to verify your app.
2. **Provides the OAuth redirect page** at `/callback`, which shows the callback link so you can paste it back into your bot.

No build step, no Jekyll, no scripts from anywhere else, no trackers.

## What's inside

| Path | Purpose |
| --- | --- |
| `.nojekyll` | Tells GitHub Pages to serve files as-is (otherwise it skips `.well-known`). |
| `index.html` | Friendly landing page that names the owner and links to the key. |
| `callback.html` | The OAuth redirect page. It reads `code`, `state` and `error` in your browser only. |
| `.well-known/appspecific/README.txt` | Marks where your bot drops `com.tesla.3p.public-key.pem`. |

## How to use it

1. On this repo's GitHub page, click **Use this template → Create a new repository**.
2. Choose your new organisation as the **Owner** (see naming below) and name the repo **`<org>.github.io`**. Keep it public.
3. Let your bot fill in the placeholders and add your public key, or do it by hand (see below).
4. In the repo, go to **Settings → Pages** and set the source to **Deploy from a branch**, `main`, `/ (root)`.
5. After a minute, check these load:
   - `https://<org>.github.io/`
   - `https://<org>.github.io/.well-known/appspecific/com.tesla.3p.public-key.pem`
   - `https://<org>.github.io/callback`

### Naming

- **Organisation:** `<github-username>-teslabot` (for example `janedoe-teslabot`). Creating an organisation is free: **github.com → + → New organization → Free**.
- **Repository:** `<org>.github.io` (for example `janedoe-teslabot.github.io`).
- **Your site:** `https://<org>.github.io`

### Why an organisation?

- A personal account gets **only one** user site (`<username>.github.io`). You may already use it, or want to keep it for something else.
- **Project sites** (`<username>.github.io/<repo>`) live under a sub-path, so they **can't serve `/.well-known/` at the domain root**, which is where Tesla looks for your key.
- A free organisation gets its own user-style site at the root of `<org>.github.io`, which solves both problems.

## Registering with Tesla

In the Tesla developer portal (developer.tesla.com), when you create or edit your app:

- **Allowed origin:** `https://<org>.github.io`
- **Allowed redirect URI:** `https://<org>.github.io/callback`

The redirect URI must match exactly what your bot sends during sign-in.

## Privacy

- **No third-party anything.** No external scripts, fonts, images, analytics or trackers. A Content-Security-Policy blocks all network requests from the page.
- **The code never leaves your browser.** The callback page reads the query string with plain JavaScript and only shows it to you. Nothing is sent anywhere; you copy it yourself.
- **No referrer leaks.** The page sets `referrer` to `no-referrer`, so the optional coffee link doesn't receive your callback URL.
- **Short-lived.** The authorisation code expires within minutes and works only once, together with your bot's own secret.
- **Your private key never leaves your bot's computer.** Each user's bot generates its own key pair; only the public half is published here. Never commit a private key to this repo.

## Placeholders

| Placeholder | Where | Replace with |
| --- | --- | --- |
| `{{OWNER}}` | `index.html` | The owner's display name, for example `Jane`. |
| `{{COFFEE_URL}}` | `callback.html` (the `coffee-url` meta tag) | An `https://` tip link (for example `https://buymeacoffee.com/yourname`), or an empty string to hide every coffee element. |

The coffee link is optional and never gets in the way: the callback link is always visible and copyable, and a gentle "Fancy buying me a coffee?" prompt appears only *after* a successful copy, with **Yes** and **No thanks**. Anything that isn't an `https://` URL (including an unreplaced placeholder) hides the coffee bits.

## Notes

- GitHub Pages serves `/callback` by redirecting to `/callback/`; the query string is kept, so `https://<org>.github.io/callback?code=...&state=...` works.
- The `.well-known` folder only works because of `.nojekyll`. Don't delete it.
- The `screenshots/` folder (if present) is just a preview and can be deleted.

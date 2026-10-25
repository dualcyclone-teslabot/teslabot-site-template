# Tesla bot site template

A tiny, static GitHub Pages site for anyone setting up a personal **Tesla Fleet API** bot. It does two jobs:

1. **Hosts your public key** at `/.well-known/appspecific/com.tesla.3p.public-key.pem`, which Tesla fetches to verify your app.
2. **Provides the OAuth redirect page** at `/callback`, which shows the one-time code so you can copy it back into your bot.

No build step, no Jekyll, no scripts from anywhere else, no trackers.

## What's inside

| Path | Purpose |
| --- | --- |
| `.nojekyll` | Tells GitHub Pages to serve files as-is (otherwise it skips `.well-known`). |
| `index.html` | About page explaining the project: why it exists, why it's good, how it works and a "Use this template" link, plus a small "This copy is run by …" line and a link to the public key. |
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

## Keep your site online

Your Tesla connection depends on this site for as long as you use it. Tesla expects the public key to stay at `/.well-known/appspecific/com.tesla.3p.public-key.pem`, and the `/callback` page is needed whenever you reconnect. Don't delete, rename or make the repository private. It's free and needs no upkeep.

## Privacy

- **No third-party anything.** No external scripts, fonts, images, analytics or trackers. A Content-Security-Policy blocks all network requests from the page.
- **The code never leaves your browser.** The callback page reads the query string with plain JavaScript and only shows it to you. Nothing is sent anywhere; you copy it yourself.
- **No referrer leaks.** The page sets `referrer` to `no-referrer`, so the optional coffee link never receives your code or callback URL.
- **Short-lived.** The authorisation code expires within minutes and works only once, together with your bot's own secret.
- **Your private key never leaves your bot's computer.** Each user's bot generates its own key pair; only the public half is published here. Never commit a private key to this repo.

## Placeholders

| Placeholder | Where | Replace with |
| --- | --- | --- |
| `{{OWNER}}` | `index.html` | The owner's display name for the small "This copy is run by …" line, for example `Jane`. Left unfilled, that line is hidden. |
| `{{OWNER_GITHUB_URL}}` | `index.html` | The owner's GitHub profile URL (`https://github.com/...`), or empty to hide it. |
| `{{OWNER_X_URL}}` | `index.html` | The owner's X profile URL (`https://x.com/...`), or empty to hide it. |
| `{{COFFEE_URL}}` | `index.html` and `callback.html` (the `coffee-url` meta tag) | An `https://` tip link (for example `https://buymeacoffee.com/yourname`), or empty to hide every coffee element. |

Each owner link on the about page shows only when its placeholder holds a real `https://` URL; empty or unfilled ones are hidden, and the row of links disappears if none are left. Owner links that match the footer credit links are skipped, so they never appear twice.

The footer credit (**Made by Luke**, with GitHub, X and Buy me a coffee links) is fixed text on both pages, not a placeholder. Please keep it if you can. The callback page's footer also links back to the about page (`/`).

## How the callback page behaves

- **Success (`?code=...`):** the page shows **only the code** (the `code` query parameter), not the full URL, in a large box with one big **Copy code** button that copies just the code. It says: *"Copy this code and paste it into the secure field your bot gave you. It expires in a few minutes."*
- **After a successful copy:** the code panel is replaced in place by a short panel: **Copied ✓ Now paste it into your bot.** If a coffee URL is set, it adds *"Saved you some faff? Fancy buying me a coffee?"* with a **Yes ☕** button (opens the link in a new tab, `rel="noopener noreferrer"`). A **Back to my code** button always brings the code panel back so you can copy again. The code is never hidden behind the coffee prompt; the prompt only appears once you've already copied it.
- **If copying fails:** the panel doesn't change. The code is selected and the page asks you to copy it manually.
- **Error (`?error=access_denied` etc.):** a kind message asking you to go back to your bot and try again.
- **No code:** a short explanation of what the page is for.

The bot accepts **either the bare code or the full callback URL** in its secure field, so pasting the code from this page or the whole address from the browser bar both work. The bot checks `state` itself.

The coffee link is optional. If `{{COFFEE_URL}}` is empty, or anything that isn't an `https://` URL (including an unreplaced placeholder), the post-copy panel just shows **Copied ✓** and **Back to my code**, with no coffee bits.

## Notes

- GitHub Pages serves `/callback` straight from `callback.html` (no redirect), so `https://<org>.github.io/callback?code=...&state=...` works as is.
- The `.well-known` folder only works because of `.nojekyll`. Don't delete it.
- The `screenshots/` folder (if present) is just a preview and can be deleted.

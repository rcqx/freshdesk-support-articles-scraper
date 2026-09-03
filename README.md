# A simple Freshdesk support articles scraper

Small scraper that fetches support articles stored within Freshdesk.

How to run (single-article / test modes):

- Install dependencies (if not already):

```bash
npm install
```

- Run normally (scrapes discovered KB articles):

```bash
node scraper.js
```

- Run a single article (accepts full URL or path):

```bash
# full URL
node scraper.js --single https://help.appenate.com/article/some-article

# relative path
node scraper.js --single /article/some-article

# or via env var
SINGLE_ARTICLE_URL="/article/some-article" node scraper.js
```

- Limit the number of discovered articles (useful for quick tests):

```bash
node scraper.js --limit 2
```

Notes:
- The script uses `DATABASE_URL` from environment and will create tables if they don't exist.
- `util/correctImages.js` requires `GEMINI_API_KEY` in the environment (see Environment variables / secrets below).
- It also saves the screaped content in a json file
- Images are saved to `./downloaded_images` by default.
- The default delay between requests is 2s; adjust `CONFIG.delayBetweenRequests` in `scraper.js`.

Environment variables / secrets
---------------------------------
Add these to your `.env` (never commit it — `.env` is gitignored):

```
# Postgres connection string used by the scraper
DATABASE_URL=postgres://user:password@host:5432/dbname

# Google Gemini API key, used by util/correctImages.js for image editing
GEMINI_API_KEY=your-gemini-api-key
```

Get a Gemini API key from Google AI Studio (https://aistudio.google.com/apikey). The
image correction script uses the `models/gemini-2.5-flash-image` model, so the key must
belong to a project with access to that model.

Login (headful testing)
---------------------------------
If the KB requires authentication, set these env vars (or add to your `.env`):

```
LOGIN_USERNAME=your-username-or-email
LOGIN_PASSWORD=your-password
# optional overrides
LOGIN_URL=https://help.appenate.com/login
# If the page uses non-standard selectors, override these
LOGIN_USERNAME_SELECTOR="input[name=\"email\"]"
LOGIN_PASSWORD_SELECTOR="input[name=\"password\"]"
LOGIN_SUBMIT_SELECTOR="button[type=\"submit\"]"
```

Run with headful mode (so you can watch the browser):

```bash
# set headless to false in scraper.js or run with an env var if you prefer
node scraper.js --single /article/creating-forms-with-ai/
```

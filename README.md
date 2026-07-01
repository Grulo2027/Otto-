# Otto — landing page + live AI demo

A single-page waitlist site for **Otto, your AI Chief of Staff**, with a demo that
generates a personalized "morning brief" from whatever business a visitor types in.

## How it works

- `index.html` — the whole landing page (styles + logic inline). The waitlist is
  stored in the visitor's browser (`localStorage`).
- `api/demo.js` — a serverless function that runs **Otto's persona** through the
  Claude API and returns a tailored brief as JSON.
- The page calls `POST /api/demo`. **If that call fails** (e.g. you opened the
  file locally with no server, or the API errors), it automatically falls back to
  built-in template briefs — so the demo always shows *something*.

> The demo never reads anyone's real email or calendar. It *imagines* a realistic
> day for the visitor's type of business. Connecting a real inbox is a separate,
> post-signup product step.

## Test it locally (two ways)

**A) Just open the file** — double-click `index.html`. The AI endpoint won't exist
locally, so the demo uses the template fallback. Good for checking layout/copy.

**B) Run the real AI demo locally** — needs Node 18+ and the Vercel CLI:

```bash
npm i -g vercel
cd otto-site
cp .env.example .env        # then paste your real ANTHROPIC_API_KEY into .env
vercel dev                  # serves the page AND /api/demo on http://localhost:3000
```

Open the printed URL, fill in the form, and hit **Run live demo** — you'll see a
brief generated live by Claude (the badge reads "AI-generated").

## Deploy (Vercel — recommended)

1. Create a free account at https://vercel.com.
2. Put this `otto-site` folder in a Git repo (GitHub/GitLab) **or** run `vercel`
   from inside the folder to deploy directly from your machine.
3. In the Vercel project: **Settings → Environment Variables**, add
   `ANTHROPIC_API_KEY` = your key from https://console.anthropic.com
   (optionally `OTTO_MODEL`).
4. Deploy. Your live URL (e.g. `https://otto.vercel.app`) serves the page and the
   `/api/demo` endpoint together.

That's the real "connection": the deployed site's demo is powered by Otto.

## Cost & safety notes

- The API key lives **only on the server** (env var) — never in `index.html`.
- Each demo is one short Claude call (fractions of a cent on Haiku). Consider
  adding rate-limiting before sharing the link widely.
- To collect waitlist emails for real (not just browser-local), wire the form's
  submit to a form backend (Formspree/Tally) or another serverless function.

## Files

```
otto-site/
├─ index.html        # landing page + demo UI (+ template fallback)
├─ api/demo.js       # serverless: Otto persona → Claude API → brief JSON
├─ package.json
├─ vercel.json       # function config
├─ .env.example      # copy to .env, add your key
└─ .gitignore
```

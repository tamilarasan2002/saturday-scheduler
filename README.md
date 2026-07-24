# Alternate Saturday Scheduler

A single-page app to manage **Batch 1 / Batch 2** alternate-Saturday working.
One Saturday Batch 1 works (Batch 2 off), next Saturday Batch 2 works (Batch 1 off).
Data is stored in the browser (`localStorage`) — no database, no server, no cost.

## How to use
1. Open `index.html` in any browser.
2. **Rotation setup** — pick any known Saturday and choose which batch works that day. Everything else alternates automatically.
3. **Team members** — add names to Batch 1 and Batch 2. Double-click a name to move it to the other batch; click ✕ to remove.
4. **This Saturday** card shows who is *working* and who is *off*.
5. **Calendar** colours every Saturday by the batch on duty. Use the dropdown to highlight one person's working Saturdays.
6. **Export / Import** JSON to move the setup to another device.

## Deploy to Netlify (free, no login needed for drag & drop)
1. Go to https://app.netlify.com/drop
2. Drag this whole `alternativesaturday` folder onto the page.
3. Netlify gives you a live URL — share it with teammates.

> Note: `localStorage` is **per-browser**. Each teammate sees their own copy.
> To share one setup: set it up once, **Export JSON**, and send the file so others **Import** it.
> (If you later want everyone to see the *same shared* data live, that needs a small backend/DB — this version is intentionally free & local.)

## Files
- `index.html` — the entire app (HTML + CSS + JS, self-contained)
- `netlify.toml` — Netlify config

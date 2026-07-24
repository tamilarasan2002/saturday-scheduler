# Alternate Saturday Scheduler

A single-page static app to manage **Batch 1 / Batch 2** alternate-Saturday working.
One Saturday Batch 1 works (Batch 2 off), the next Saturday Batch 2 works (Batch 1 off), repeating.

- **No backend to run, no build tools** — plain HTML/CSS/JS in one file.
- **Free cloud sync** via Supabase free tier (optional) so all teammates see the **same live data**.
- Works offline with `localStorage` as a fallback when cloud sync is not configured.

---

## Features
- **Member management** — add / edit (✎) / delete (✕) members in each batch; double-click a name to move it to the other batch.
- **Rotation setup** — pick any reference Saturday + the batch working that day; the app computes every other Saturday automatically.
- **Calendar** — monthly view, every Saturday coloured by the batch on duty; navigate months; highlight one person's working Saturdays.
- **Saturday summary** — "This Saturday" or pick any date to see who is **Working** 🟢 vs **Off** 😴.
- **Cloud Sync / Team link** — connect Supabase, then share a link so colleagues auto-connect to the same data.
- **Export / Import / Reset** JSON backup.

---

## How the rotation logic works
1. You set a **reference Saturday** and say which batch works that day.
2. For any Saturday, the app counts how many weeks away it is from the reference.
   - Even number of weeks → same batch as the reference works.
   - Odd → the other batch works.
3. So the two batches simply alternate every Saturday, forever, in both directions.

---

## Deploy to Netlify (free, no login needed)
1. Go to <https://app.netlify.com/drop>
2. Drag the whole `alternativesaturday` folder onto the page.
3. Netlify gives you a live URL — share it with teammates.

(Or connect the GitHub repo to Netlify for auto-deploy on every push.)

---

## Data storage — read this

### Default: `localStorage` (per-device)
If you don't set up cloud sync, all data is stored in **that one browser only**.
It will **NOT** sync across different teammates' devices — each person sees their own copy.

### Recommended: free shared cloud sync (Supabase)
So everyone sees the **same synced data**:

1. Create a free account at <https://supabase.com> → **New project**.
2. Project → **Settings → API** → copy the **Project URL** and the **anon public** key.
3. Project → **SQL Editor** → run:
   ```sql
   create table if not exists app_state (
     id text primary key,
     data jsonb not null,
     updated_at timestamptz default now()
   );
   alter table app_state enable row level security;
   create policy "team read"  on app_state for select using (true);
   create policy "team write" on app_state for insert with check (true);
   create policy "team update" on app_state for update using (true);
   ```
4. In the app → **Cloud Sync** card → paste URL + anon key + a **Team ID** (e.g. `main`) → **Connect & Sync**.
5. Click **🔗 Copy team link** and send it to colleagues. When they open it, the app auto-connects to the same team data. Changes sync automatically (polled every ~8s; last-write-wins).

> The **anon public** key is meant to be used in the browser and is fine to share **within your team** for this internal tool. The permissive SQL policies above give anyone with the key read/write access — keep the link inside the team. If you need stricter access later, add Supabase Auth + row-level policies.

### Other free alternatives
- **Firebase Firestore** free tier (similar browser SDK approach).
- **Netlify Blobs** (needs a small Netlify Function).
- **JSONBin.io** (simple free JSON store).
Supabase is used here because it's free, reliable, and callable directly from a static page.

---

## Files
- `index.html` — the entire app (HTML + CSS + JS, self-contained)
- `netlify.toml` — Netlify config
- `README.md` — this file

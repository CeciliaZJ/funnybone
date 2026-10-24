# Funnybone local setup status

**App:** Funnybone (Crackd.ai front end, course ZIP from commit `ed536ef`)
**Chosen app confirmed:** yes. Other semester apps ignored.

## Extract / copy paths

| Role | Path |
| --- | --- |
| Original extract (Downloads) | `/Users/ceciliazhuo/Downloads/funnybone` |
| Working copy used for install/run | `/Users/ceciliazhuo/IdeaProjects/funnybone` |

That directory contains all of: `AGENTS.md`, `COURSE_SETUP.md`, `COURSE_PACKAGE.json`, `docs/caption-jobs.md`, `sst.config.ts`, `package.json`, `README.md`. Loading smileys are at `public/loading/loading1.png`–`loading8.png`.

No Funnybone/teaching-data ZIP file remains in Downloads/Desktop/Documents/IdeaProjects (already extracted folder only). Shared anonymized Supabase ZIP with `RESTORE.md` / `REPOSITORIES.md` was **not found** on this Mac.

## Commands run

```bash
# Node already 24.13.0 (/usr/local/bin/node)
rsync -a --exclude node_modules --exclude .next \
  ~/Downloads/funnybone/ ~/IdeaProjects/funnybone/
cd ~/IdeaProjects/funnybone
cp .env.example .env.local
npm ci
npm run dev   # Next.js 16.2.12 turbopack
```

Did not overwrite `assignment1` or `adminpanel`. No upgrade work.

## URL / port

- Funnybone: **http://localhost:3001**
- Port 3000 is already taken by assignment1 (`Morningside Nook` at `~/IdeaProjects/assignment1`). Dev server fell back to 3001.

Env file loaded: `.env.local` (placeholder keys only).

## What works

- `npm ci` completed (Node 24.x).
- Dev server is up on :3001.
- `/login` 200 — Google login + “Continue with Email”
- `/login-with-email` 200 — email/password form
- `/sign-up` 200
- `/get-started`, `/about`, `/my-images`, `/top-100` 200 (UI shell; no restored data)

## What does not work yet

- **`/` returns 500** — `fetchPopularCaptionsAction` cannot reach a real Supabase project (`TypeError: fetch failed` against placeholder `https://YOUR_PROJECT_REF.supabase.co`).
- Imported teaching rows, image URLs, login against real users, admin, and caption API: blocked until her project + restore + keys.

## Missing Supabase / keys / PM items (exact paste)

File to edit: `/Users/ceciliazhuo/IdeaProjects/funnybone/.env.local`

**Required — paste from her own new empty Supabase project (Settings → API):**

```
NEXT_PUBLIC_SUPABASE_URL=https://<her-project-ref>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<her-public-anon-key>
```

Do not reuse anyone else’s project (the source `package.json` `supabase:types` id `qihsgnfjqmkjmoowyfbn` is not hers). Do not copy keys from assignment1.

**Already set:**

```
NEXT_PUBLIC_BASE_URL=http://localhost:3000
```

(After keys are real, either keep this or set it to `http://localhost:3001` to match the port actually in use. Restart `npm run dev`.)

**Optional (do not block start):**

```
NEXT_PUBLIC_API_BASE_URL=          # caption/upload API is not in the ZIP
NEXT_PUBLIC_GOOGLE_CLIENT_ID=      # needed only if using Google OAuth
```

## Teaching-data ZIP (not on this Mac)

She still needs the shared anonymized data ZIP:

1. Create a **new empty Supabase project she controls**.
2. Follow that ZIP’s **`RESTORE.md`** (schema + anonymized rows).
3. Follow **`REPOSITORIES.md`** for Funnybone login, admin, and services.
4. Create **her own test user** in that project (email/password via `/sign-up` or `/login-with-email`, or Google OAuth if she configures it). The Funnybone ZIP has **no canned test-user credentials**; COURSE_SETUP.md says use a signed-in session on her project.

Image URLs in imported rows need internet. Caption/upload/AI services are out of the ZIP and are not required for the app process to start.

## Login notes from this repo (COURSE_SETUP.md + auth pages)

- Primary UI: **Login with Google** (`/login`) plus **Continue with Email** (`/login-with-email`).
- If using Google: configure the provider and local redirect URLs on **her** Supabase project; set `NEXT_PUBLIC_GOOGLE_CLIENT_ID` if the course flow needs it.
- She must create her own test user after restore. No upgrade until data + login work.

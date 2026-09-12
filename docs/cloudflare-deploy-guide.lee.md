# Cloudflare Deployment Guide — Arto's Repos

**Account ID:** `5a01b546bdf7de66a7ff19c5117911c7`
**GitHub owner:** `traali`
**Date:** 2026-09-12

---

## Repo → Destination mapping

| GitHub Repo | Destination | Type | Git connected? | Status |
|-------------|-------------|------|----------------|--------|
| `traali/pelipaiva` | Worker `pelipaiva` | Worker (Git builds) | ✅ | OK — verify bindings |
| `traali/sakkoja` | Pages `sakkoja` | Pages (Git) | ✅ | OK |
| `traali/football-stats` | Pages `football-stats` | Pages (direct upload) | ❌ | Redeploy + connect Git |
| `traali/floorball-stats` | Pages `floorball-stats` | Pages (direct upload) | ❌ | Redeploy + connect Git |
| `traali/basketball-stats` | Pages `basketball-stats` | Pages (direct upload) | ❌ | Redeploy + connect Git |
| `traali/volleyball-stats` | Pages `volleyball-stats` | Pages (direct upload) | ❌ | Redeploy + connect Git |
| `traali/Parkkis` | Pages `parkkis` | Pages (direct upload) | ❌ | Redeploy + connect Git |
| `traali/weather-stats` | Pages `weather-stats` | Pages (new) | ❌ | Create + deploy + connect Git |
| `traali/sports-federation` | — | — | — | **SKIP** (not needed) |

---

## Existing account resources

### Workers (9)
| Worker | URL | Git | Notes |
|--------|-----|-----|-------|
| `pelipaiva` | pelipaiva.sakkoja.workers.dev | ✅ traali/pelipaiva | Main app, has assets, active |
| `pelipaiva-edge` | pelipaiva-edge.sakkoja.workers.dev | ❌ | Edge worker |
| `taso-proxy` | taso-proxy.sakkoja.workers.dev | ❌ | Proxy |
| `sakkoja-cors-proxy` | — | ❌ | Old? |
| `smallbusinesspages` | — | ✅ | Has assets |
| `muistot-remote` | — | ❌ | |
| `muistot` | — | ❌ | Durable Object: MemvidServer |
| `recursive-lm-worker` | — | ❌ | |
| `memvid-cloudflare` | — | ❌ | Durable Object: MemvidServer |

### Pages projects (7)
| Project | URL | Git | Last deploy |
|---------|-----|-----|------------|
| `football-stats` | football-stats-agk.pages.dev | ❌ | 2h ago |
| `pelipaiva` | pelipaiva.pages.dev | ❌ | 2d ago — **DUPLICATE, delete** |
| `basketball-stats` | basketball-stats-byu.pages.dev | ❌ | 2d ago |
| `volleyball-stats` | volleyball-stats-7xq.pages.dev | ❌ | 9d ago |
| `floorball-stats` | floorball-stats.pages.dev | ❌ | 9d ago |
| `parkkis` | parkkis.pages.dev | ❌ | 9d ago |
| `sakkoja` | sakkoja.pages.dev | ✅ traali/sakkoja | 11d ago |

### KV namespaces (2)
| Name | ID |
|------|----|
| `CONTEXT_KV` | `66d9ff6eb08343afbe3a2a67befd0ed1` |
| `MATCHDAY_KV` | `10b2dc844fe04f01920bdfba6fdecda5` |

### R2 buckets (1)
| Bucket | Objects | Size |
|--------|---------|------|
| `memvid-store` | 491 | 7.2 MB |

### D1 databases (1)
| Name | UUID | Tables | Size | Created |
|------|------|--------|------|---------|
| `muistot-db` | `29932d66-bb1c-4682-b592-4a1e13e6ff1c` | 0 | 614.4 kB | May 4 2026 |

---

## Step 0 — Prerequisites

```bash
npm install -g wrangler
npx wrangler login
npx wrangler whoami   # verify Account ID 5a01b546bdf7de66a7ff19c5117911c7
```

---

## Step 1 — Deploy `weather-stats` (new Pages project)

```bash
git clone https://github.com/traali/weather-stats.git
cd weather-stats
npm install
npm run build
# NOTE: adjust output dir if not ./dist (could be build/, out/, .output/public)
npx wrangler pages deploy ./dist --project-name=weather-stats
cd ..
```

**Result:** New Pages project `weather-stats` created, live at a random `weather-stats-*.pages.dev` URL.

---

## Step 2 — Redeploy 5 existing Pages projects (direct upload)

For each repo, clone → install → build → deploy. **Check each repo's build output directory** (dist, build, out, .output/public) and adjust.

```bash
# football-stats
git clone https://github.com/traali/football-stats.git
cd football-stats && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=football-stats
cd ..

# floorball-stats
git clone https://github.com/traali/floorball-stats.git
cd floorball-stats && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=floorball-stats
cd ..

# basketball-stats
git clone https://github.com/traali/basketball-stats.git
cd basketball-stats && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=basketball-stats
cd ..

# volleyball-stats
git clone https://github.com/traali/volleyball-stats.git
cd volleyball-stats && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=volleyball-stats
cd ..

# parkkis (repo has capital P, project name is lowercase)
git clone https://github.com/traali/Parkkis.git
cd Parkkis && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=parkkis
cd ..
```

**Result:** Existing Pages projects updated with latest code. URLs serve new version immediately. No Git connection created (still manual deploys).

---

## Step 3 — Connect Git repos for auto-deploy (dashboard only)

CLI **cannot** connect a GitHub repo to a Pages project. Must be done in dashboard for each of the 6 projects (football-stats, floorball-stats, basketball-stats, volleyball-stats, parkkis, weather-stats):

1. Go to Workers & Pages → click the Pages project
2. **Settings** → **Build & Development** → **Connect to Git**
3. Authorize Cloudflare's GitHub app if prompted
4. Select repo (e.g. `traali/football-stats`)
5. Production branch: `main`
6. Build command: `npm run build`
7. Build output directory: `dist` (match the repo)
8. Root directory: `/`
9. **Save and Deploy**

**Result:** Every push to `main` auto-triggers a build+deploy in Cloudflare's CI. Free plan = 1 concurrent build (builds queue).

---

## Step 4 — Delete `pelipaiva` Pages duplicate

```bash
npx wrangler pages project list          # confirm names
npx wrangler pages project delete pelipaiva
```

**Result:** `pelipaiva.pages.dev` removed. The `pelipaiva` **Worker** (pelipaiva.sakkoja.workers.dev) is unaffected. Permanent — verify before deleting.

---

## Step 5 — Verify KV bindings on `pelipaiva` Worker

```bash
npx wrangler kv namespace list
# CONTEXT_KV    66d9ff6eb08343afbe3a2a67befd0ed1
# MATCHDAY_KV   10b2dc844fe04f01920bdfba6fdecda5

cd pelipaiva && cat wrangler.toml   # or wrangler.jsonc
```

If missing, add to `wrangler.toml`:

```toml
[[kv_namespaces]]
binding = "CONTEXT_KV"
id = "66d9ff6eb08343afbe3a2a67befd0ed1"

[[kv_namespaces]]
binding = "MATCHDAY_KV"
id = "10b2dc844fe04f01920bdfba6fdecda5"
```

Redeploy:
```bash
npx wrangler deploy --name pelipaiva
```

**Result:** Worker code can access `env.CONTEXT_KV` and `env.MATCHDAY_KV`. KV data unchanged.

---

## Step 6 — Verify R2 binding on `muistot` / `memvid-cloudflare`

```bash
npx wrangler r2 bucket list   # shows memvid-store
cd muistot && cat wrangler.toml
```

If missing, add:

```toml
[[r2_buckets]]
binding = "memvid-store"
bucket_name = "memvid-store"
```

Redeploy:
```bash
npx wrangler deploy --name muistot
```

Repeat for `memvid-cloudflare` if it needs the bucket.

**Result:** Worker can read/write objects in `memvid-store` via `env.memvid-store`.

---

## Step 7 — Verify D1 binding on `muistot` Worker

```bash
npx wrangler d1 list   # shows muistot-db 29932d66-bb1c-4682-b592-4a1e13e6ff1c
cd muistot && cat wrangler.toml
```

If missing, add:

```toml
[[d1_databases]]
binding = "muistot-db"
database_name = "muistot-db"
database_id = "29932d66-bb1c-4682-b592-4a1e13e6ff1c"
```

Redeploy:
```bash
npx wrangler deploy --name muistot
```

Database currently has **0 tables** — run migrations if needed:
```bash
npx wrangler d1 execute muistot-db --file=./schema.sql
```

**Result:** Worker can run SQL via `env.muistot-db.prepare(...)`.

---

## Step 8 — Skip `sports-federation`

No action.

---

## Recommended execution order

```bash
# 1. Login
npx wrangler login

# 2. Deploy weather-stats (new)
git clone https://github.com/traali/weather-stats.git
cd weather-stats && npm install && npm run build
npx wrangler pages deploy ./dist --project-name=weather-stats
cd ..

# 3. Redeploy 5 existing Pages projects
for repo in football-stats floorball-stats basketball-stats volleyball-stats Parkkis; do
  git clone https://github.com/traali/$repo.git
  cd $repo
  npm install
  npm run build
  project=$(echo $repo | tr '[:upper:]' '[:lower:]')
  npx wrangler pages deploy ./dist --project-name=$project
  cd ..
done

# 4. Delete pelipaiva Pages duplicate
npx wrangler pages project delete pelipaiva

# 5. Verify bindings
npx wrangler kv namespace list
npx wrangler r2 bucket list
npx wrangler d1 list
# check each wrangler.toml, add missing bindings, redeploy

# 6. Connect Git repos via dashboard (cannot be done via CLI)
```

---

## Final checklist

- [ ] Deploy `weather-stats` Pages (new)
- [ ] Redeploy `football-stats`, `floorball-stats`, `basketball-stats`, `volleyball-stats`, `parkkis` Pages
- [ ] Delete `pelipaiva` Pages duplicate
- [ ] Verify `CONTEXT_KV` + `MATCHDAY_KV` bound to `pelipaiva` Worker
- [ ] Verify `memvid-store` R2 bound to `muistot` / `memvid-cloudflare`
- [ ] Verify `muistot-db` D1 bound to `muistot`
- [ ] Connect Git to all 6 Pages projects (dashboard)
- [ ] Skip `sports-federation`

---

## Gotchas

- **Build output directory** varies per repo (`dist`, `build`, `out`, `.output/public`) — check `package.json` before deploying.
- **Parkkis** repo name has a capital P (`traali/Parkkis`) but the Pages project is lowercase `parkkis`.
- **Free plan**: 1 concurrent build — pushes to multiple repos queue up.
- `wrangler pages project delete` is **permanent** and cannot be undone.
- Direct upload deploys do **not** create a Git connection — that step is dashboard-only.
- `pelipaiva` exists as both Worker (keep) and Pages (delete) — do not confuse them.

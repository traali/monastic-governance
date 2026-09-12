# Cellarer guide — Cloudflare for the monasteries

Account: `5a01b546bdf7de66a7ff19c5117911c7`  
GitHub: `traali`  
Rule: **Pages = UI. One Worker = Torneopal. Pelipäivä = Worker + KV.**

Do this on a machine where `npx wrangler login` works (your laptop). This sandbox cannot finish OAuth.

---

## 0. Login

```bash
npm i -g wrangler
npx wrangler login
npx wrangler whoami
# expect Account ID 5a01b546bdf7de66a7ff19c5117911c7
```

---

## 1. What each repo is

| Repo | Cloudflare | Notes |
|---|---|---|
| football-stats | **Pages** `football-stats` | UI. Torneopal via Worker `taso-proxy` |
| floorball-stats | **Pages** `floorball-stats` | UI. Proxy path `/ssbl` |
| basketball-stats | **Pages** `basketball-stats` | UI. Proxy path `/basket` |
| volleyball-stats | **Pages** `volleyball-stats` | UI. Proxy path `/volley` |
| Parkkis | **Pages** `parkkis` | UI. Helsinki/Espoo from the browser |
| weather-stats | **Pages** `weather-stats` | UI. FMI from the browser |
| sakkoja | **Pages** `sakkoja` | already Git-connected |
| pelipaiva | **Worker** `pelipaiva` (UI assets) + **Worker** `pelipaiva-edge` (KV) | do **not** delete Pages until redirect |
| football-stats/workers/taso-proxy | **Worker** `taso-proxy` | shared cache/CORS for all four sports |
| sports-federation | skip | |

**Project name ≠ hostname.** Wrangler uses the project name:

| `--project-name` | Live URL |
|---|---|
| `football-stats` | https://football-stats-agk.pages.dev |
| `floorball-stats` | https://floorball-stats.pages.dev |
| `basketball-stats` | https://basketball-stats-byu.pages.dev |
| `volleyball-stats` | https://volleyball-stats-7xq.pages.dev |
| `parkkis` | https://parkkis.pages.dev |
| `weather-stats` | created on first deploy |
| `sakkoja` | https://sakkoja.pages.dev |
| `pelipaiva` | https://pelipaiva.pages.dev |

Never deploy to `football-stats-agk` / `basketball-stats-byu` / `volleyball-stats-7xq`. Those are hostnames.

---

## 2. Direct-upload deploys (unblocks prod today)

From a clone of each house (`/tmp/houses/…` is fine if it is on `main`):

```bash
# football
cd football-stats
npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=football-stats

# floorball
cd ../floorball-stats
npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=floorball-stats

# basketball
cd ../basketball-stats
npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=basketball-stats

# volleyball
cd ../volleyball-stats
npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=volleyball-stats

# Parkkis — output is web/dist, default branch is master
cd ../Parkkis
cd web && npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=parkkis

# weather-stats — creates the project if missing
cd ../../weather-stats
npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=weather-stats
```

Then the Torneopal cache Worker (from football-stats):

```bash
cd football-stats/workers/taso-proxy
npx wrangler deploy
# → https://taso-proxy.sakkoja.workers.dev
```

Hard-refresh the `*.pages.dev` URLs. Football player cards (syksy first, DNP boxes) and floorball discovery ship only after this step.

---

## 3. Auto-deploy on push (dashboard, once)

CLI cannot attach Git. For each Pages project:

1. [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → project  
2. **Settings → Builds → Connect to Git**  
3. Repo as in the table, production branch **`main`** (Parkkis: **`master`**)  
4. Framework: Vite. Node 22. Install `npm ci`. Build `npm run build`. Output `dist`.  
5. Root `/` except Parkkis root **`web`**.  
6. Save and Deploy.

After this, GitHub Actions does **not** need `CLOUDFLARE_API_TOKEN` for Pages. Keep the token only for `taso-proxy` / `pelipaiva-edge` Worker deploys.

Optional GitHub secrets (if you keep Actions CD):

| Secret | On which repos |
|---|---|
| `CLOUDFLARE_API_TOKEN` | all houses that still use `.github/workflows/cd.yml` |
| `CLOUDFLARE_ACCOUNT_ID` | `5a01b546bdf7de66a7ff19c5117911c7` |

Token permissions: Account → Cloudflare Pages **Edit**, Workers Scripts **Edit**.

---

## 4. Pelipäivä — do not delete Pages yet

| Piece | Keep? |
|---|---|
| Worker `pelipaiva` (Git, assets) → pelipaiva.sakkoja.workers.dev | yes — newer app |
| Worker `pelipaiva-edge` + KV | yes — family API |
| Pages `pelipaiva` → pelipaiva.pages.dev | **keep until** that hostname redirects to the Worker / custom domain |

Delete only after:

```bash
# confirm Worker serves the PWA
curl -sI https://pelipaiva.sakkoja.workers.dev | head
# then, and only then:
# npx wrangler pages project delete pelipaiva
```

KV on `pelipaiva-edge` (`cloudflare-worker/wrangler.jsonc`):

```jsonc
"kv_namespaces": [
  { "binding": "MATCHDAY_KV", "id": "10b2dc844fe04f01920bdfba6fdecda5" }
]
```

Do **not** add `CONTEXT_KV` — the worker never reads `env.CONTEXT_KV`.

---

## 5. Skip

- `sports-federation` — kattorepo, no prod
- `CONTEXT_KV` `66d9ff6eb08343afbe3a2a67befd0ed1` — unused here
- R2 `memvid-store`, D1 `muistot-db`, Workers `muistot` / `memvid-cloudflare` — other product
- Turning each stats SPA into its own Worker — that is `taso-proxy`’s job

---

## 6. Checklist

- [ ] `wrangler login` on your machine
- [ ] Deploy 5 existing Pages + create `weather-stats`
- [ ] Deploy `taso-proxy`
- [ ] Confirm floorball search (not match #8198) and football current-season cards
- [ ] Connect Git on those 6 Pages projects
- [ ] Leave `pelipaiva` Pages up
- [ ] Confirm `MATCHDAY_KV` on `pelipaiva-edge`
- [ ] Skip federation / muistot / CONTEXT_KV

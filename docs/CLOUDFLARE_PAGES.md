# Cloudflare Pages — Cellarer map

Source: Lee account scan 2026-09-12 (account `5a01b546bdf7de66a7ff19c5117911c7`).
**Project name ≠ hostname.** Wrangler `--project-name` is the first column.

| GitHub repo | Pages **project** | Live hostname | Branch | Root | Build | Output |
|---|---|---|---|---|---|---|
| traali/football-stats | `football-stats` | football-stats-agk.pages.dev | main | `/` | `npm run build` | `dist` |
| traali/floorball-stats | `floorball-stats` | floorball-stats.pages.dev | main | `/` | `npm run build` | `dist` |
| traali/basketball-stats | `basketball-stats` | basketball-stats-byu.pages.dev | main | `/` | `npm run build` | `dist` |
| traali/volleyball-stats | `volleyball-stats` | volleyball-stats-7xq.pages.dev | main | `/` | `npm run build` | `dist` |
| traali/Parkkis | `parkkis` | parkkis.pages.dev | **master** | **`web`** | `npm run build` | `dist` |
| traali/weather-stats | `weather-stats` | create | main | `/` | `npm run build` | `dist` |
| traali/sakkoja | `sakkoja` | sakkoja.pages.dev | Git already connected | | | |
| traali/pelipaiva | Pages `pelipaiva` **and** Worker `pelipaiva` | pelipaiva.pages.dev / pelipaiva.sakkoja.workers.dev | | | | |

## Do not

- Do **not** `wrangler pages deploy --project-name=football-stats-agk` (or `-byu` / `-7xq`). Those suffixes are hostnames, not project names. CD used to do this — it would miss prod.
- Do **not** delete Pages `pelipaiva` until `pelipaiva.pages.dev` is redirected. Worker `pelipaiva` (Git, assets) is the newer app; Pages is the public URL in current docs.
- Do **not** bind `CONTEXT_KV` on `pelipaiva-edge`. Code only uses `MATCHDAY_KV` (`10b2dc844fe04f01920bdfba6fdecda5`).
- Skip `sports-federation`. Skip `muistot` / `memvid-store` / D1 here — not this federation.

## CLI (needs `wrangler login`)

```bash
npx wrangler whoami   # 5a01b546bdf7de66a7ff19c5117911c7

# weather-stats (new)
cd weather-stats && npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=weather-stats

# existing
npx wrangler pages deploy ./dist --project-name=football-stats
npx wrangler pages deploy ./dist --project-name=floorball-stats
npx wrangler pages deploy ./dist --project-name=basketball-stats
npx wrangler pages deploy ./dist --project-name=volleyball-stats
# Parkkis:
cd web && npm ci && npm run build
npx wrangler pages deploy ./dist --project-name=parkkis
```

Git-connect is dashboard-only (Settings → Build → Connect to Git). After that, push to `main` (Parkkis: `master`) deploys without a GitHub token.

## Shared Worker

`taso-proxy.sakkoja.workers.dev` — Torneopal cache for all sport houses. Deploy from `football-stats/workers/taso-proxy`. Live matches `no-store`; upcoming 30s.

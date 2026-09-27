# games

Syncloud app: dedicated game server panel. Install it from the Syncloud store,
pick a game from the catalog, and the server runs on your own device.

Implements [syncloud/platform#35](https://github.com/syncloud/platform/issues/35) — SteamCMD + Pelican egg catalog.

## Architecture

- `cli/` — Go install/configure/access-change/storage-change/backup hooks (cobra).
- `backend/` — Go HTTP backend on `unix:/var/snap/games/current/backend.sock`.
- `web/` — Vue 3 + Vite SPA, store-styled (`../store/web` look). Plain CSS, no Element Plus.
- `config/` — nginx + authelia templates rendered at configure time.
- `nginx/` — vendored static nginx.
- `test/` — pytest integration tests.

## Constraints

- **amd64 only.** SteamCMD ships x86_64 binaries only.
- **Steam accounts.** Most popular servers (CS2/TF2/Gmod/Valheim/Project Zomboid/ARK) work with `+login anonymous`. Rust/Squad/Arma 3 need a real Steam account — the UI surfaces an optional credentials field.
- **Pelican eggs.** Best-effort: many upstream eggs assume Docker; we support the bash/native subset. A game that is not in the catalog is usually a day's work to add.

## Integration test fixture

We use **Teeworlds** (smallest egg in `parkervcp/eggs/game_eggs/teeworlds`, ~10MB binary, headless, A2S-queryable) as the install/start/stop fixture in CI.

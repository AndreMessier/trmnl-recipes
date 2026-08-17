# Custom TRMNL / LaraPaper Recipes

A small collection of custom recipes built for a self-hosted [LaraPaper](https://github.com/usetrmnl/larapaper)
instance. Each folder is a standalone recipe in the standard `settings.yml` + `.liquid` format,
importable via LaraPaper's **Plugins → Import → Import Recipe Archive**, or usable as a base for
a submission to the [community recipe catalog](https://github.com/bnussbau/trmnl-recipe-catalog).

## Recipes

*(Screenshots use stubbed example titles/times, not real personal data.)*

### `plex-watchlist-movies`
Your Plex Watchlist movies, newest release first. Data source: Plex cloud
(`discover.provider.plex.tv`). Needs: personal Plex token.

![plex-watchlist-movies](screenshots/plex-watchlist-movies.png)

### `plex-watchlist-episodes`
Your Plex Watchlist TV shows, sorted by most recent episode air date (not premiere date),
excludes anything more than 3 days out. Data source: Plex cloud. Needs: personal Plex token.

![plex-watchlist-episodes](screenshots/plex-watchlist-episodes.png)

### `plex-server-new-movies`
Recently added movies on your own Plex Media Server. Data source: your PMS. Needs: personal
Plex token + your server URL.

![plex-server-new-movies](screenshots/plex-server-new-movies.png)

### `plex-server-new-episodes`
Recently added TV episodes on your own PMS, aggregated by show (not one row per episode).
Data source: your PMS. Needs: personal Plex token + your server URL.

![plex-server-new-episodes](screenshots/plex-server-new-episodes.png)

### `wmata-bethesda-bus`
Combined bus arrival board for all three Metrobus routes serving Bethesda Station (D96, M22,
M70). Data source: WMATA NextBusService API. Needs: free WMATA developer API key.

![wmata-bethesda-bus](screenshots/wmata-bethesda-bus.png)

## Setup

Each recipe's `settings.yml` lists its `custom_fields` — after importing, fill those in via the
plugin's edit screen in LaraPaper (or TRMNL). No field in any of these files contains a real
credential; they're all Liquid placeholders (`{{ field_name }}`) resolved from your own saved
configuration values at render time.

- **Plex token**: Plex Web App → any title → ⋮ → Get Info → View XML → the `X-Plex-Token=`
  value is in the resulting page's URL.
- **Plex server section IDs**: visit `http://<your-server>:32400/library/sections` (with your
  token) to find the numeric ID for your Movies/TV Shows libraries — this varies per server
  depending on setup order, so it's not something that can be hardcoded.
- **WMATA API key**: free signup at [developer.wmata.com](https://developer.wmata.com/).

## Notes

- The `plex-watchlist-episodes` sort (by last-aired-episode date) happens client-side in the
  Liquid template, not via the Plex API's own `sort` parameter — Plex silently ignores
  `sort=lastEpisodeOriginallyAvailableAt` server-side, so this has to be handled in the recipe.
- The `wmata-bethesda-bus` recipe fetches three separate stop IDs (one per physical bus bay at
  Bethesda Station) via LaraPaper's multi-URL `polling_url` support (newline-separated URLs get
  merged into `data.IDX_0`, `data.IDX_1`, `data.IDX_2`), then combines and sorts them in Liquid.
  Adapting this to a different station means swapping the `StopID` values in `settings.yml`.

See [`CUSTOM_PLUGINS.md`](../CUSTOM_PLUGINS.md) in the parent directory for the full writeup,
including bugs found and fixed in *other* existing recipes along the way (not included in this
repo, since those are fixes to someone else's code, not new plugins — worth submitting as PRs
against their original repos instead).

## Authorship

These recipes were authored with [Claude Code](https://claude.com/claude-code), working through
LaraPaper's actual database and container to build, test, and debug each recipe against live
data before exporting it here.

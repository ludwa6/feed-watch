# feed-watch

Samples two RSS feeds every 15 minutes **from a GitHub-hosted runner** (Azure datacenter, US) and appends timing to `log.tsv`.

**Why:** FeedLand reports a high, rising error rate (`ESOCKETTIMEDOUT`) on `www.valedalama.net/feed/`, while a watcher on the MacMini **in Portugal** has recorded 477 consecutive clean fetches. The two observers disagree, and the Portuguese watcher cannot see the path FeedLand uses.

**Design:** `refarmer.blog/feed/` is the **control** — a known-healthy origin fetched from the same runner at the same moment. Three samples per feed per run.

**Reading it:**
- VdL fails here, refarmer.blog doesn't → the origin treats datacenter/foreign traffic differently (rate limiting, geo-routing). Not the feed, not the plugin.
- Both fail → GitHub's egress or the runner, not ICDsoft.
- Neither fails → the failure is specific to FeedLand's host, and the next question is for Dave, not Bruno.

Columns are curl's cumulative timers: `dns` → `connect` → `tls` (`time_appconnect`) → `ttfb` → `total`. Handshake = `tls − connect`; server think-time = `ttfb − tls`.

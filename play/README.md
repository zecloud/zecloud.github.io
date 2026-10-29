# Contribution arcade

A standalone, French-language personal page at `/play/`, extending the existing
ZeCloud blog without replacing it. This repository contains Gatsby's generated
output (the source blog lives in `zecloud/zecloud-blog`), so the arcade uses native
HTML, CSS, and JavaScript with no build step or external runtime dependencies.
Keep this directory when updating the generated blog.

## Contribution data

From the repository root, run:

```sh
python scripts/update-contributions.py
```

The script reads **only the public GitHub contribution calendar** for `zecloud`,
validates a full year of consecutive dates and intensity levels, and atomically
replaces `play/contributions.json`. It requires no GitHub token. Do not add a token
to this page or its assets. A failed request or changed GitHub markup fails the
command and preserves the previous snapshot. Review and commit refreshed data
with normal site updates; nothing here automatically publishes or deploys.

The browser fetches only the local JSON snapshot, never the GitHub API. It shows
the snapshot date (and labels snapshots older than eight days as historical).
The last 364 days are folded, oldest first, into 26 columns of 14 days each,
top to bottom then left to right. Green intensity is GitHub's 0–4 level, not a
claimed count of contributions. A dotted target is a contribution day. Eating
all targets wins. If data cannot load, an explicit error and neutral practice
mode keep the game usable without inventing contributions.

## Playing and accessibility

There is no autoplay. Select a speed, then **Jouer**. Arrow keys or WASD control
the focused board; Space starts/pauses, Escape pauses, and R resets. Direction
buttons support touch and keyboard use. Walls and the snake's body end play.
One turn is accepted per tick to prevent accidental double-turn reversals.

**Pas à pas** has no timer: each valid direction advances exactly one cell.
It is the default when reduced motion is requested. Text coordinates and a
**Lire la position** button expose head/target position without live-announcing
every timed movement. Pause, score, collision, and completion messages use a
live region. Leaving the arcade controls, hiding the page, or switching windows
pauses the game; returning never resumes it automatically.

## Checks

```sh
node --test tests/snake.test.mjs
python -m unittest discover -s tests -p 'test_*.py'
node --input-type=module --check < play/app.js
```

For local use, serve the repository root (for example
`python -m http.server 8000`) and open `/play/`. ES modules and JSON fetching
require HTTP, not `file://`. The read-only check workflow runs these checks;
it has no deployment or write permissions.

The separate GitHub profile README is not managed here. Its prerequisite
repository, `zecloud/zecloud`, does not currently exist; creating it is outside
the scope of this site change.

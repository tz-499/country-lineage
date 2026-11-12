# Country Lineage

A node-and-edge graph of who references whom in country music: name-drops, covers,
tributes, duets and influences. Each artist is a node; each reference is a directed
edge from the artist making the reference to the artist being referenced.

## Layout

| Path | What it is |
| --- | --- |
| `index.html` | The whole app: markup, styles and script. |
| `data/graph.json` | The published graph: `artists` and `refs`. |
| `vendor/d3.min.js` | d3 7.9.0, bundled so the site has no CDN dependency. |

There is no build step. It is a static site.

## Run it locally

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>. It has to be served over HTTP; opening `index.html`
straight from disk blocks the page from loading `data/graph.json`.

## Publish on GitHub Pages

Repository **Settings → Pages → Build and deployment**: set Source to
"Deploy from a branch", pick `main` and `/ (root)`. Every push to `main` redeploys.

## How data is saved

`data/graph.json` is the source of truth. Edits made in the browser are kept in that
browser's local storage and the page shows a notice while they are unpublished. To
publish them:

1. Click **Export data** to download `graph.json`.
2. Replace `data/graph.json` with it, commit and push.

**Import data** loads a previously exported file.

### Data format

```json
{
  "version": 1,
  "artists": { "buck-owens": { "name": "Buck Owens" } },
  "refs": {
    "s01": {
      "from": "dwight-yoakam", "to": "buck-owens", "kind": "duet",
      "title": "Streets of Bakersfield", "year": 1988,
      "note": "optional", "createdAt": 1001
    }
  }
}
```

`kind` is one of `namedrop`, `cover`, `tribute`, `duet`, `influence`. Entries with
`"starter": true` were seeded as examples and have not been verified.

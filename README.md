# Why Meta Suck — The Meta Algorithm Explained

Standalone static site for the Meta algorithm visualiser, previously shipped as part of the
`lumos-tools` WordPress plugin. Deployed via GitHub Pages at
https://whymetasuck.nathanknowsnothing.co.uk

## Structure

- `index.html` — entry point; mounts the app at `#lumos-metavis-root` and sets
  `window.lumosTools.url` (the base URL the bundle uses to resolve assets).
- `index-CDf_bJKv.js` — pre-built React bundle (styles are inlined).
- `tools/metavis/assets/` — runtime assets; the bundle loads the logo from this path,
  so the directory layout must be preserved.

## Updating the visualiser

Build the app in its source repo, then replace the `index-*.js` file here and update the
`<script src>` in `index.html` to match the new filename.

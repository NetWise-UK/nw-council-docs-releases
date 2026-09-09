# NW CouncilDocs — releases

Built release zips and update metadata for the **NW CouncilDocs** WordPress
plugin, published by NetWise UK.

**There is no source code here, and there never will be.** The plugin's source
repository is private. This repository exists only so that client sites can see
and download an update *without a credential*.

## Why the split

A release on a private repository can only be downloaded with a token, and a
token that has to reach several hundred client sites is not a secret. The plugin
shipped one until September 2026; it was revoked in August 2026 and every site's
update check silently returned 404 from then until it was fixed.

So the source stays private and only built zips go public. This mirrors
`NetWise-V3-releases` and `nw-event-manager-releases` — one pattern across the
estate, so a failure looks the same everywhere and gets diagnosed once.

## What sites poll

`nw-councildocs.json` on `main`, served by `raw.githubusercontent.com`:

```json
{
  "slug": "nw-councildocs",
  "version": "1.0.10",
  "details_url": "https://github.com/NetWise-UK/nw-council-docs-releases/releases/tag/v1.0.10",
  "download_url": "https://github.com/NetWise-UK/nw-council-docs-releases/releases/download/v1.0.10/nw-councildocs.zip"
}
```

It is a static file on a CDN, deliberately not `api.github.com` — the GitHub API
allows 60 unauthenticated requests an hour counted *per IP*, shared by every site
on a hosting box and by every update checker on them, and exhausting it fails
silently with wp-admin reporting that everything is up to date.

**Do not hand-edit that file.** It is rewritten by the release workflow in the
private repo every time a version tag is pushed, and an edit here is overwritten
by the next release.

## The zip

`nw-councildocs.zip` contains a single top-level `nw-councildocs/` folder — the
folder the plugin installs into, which is *not* the same as the repository name.
WordPress names the installed directory after the folder inside the archive, so
getting that wrong installs a second copy of the plugin alongside the first.

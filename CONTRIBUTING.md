# Contributing a plugin or theme

Thanks for building something for SwiftyEdit! Here's how to get it listed
in the registry.

## 1. Requirements

- Your plugin/theme has its own public GitHub repository
- It includes an `info.json` manifest as required by SwiftyEdit's
  plugin/theme installer, with a `versions` array (each entry carrying
  `version`, `build`, `requires_build`, and `download_url`) — this is the
  single source of truth for what gets installed, see "How versioning
  works" below
- The `download_url` of its most recent compatible version is itself
  GitHub-hosted (a Release asset, `raw.githubusercontent.com`, or
  `codeload.github.com`) — this lets anyone inspect the source before
  installing. Plugins/themes hosted elsewhere are still valid SwiftyEdit
  installs via the existing URL-based installer, they just won't be
  listed here

## 2. Add your entry

Fork this repository and add a new file:

- Plugins: `plugins/{your-slug}.json`
- Themes: `themes/{your-slug}.json`

The slug should be lowercase, hyphen-separated, and match your plugin's
identifier (e.g. `rabbit-editor`, `former`).

### Entry format

```json
{
    "slug": "your-plugin-slug",
    "name": "Your Plugin Name",
    "type": "plugin",
    "author": "your-github-username",
    "repo": "https://github.com/your-username/your-plugin-repo",
    "description": "One or two sentences describing what it does.",
    "tags": ["tag-one", "tag-two"],
    "screenshots": [
        "https://raw.githubusercontent.com/your-username/your-plugin-repo/main/screenshots/1.png"
    ]
}
```

| Field | Required | Notes |
|---|---|---|
| `slug` | yes | Unique, lowercase, hyphen-separated |
| `name` | yes | Display name |
| `type` | yes | `plugin` or `theme` |
| `author` | yes | Your GitHub username |
| `repo` | yes | Public GitHub repo URL — this is also where the catalog service reads your `info.json` from on every sync |
| `description` | yes | Short, plain text |
| `tags` | no | Lowercase, used for filtering in the catalog |
| `screenshots` | no | Up to 6 https URLs, PNG/JPEG/WebP, max 3 MB each |

That's the whole entry — no version, build, or download link lives here.

### How versioning works

Version, build, `requires_build`, and the actual `download_url` are never
stored in the registry entry itself. On every sync, the catalog service
reads your `info.json` directly from your repo's root (via the GitHub
Contents API, so it always sees the current default branch — no `branch`
field needed here) and picks the entry in your `versions` array with the
highest `build` number whose `download_url` is GitHub-hosted.

This means shipping a new release never touches this registry: just
update your own `info.json` (add a new `versions[]` entry, same as you'd
do for the existing "Install from URL" / self-update mechanism) and the
catalog picks it up automatically. If your `info.json` is missing, has no
`versions` array, or its latest `download_url` isn't GitHub-hosted, your
entry is skipped during sync rather than shown with broken data.

### About screenshots

Screenshot URLs must point to publicly accessible https images (raw
GitHub content links work well). They're downloaded, validated, resized,
and cached by the catalog service on sync — so they don't need to be
pre-optimized, but they do need to be reachable at merge time.

## 3. Open a pull request

Submit a PR with just your new JSON file. Please don't bundle multiple
plugin/theme submissions or unrelated changes in one PR.

## 4. Review

A maintainer will check that your entry is valid and your repo (and its
`info.json`) is reachable, then merge it. Once merged, it typically
appears in the catalog within a few minutes (sync is triggered
automatically on merge).

## Updating an existing entry

Only needed for metadata changes — a new name, description, tags, or
screenshots. Open a PR editing your existing `{slug}.json` file. Shipping
a new *version* of your plugin/theme needs no registry change at all;
just update your own `info.json`'s `versions` array and the next sync
picks it up.

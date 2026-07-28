# Contributing a plugin or theme

Thanks for building something for SwiftyEdit! Here's how to get it listed
in the registry.

## 1. Requirements

- Your plugin/theme has its own public GitHub repository
- It includes an `info.json` manifest as required by SwiftyEdit's
  plugin/theme installer
- It's compatible with the SwiftyEdit version you declare below

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
    "download_url": "https://github.com/your-username/your-plugin-repo/releases/download/v1.0.0/your-plugin-slug-1.0.0.zip",
    "description": "One or two sentences describing what it does.",
    "min_swiftyedit_version": "1.4.0",
    "latest_version": "1.0.0",
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
| `repo` | yes | Public GitHub repo URL (for reference/documentation) |
| `download_url` | yes | Direct, versioned link to the installable `.zip`. Must point to the exact build for `latest_version` — update this together with `latest_version` on every release |
| `description` | yes | Short, plain text |
| `min_swiftyedit_version` | yes | Semver |
| `latest_version` | yes | Semver, must match your repo's latest release/tag |
| `tags` | no | Lowercase, used for filtering in the catalog |
| `screenshots` | no | Up to 6 https URLs, PNG/JPEG/WebP, max 3 MB each |

### About `download_url`

This must be a direct, versioned link to a `.zip` file — not the repo
page itself. The easiest way is to attach a `.zip` as an asset to a
[GitHub Release](https://docs.github.com/en/repositories/releasing-projects-on-github)
and link that asset directly. This gives you full control over what's
inside the package (e.g. excluding dev files, bundling vendored
dependencies) and avoids relying on GitHub's tag-name conventions for
auto-generated archive links.

Whenever you release a new version, update both `latest_version` and
`download_url` together in the same PR — a mismatch between the two
means users install the wrong version.

### About screenshots

Screenshot URLs must point to publicly accessible https images (raw
GitHub content links work well). They're downloaded, validated, resized,
and cached by the catalog service on sync — so they don't need to be
pre-optimized, but they do need to be reachable at merge time.

## 3. Open a pull request

Submit a PR with just your new JSON file. Please don't bundle multiple
plugin/theme submissions or unrelated changes in one PR.

## 4. Review

A maintainer will check that your entry is valid and your repo is
reachable, then merge it. Once merged, it typically appears in the
catalog within a few minutes (sync is triggered automatically on merge).

## Updating an existing entry

Same process — open a PR editing your existing `{slug}.json` file (e.g.
to bump `latest_version` after a new release).

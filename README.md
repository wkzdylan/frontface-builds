# Frontface builds

Windows builds of **Frontface**, a low-poly conquest shooter, for the Frontface Launcher.

## Play

1. Download **[Frontface-Launcher-win64.zip](https://github.com/wkzdylan/frontface-builds/releases/download/launcher/Frontface-Launcher-win64.zip)**.
2. Unzip it anywhere and run `Frontface Launcher.exe`.
3. Press **Play**. The launcher downloads the newest build, checks it, and starts the game. New builds are installed the same way when they come out.

Builds install to `%LOCALAPPDATA%\Programs\Frontface`, so no admin prompt is needed. Older builds stay installed until you remove them, and the version picker can go back to any of them.
Windows 10 or 11, 64-bit, with the Edge WebView2 Runtime (part of Windows 11).

No install at all: [play in the browser](https://frontface-neon.vercel.app) against bots (Chrome or Edge with WebGPU).

## What is in this repo

| Path | What it is |
|---|---|
| Releases `v<version>` | `Frontface-<version>-win64.zip`: the game, with `GenAssets/` next to `Frontface.exe`. Some also carry `Frontface-Server-<version>-win64.zip`, the dedicated server. |
| Release `launcher` | `Frontface-Launcher-win64.zip`, replaced on every launcher update so its link never changes. |
| `versions.json` | The build list the launcher reads: version, channel (stable or beta), tag, zip name, size, SHA-256, notes. Written by the publish script; don't edit it by hand. |
| `updates.json` | The news on the launcher's Home and Updates pages. Edit it freely. |
| `img/` | Pictures used by `updates.json` (`"src": "name.jpg"`). |

The launcher checks every download against the SHA-256 in `versions.json` before installing it.

## Publishing (from the dev PC)

The launcher's source and tools are in `E:\AI\frontface-launcher`. Both scripts use the signed-in `gh` CLI.

```
python tools/publish_build.py 0.2.0 --notes-file notes.md            # zip E:\AI\frontface\Build\Windows + GenAssets, release v0.2.0
python tools/publish_build.py 0.2.0 --build --server --notes "..."   # build the game first, add the dedicated server
python tools/publish_build.py 0.3.0-beta.1 --channel beta --notes "..."
python tools/publish_launcher.py                                     # rebuild and replace the launcher download
```

### updates.json

```json
{
  "updated": "2026-11-04",
  "hero": { "src": "depot.jpg", "alt": "...", "caption": "..." },
  "pictures": [{ "src": "spawn.jpg", "alt": "...", "caption": "..." }],
  "releases": [
    { "date": "2026-11-04", "label": "Launcher", "title": "...", "summary": "...",
      "entries": [{ "type": "new | improved | fixed | tools", "title": "...", "text": "...", "images": [{ "src": "...", "alt": "...", "caption": "..." }] }] }
  ],
  "inProgress": [{ "title": "...", "status": "building | testing | waiting | planned", "body": "..." }]
}
```

Newest release first. One release per date.

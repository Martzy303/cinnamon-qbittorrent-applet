# qBittorrent Monitor (Cinnamon applet)

Shows the most recently added torrent's name and a progress bar directly in the panel, polling qBittorrent's WebUI API.

## Requirements

- qBittorrent with the **Web UI** enabled (Tools/Options > Web UI > "Web User Interface").
- `curl` installed (present by default on virtually every distro).

## 1. Enable qBittorrent's WebUI

In qBittorrent: **Options > Web UI**
- Check "Web User Interface (Remote control)"
- Set an IP/port (default `8080`)
- Set a username/password, **or**, if qBittorrent and this applet run on the same machine, check "Bypass authentication for clients on localhost" and leave the applet's username/password blank.

## 2. Install the applet

Symlink (recommended, so edits here show up immediately) or copy this folder into Cinnamon's applets directory:

```bash
ln -s /home/user/Work/qbittorrent@martzy ~/.local/share/cinnamon/applets/qbittorrent@martzy
```

## 3. Reload Cinnamon

Press `Ctrl+Alt+Esc`, or run:

```bash
cinnamon --replace &
disown
```

## 4. Activate it

- Open **System Settings > Applets**
- Find "qBittorrent Monitor" under the "Download" tab (or search), click the `+` to enable it
- Right-click the panel (or the panel's applet zone) and choose to add it if it doesn't appear automatically

## 5. Configure it

Right-click the applet in the panel > **Configure...**

- **URL** — the full WebUI base URL, with scheme and no trailing slash, e.g.:
  - `http://127.0.0.1:8081` (local, default port)
  - `https://martzy.priam.usbx.me/qbittorrent` (reverse-proxied domain, path prefix, no port — omit the port entirely when the proxy handles it on 80/443)
- **Username / Password** — leave blank if using the localhost-bypass option above
- **Refresh interval** — how often to poll (seconds)
- **Max label length** — how many characters of the torrent name to show
- **Left-click action** — open the WebUI in a browser, or force an immediate refresh

Right-click also gives you "Refresh now" and "Open WebUI" shortcuts.

## Notes / limitations

- The password is stored in plain text by Cinnamon's xlet settings system (under `~/.config/cinnamon/spices-settings/qbittorrent@martzy/`) — there's no masking or encryption available in the applet settings framework. Prefer the localhost-bypass option, or a qBittorrent account with limited scope, if that's a concern.
- "Latest torrent" = the most recently **added** torrent (`sort=added_on`), not necessarily the one currently downloading fastest.
- If qBittorrent's WebUI isn't reachable, or login fails, the panel shows "Error" — hover over it for the reason.

## Files

- `metadata.json` — applet identity (uuid `qbittorrent@martzy`)
- `settings-schema.json` — drives the Configure dialog
- `applet.js` — polling + rendering logic (uses `curl` under the hood for the qBittorrent WebUI API, so there's no libsoup version dependency)

# Fediverse Notifications (Misskey)

Misskey Ai plugin that connects your account to the CoreTalk hooks WebSocket. Incoming webhook events can show as notifications, toasts, dialogs, or confirm dialogs that open a URL.

The default fediverse portal this stack is built for is **[coretalk.space](https://coretalk.space)**.

Repository: [github.com/coretalkspace/plugin-notifications](https://github.com/coretalkspace/plugin-notifications)

## Installation (external install, Misskey v2023.11.0+)

Misskey can install Ai **plugins** from your site using **external installation**: open a URL on the user’s own instance. See [Creating Plugins](https://misskey-hub.net/en/docs/for-developers/plugin/create-plugin/) and [Distributing Plugins and Themes](https://misskey-hub.net/en/docs/for-developers/publish-on-your-website/).

### What you need

1. **Plugin API URL** — a JSON endpoint that returns `{ "type": "plugin", "data": "…source…" }` with the AiScript source as a string (LF newlines). Each [GitHub release](https://github.com/coretalkspace/plugin-notifications/releases) attaches **`plugin.json`** for that purpose.
2. **SHA-512 (hex)** of that source after normalizing line endings to **LF**. It is printed on the release page (snippet appended by CI) and in the **`install.txt`** / **`SHA512.txt`** assets.
3. **Install URL** on the user’s Misskey host:

```text
https://{HOST}/install-extensions?url={API_URL}&hash={HASH}
```

- `{HOST}` — the user’s Misskey server (e.g. `social.example.com`).
- `{API_URL}` — absolute URL to `plugin.json` (URL-encoded when used in the query).
- `{HASH}` — SHA-512 hex of the LF-normalized plugin source (must match the `data` field Misskey fetches).

Example (values are illustrative):

```html
<a href="https://misskey.example/install-extensions?url=https%3A%2F%2Fgithub.com%2Fcoretalkspace%2Fplugin-notifications%2Freleases%2Fdownload%2Fv0.0.5%2Fplugin.json&amp;hash=YOUR_SHA512_FROM_RELEASE">
  Install plugin
</a>
```

Use the **exact** `plugin.json` URL and **hash** from the [release](https://github.com/coretalkspace/plugin-notifications/releases/latest) you are installing, or use **`releases/latest/download/plugin.json`** only together with the **current** hash from that same release.

### Raw script (manual / older flows)

You can still use the raw AiScript file: [`notifications.aiscript` on `main`](https://raw.githubusercontent.com/coretalkspace/plugin-notifications/main/notifications.aiscript).

## Requirements

- The **hooks** Cloudflare Worker deployed for your portal so WebSockets are available at `wss://hooks.<your-portal-host>/ws/<username>` (e.g. `hooks.coretalk.space` for [coretalk.space](https://coretalk.space)).
- Webhook sources must POST allowed payloads to that worker; this plugin only consumes messages over the WebSocket.

## License

[MIT](LICENSE) — CoreTalk Space.

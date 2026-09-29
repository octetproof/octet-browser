# Changelog

All notable changes to Octet Browser are listed here. Each version's files are on the [Releases](https://github.com/octetproof/octet-browser/releases) page, and the [docs](https://octetproof.com/docs/browser/) describe every option.

## [1.3.0]

**Breaking:** update the collector and the edge together, from this release. Octet now accepts only sealed reports and only edges with a client certificate for your license.

- Reports from the collector are now encrypted in the browser to an Octet key, so your edge and the network see only ciphertext, and a copy of a report accepted recently is refused.
- Octet accepts only reports from this collector version or later. A 1.2.0 collector gets `400` with `unsealed_bundle`.
- The edge connects to Octet only over mutual TLS, with a client certificate for your license, and refuses to start without one. A 1.2.0 edge gets `401` with `client_cert_required`.
- The collector needs a secure context (https). On a plain http page it now fails at once with a clear error.
- The collector keeps a key for your site in the browser's IndexedDB (database `octet`). Mention it in your privacy notice as needed.
- Get your edge's client certificate yourself in the Octet Portal: open your license, then the **Edge certificates** tab, paste your certificate request and download the certificate. You can hold up to 3 at once, for more hosts or for a renewal overlap.
- You can revoke a single edge certificate in the portal without replacing your license. Octet refuses a revoked certificate within about a minute, with `403 client_cert_revoked`.
- The published collector files are scrambled, which makes them harder to read. The script is about 100 KB, or about 36 KB gzipped, and still needs no `unsafe-eval`.
- The edge now sends `{"t":"hello"}` and `{"t":"done"}` messages on `/v1/ws`. If you run a proxy or your own tooling on that path, let them through.
- Verdicts are harder to influence from the browser.
- Sessions through iCloud Private Relay keep the user's own country, and aren't treated as masked.
- In `full` mode, `ready()` can take up to about 4.5 s on some connections.

## [1.2.0]

First public release.

- The browser collector and the edge service are downloaded from this repository's releases. No package registry or access token is needed.
- Three collection modes: `full`, `lite` and `passive`. No mode asks the user for any permission, and the collector only contacts your edge and Octet's own network hosts.
- Verdicts carry a country, a confidence and an alarm level that works the same way for every country.
- Your backend reads verdicts with read tokens you create in the Octet Portal, and each verdict comes with a signed token your backend can verify offline.
- The edge now reports errors from Octet instead of hiding them.
- Allow `worker-src blob:` in your Content Security Policy. The collector still works without it, but its results are better with it.

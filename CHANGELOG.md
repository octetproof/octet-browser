# Changelog

All notable changes to Octet Browser are listed here. Each version's files are on the [Releases](https://github.com/octetproof/octet-browser/releases) page, and the [docs](https://octetproof.com/docs/browser/) describe every option.

## [1.4.0]

**Action needed:** give your edge a signing key and register it in the Octet Portal when you update the edge. Octet is turning on required edge signatures, and an edge without a registered key then gets `401` with `edge_signature_required`.

- The edge now signs each report it forwards to Octet with its own key, covering the report and what the edge measured. To set it up:
  1. On the edge server, run `openssl rand -base64 32` and put the result in the edge's environment as `EDGE_SIGNING_KEY`. The key never leaves the server.
  2. Start the edge. It logs `S2 signing on: kid=… ed25519 public key=…`.
  3. In the Octet Portal, open your license, then the **Edge certificates** tab, and register the `kid` and public key under **Edge signing key**. You can hold up to 3 keys at once, for a rotation overlap.
- Octet refuses a forward with an invalid signature, or a signature from a key registered for another license, with `401`.
- You can revoke an edge signing key in the portal. Octet refuses it within about a minute, with `401` and `edge_key_unknown`.
- The signing key is separate from the edge's client certificate. Both are needed.
- The edge no longer sends a `User-Agent` of its own when the browser sent none.
- A malformed report now gets `400` with `missing_fields` instead of a server error.
- Fewer wrong countries for some users behind a VPN.

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

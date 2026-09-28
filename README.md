# Octet Browser

Octet Browser tells you which country a browser session is operating in, and how far to trust that answer, without asking the user for any permission.

This repository hosts the release downloads. It contains no source code.

## Get access

Apply at [browser.octetproof.com/signup](https://browser.octetproof.com/signup). Once your request is approved, you receive a licence key and access to the Octet Portal.

## Downloads

Every version is on the [Releases](https://github.com/octetproof/octet-browser/releases) page. Each release contains:

| File | What it is |
|---|---|
| `octet-collector.js` | The browser script, for a `<script>` tag |
| `octet-collector.js.sri` | Its Subresource Integrity hash, for the `integrity` attribute |
| `octet-collector.mjs` | The same script as an ES module |
| `octetproof-collector-<version>.tgz` | The npm package: `npm install <release URL of this file>` |
| `octet-edge-linux-amd64`, `octet-edge-linux-arm64` | The edge service you run on your own infrastructure |
| `SHA256SUMS` | Checksums for every file in the release |
| `SHA256SUMS.sig`, `SHA256SUMS.pem` | The signature over `SHA256SUMS` and its certificate |

Check each download before you use it. First, check that the files match the checksums:

```bash
sha256sum --check --ignore-missing SHA256SUMS
```

Then check that the checksums were signed by this repository's release workflow, using [cosign](https://docs.sigstore.dev/cosign/system_config/installation/):

```bash
cosign verify-blob --signature SHA256SUMS.sig --certificate SHA256SUMS.pem \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  --certificate-identity-regexp '^https://github.com/octetproof/octet-browser/\.github/workflows/sign-release\.yml@' \
  SHA256SUMS
```

## Documentation

Installation, configuration and the verdict reference are at [octetproof.com/docs/browser](https://octetproof.com/docs/browser/).

## Support

Email [developer@octetproof.com](mailto:developer@octetproof.com).

## Licence

Use of Octet Browser is governed by the [Octet Browser licence agreement](https://octetproof.com/terms/browser/). See [LICENSE](LICENSE).

# Changelog

All notable changes to Octet Browser are listed here. Each version's files are on the [Releases](https://github.com/octetproof/octet-browser/releases) page, and the [docs](https://octetproof.com/docs/browser/) describe every option.

## [1.2.0]

First public release.

- The browser collector and the edge service are downloaded from this repository's releases. No package registry or access token is needed.
- Three collection modes: `full`, `lite` and `passive`. No mode asks the user for any permission, and the collector only contacts your edge and Octet's own network hosts.
- Verdicts carry a country, a confidence and an alarm level that works the same way for every country.
- Your backend reads verdicts with read tokens you create in the Octet Portal, and each verdict comes with a signed token your backend can verify offline.
- The edge now reports errors from Octet instead of hiding them.
- Allow `worker-src blob:` in your Content Security Policy. The collector still works without it, but its results are better with it.

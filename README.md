# Passkey Lab

**Live:** https://tblachowicz.github.io/passkey-lab/

A single-page demo of passkeys. Create a passkey, then sign in with it. The page also plays the
relying party's server, so every step of the WebAuthn ceremony is decoded and checked in the browser:

- **Registration:** creation options, `navigator.credentials.create()`, clientDataJSON checks
  (type, challenge, origin), authenticator data (RP ID hash, UP/UV/BE/BS flags, sign counter, AAGUID,
  COSE public key), then storing the credential.
- **Authentication:** a discoverable-credential request (no username), including passkey autofill
  through conditional mediation, the same client-data and authenticator-data checks, and verification of
  the assertion signature over `authenticatorData ‖ SHA-256(clientDataJSON)` with WebCrypto
  (ES256, EdDSA, RS256).
- **Signal API:** removing a saved passkey calls `PublicKeyCredential.signalUnknownCredential()` where
  the browser supports it.

The "server" store is `localStorage`, so passkeys registered here only work in the same browser profile.
It is a demo. A real relying party runs these checks on its own server.

## Run locally

Passkeys need a secure context and a host name (not an IP address):

```
python3 -m http.server 8000
# open http://localhost:8000
```

Passkeys don't work when the page is embedded in a frame from another site (Safari, Firefox and in-app
web views refuse `navigator.credentials.create()` there). Open it as a top-level page.

Follows [W3C Web Authentication Level 3](https://www.w3.org/TR/webauthn-3/).

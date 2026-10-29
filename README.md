# linkding for LazyCat

- Upstream: https://github.com/sissbruecker/linkding
- Package: `community.lazycat.app.linkding`; initial upstream version 1.47.0.
- Target: Linux amd64. Minimum OS: 1.5.0.
- Official upstream image: `sissbruecker/linkding`; mirror delivery via `docker.1ms.run` with matching-digest verification.
- State: `/lzcapp/var/data` mounted at upstream `/etc/linkding/data`; upstream bootstraps SQLite and initial administrator.
- First-use admin credentials are deployment parameters. Password is random; no default or hardcoded production credentials.
- Browser autofill targets upstream `/login/` with Django's `#id_username` and `#id_password` inputs.
- No public-path bypass and no file-chooser interceptor. Official-store publication remains disabled.

Existing administrators are not overwritten on restart. If the password is changed in the application, update its deployment parameter so autofill uses the new value. No automatic password reset is performed.

The cloud smoke test checks health, unauthenticated redirect, actual CSRF-protected login, bookmark creation via authenticated session API, and persistence after restarting a volume-backed container. Community publication starts disabled until tests and LPK validation pass.

These checks do not test a real LazyCat installation or autofill inside the LazyCat gateway. Browser extensions and external clients must also satisfy that gateway's authentication; an upstream API token alone does not bypass it.

Organization secrets `APPSTORE_URL`, `APPSTORE_TOKEN`, and optional `PRIVATE_STORE_GROUP_CODES` are passed explicitly by reference, never committed as values.

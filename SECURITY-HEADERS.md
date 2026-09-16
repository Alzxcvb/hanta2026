# Security headers for hanta2026.org

This site is a single static `index.html` served by **GitHub Pages**, with
`hanta2026.org` pointed through **Cloudflare**.

GitHub Pages does not let you set response headers. It serves whatever is in the
repo and adds a fixed set of its own. So the headers below cannot come from this
repo — they have to be added at the Cloudflare edge.

## What the repo does on its own

`index.html` carries what a static file *can* carry:

- A `<meta http-equiv="Content-Security-Policy">` tag restricting scripts,
  styles, images and network calls to this origin plus the Cloudflare Web
  Analytics beacon.
- A `<meta name="referrer">` tag.
- A JavaScript clickjacking guard that blanks the page and breaks out if the
  site is loaded inside someone else's frame.

Two important limits:

- **`frame-ancestors` is ignored inside a `<meta>` tag.** So is
  `X-Frame-Options`. The JS guard is a workaround, not a replacement — it fails
  against a framing page that uses `<iframe sandbox>` to block top-level
  navigation, and against a visitor with JavaScript disabled.
- **`Strict-Transport-Security` cannot be set from a `<meta>` tag at all.**

Adding the Cloudflare rule below fixes both.

## The proper fix: a Cloudflare Transform Rule

Cloudflare dashboard → select `hanta2026.org` → **Rules** → **Transform Rules**
→ **Modify Response Header** → **Create rule**.

- **Rule name:** `Security headers`
- **If:** *All incoming requests*
- **Then:** add these static headers:

| Header | Value |
| --- | --- |
| `Content-Security-Policy` | `default-src 'self'; base-uri 'self'; object-src 'none'; frame-ancestors 'none'; form-action 'self'; script-src 'self' 'unsafe-inline' https://static.cloudflareinsights.com; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:; connect-src 'self' https://cloudflareinsights.com https://static.cloudflareinsights.com; upgrade-insecure-requests` |
| `X-Frame-Options` | `DENY` |
| `X-Content-Type-Options` | `nosniff` |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=(), payment=(), usb=()` |
| `Strict-Transport-Security` | `max-age=31536000` |

Deploy the rule, then confirm:

```sh
curl -sSI https://hanta2026.org | grep -i -E 'content-security-policy|x-frame-options|strict-transport'
```

Once the `X-Frame-Options: DENY` header is live, the `<meta>` CSP and the JS
guard in `index.html` become redundant belt-and-braces. They are harmless to
leave in place, and they keep the protection working if the site is ever served
from somewhere other than Cloudflare.

## Optional hardening, later

`Strict-Transport-Security` is set to one year with no `includeSubDomains` and
no `preload`, because both are hard to walk back. If every hostname under
`hanta2026.org` is HTTPS-only and you want it permanent, upgrade the value to
`max-age=63072000; includeSubDomains; preload` and submit the domain at
<https://hstspreload.org>.

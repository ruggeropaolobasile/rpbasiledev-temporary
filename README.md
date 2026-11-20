# rpbasiledev.com — temporary static holding page

Clean, self-contained maintenance page. No WordPress code, third-party scripts, secrets, dependencies or legacy server assets.

Render Static Site configuration: `publishPath=public`, `buildCommand=echo ready`.

DNS safety: MX currently points to the apex (`0 rpbasiledev.com`), which resolves to `131.226.5.6`. Changing the apex A alone can interrupt mail. First associate `www.rpbasiledev.com` in Render and update only its CNAME. Resolve email independently before changing apex A or nameservers.

Do not reuse any files from suspended THCServers hosting until vetted.

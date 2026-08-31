# wecanfixeverything.com

A Cloudflare Worker serving one static page for `wecanfixeverything.com`,
pointing at [omarchy.org](https://omarchy.org):

> we can fix
> \- ~~some things~~
> \+ **everything**
>
> every missing app, every incompatibility, every paper cut.

Colors and type are lifted from omarchy.org's `root.css` (Tokyo Night,
JetBrains Mono). The slogan is set as a diff, because that's how you fix
things around here.

## Deploying

```sh
npx wrangler deploy
```

Routes `wecanfixeverything.com/*` and `*.wecanfixeverything.com/*` are declared
in `wrangler.jsonc`. DNS is two proxied `A` records pointing at `192.0.2.1` — a
placeholder, since the Worker intercepts at the edge and no origin is ever
contacted.

Unmatched paths serve the same page rather than a 404, so mistyped deep links
still land on the slogan.

## The social card

`public/card.png` (1200×630, for Open Graph / X cards) is a headless Chromium
screenshot of `card.html`, a fixed-size variant of the page:

```sh
chromium --headless --window-size=1200,630 --hide-scrollbars \
  --virtual-time-budget=5000 --screenshot=public/card.png card.html
```

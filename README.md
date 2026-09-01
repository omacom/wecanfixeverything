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

## The theme song

The page autoplays "We Can Fix Everything" by Ryan R. Hughes, streamed from the
Omarchy station rather than vendored into the repo:

```
https://radio.cliamp.stream/omarchy/tracks/0a7974d4554686ef
```

Track ids come from `https://radio.cliamp.stream/omarchy/tracks`. That endpoint
sends `Access-Control-Allow-Origin: *` and `Accept-Ranges: bytes`, so a plain
cross-origin `<audio>` element can stream and seek it.

Browsers refuse audible autoplay until the visitor has interacted with the page,
so `play()` is attempted on load and, when it is refused, retried on the first
click, keypress or tap. The toggle sits in the bottom-right corner, dim until
you hover it (playing, then paused):

![The music toggle in its two states](docs/music-toggle.png)

It stays hidden until the script unhides it, so with JavaScript off nobody sees
a dead control. The track does not loop: when it ends, the toggle goes back to
play.

## The social card

`public/card.png` (1200×630, for Open Graph / X cards) is a headless Chromium
screenshot of `card.html`, a fixed-size variant of the page:

```sh
chromium --headless --window-size=1200,630 --hide-scrollbars \
  --virtual-time-budget=5000 --screenshot=public/card.png card.html
```

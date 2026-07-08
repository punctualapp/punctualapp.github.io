# Punctual — logo & favicon assets

Files:
- punctual-mark.svg — primary mark (light backgrounds)
- punctual-mark-dark.svg — mark for dark backgrounds
- favicon.svg — favicon, auto-adapts to dark mode via prefers-color-scheme
- favicon-32.png / favicon-64.png — raster fallbacks
- favicon-512.png — PWA / og image base
- apple-touch-icon.png — 180x180, gradient tile

HTML head:
```html
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
<link rel="icon" href="/favicon-32.png" sizes="32x32" type="image/png">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
```

# serebano.com

Personal website and company presence for **Sergiu Toderascu** — software engineer
building developer tooling and AI engineering systems from Chisinau, Moldova.

This site doubles as the public company website for the registered legal entity
**SERGIU TODERASCU, AI** (D-U-N-S® 933919838), published for verification purposes —
including Apple Developer Program enrollment (individual → company account conversion).

## Tech

A single, dependency-free static `index.html`. No build step, no frameworks.
Dark/light theme (system + manual toggle), print styles, Open Graph image,
JSON-LD, `robots.txt`, and `security.txt`. Deployable on GitHub Pages, Vercel,
or any static host.

## Content

The site content is derived from the public GitHub profile at
[github.com/serebano](https://github.com/serebano).

## Local preview

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy

GitHub Pages with a custom domain (`CNAME` → `serebano.com`), or `vercel deploy`.
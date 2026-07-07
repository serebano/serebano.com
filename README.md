# serebano.com

Personal website and company presence for **Sergiu Toderascu** — software engineer
building mobile apps and developer tooling from Chisinau, Moldova.

This site doubles as the public company website for the registered legal entity
**SERGIU TODERASCU, II** (D-U-N-S® 933919838), published for verification purposes —
including Apple Developer Program enrollment (individual → company account conversion).

## Tech

A single, dependency-free static `index.html`. No build step, no frameworks.
Deployable on GitHub Pages, Vercel, or any static host.

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
# martechmatthew.com

Static personal site for Matthew Stein, hosted on GitHub Pages.

## Setup

1. In the repo **Settings → Pages**, set source to **Deploy from branch: main / (root)**.
2. Add a custom domain `martechmatthew.com` (the `CNAME` file is already included).
3. Point your DNS A/CNAME records to GitHub Pages (see GitHub repo Settings → Pages).

## Local preview

```bash
python3 -m http.server 8080
```

Then open http://localhost:8080

# usephalanx/website

> **Private repo** — source files for [usephalanx.com](https://usephalanx.com).

## Contents

| File | Purpose |
|------|---------|
| `index.html` | Landing page |
| `privacy.html` | Privacy policy |
| `terms.html` | Terms of use |
| `robots.txt` | Crawler directives |
| `sitemap.xml` | Sitemap |
| `demo.mp4` | Demo video served at `/demo.mp4` |

## How it's served

Files are served by nginx inside the `phalanx-prod` Docker stack on the production server. nginx mounts this directory as `/usr/share/nginx/html`.

There is no build step — edit the HTML directly and deploy.

## Updating the site

```bash
# 1. Edit files locally (index.html, etc.)

# 2. rsync to server
rsync -az -e "ssh -i ~/work/LightsailDefaultKey-us-west-2.pem -o StrictHostKeyChecking=no" \
  ./ ubuntu@44.233.157.41:/home/ubuntu/phalanx/site/ \
  --exclude '.git' --exclude 'README.md'

# 3. Changes are live immediately (nginx serves static files directly, no restart needed)
#    Hard-refresh browser to bust cache: Cmd+Shift+R
```

## Updating the demo video

Replace `demo.mp4` with a new recording and rsync it up:

```bash
# Record a new demo, save as demo.mp4
rsync -az -e "ssh -i ~/work/LightsailDefaultKey-us-west-2.pem" \
  demo.mp4 ubuntu@44.233.157.41:/home/ubuntu/phalanx/site/demo.mp4
```

The video autoplays muted in a loop on the landing page.

## Pushing changes to this repo

```bash
cd /tmp/phalanx-website   # or wherever you cloned it
git add -A
git commit -m "update: <what changed>"
git push origin main
```

> This repo is private. Do not make it public — the main OSS landing lives in the repo README at [usephalanx/phalanx](https://github.com/usephalanx/phalanx).

# pulse-landing

Static landing page for Pulse, built from the design in
[`pulse-design`](https://github.com/scarecrowgit/pulse-design) (`workspace/Landing.dc.html`,
violet theme) and served by unprivileged nginx in Docker.

## Run

```sh
docker compose up --build        # http://localhost:8080
# or
docker build -t pulse-landing . && docker run --rm -p 8080:8080 pulse-landing
```

Change the host port with `LANDING_PORT=3000 docker compose up`. A health endpoint is available at `/healthz`.

## Layout

| Path | What |
|---|---|
| `public/index.html` | The page: plain HTML, inline styles, SEO meta and JSON-LD from the design |
| `public/assets/bundle.css` | Pulse design-system CSS (copied from `pulse-design/workspace/ds/pulse/components/`) |
| `public/404.html`, `favicon.svg`, `robots.txt`, `sitemap.xml`, `llms.txt` | Supporting files |
| `nginx.conf` | Server config: gzip, caching, security headers, 404 page, `/healthz` |
| `Dockerfile`, `docker-compose.yml` | Image (`nginxinc/nginx-unprivileged`, port 8080) and compose service |

## Notes

- The design source is a `.dc.html` canvas artboard. It was converted to a standalone page: the canvas wrapper and
  runtime script were removed and a viewport meta tag, favicon and theme colour were added. Content is unchanged.
- Links such as `/signup`, `/login`, `/docs`, `/demo` and `/privacy` point at routes served by the product, not by this
  site. Route them to the app at your reverse proxy, or replace them with absolute URLs.
- Geist fonts load from Google Fonts via `bundle.css`; the CSP in `nginx.conf` allows only that origin.
- Canonical/OG URLs use `https://pulse.app/` as in the design; update them if the production domain differs.

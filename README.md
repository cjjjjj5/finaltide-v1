# FinalTide

A single-page site for showing photos and videos of ocean activities — surf, dive, sail, freedive, storm swell — built as a static site so it deploys straight to Cloudflare Pages.

## What's in here

```
finaltide/
├── index.html   ← the whole site (HTML + CSS + JS in one file)
├── _headers     ← Cloudflare Pages security/cache headers
└── README.md
```

There's no build step. It's plain HTML/CSS/JS, so Cloudflare Pages can serve it as-is.

## Add your own photos and videos

Right now the gallery ("The Log") and the reels strip use colored placeholder tiles so you can see the layout, filtering, and lightbox working. Swap in real media like this:

1. Create an `assets/photos/` and `assets/videos/` folder next to `index.html`.
2. **For a photo card**, find its `<button class="card ...">` block in `index.html` and replace the `.card__media` div's contents with an image:
   ```html
   <div class="card__media">
     <img src="assets/photos/dawn-patrol.jpg" alt="Dawn patrol surf session" style="width:100%;height:100%;object-fit:cover;">
   </div>
   ```
3. **For a reel**, replace a `.reel` button's background with a real video:
   ```html
   <button class="reel" data-title="Barrel, North Point" data-meta="0:18 · Surf">
     <video muted loop playsinline autoplay poster="assets/videos/barrel-poster.jpg" style="width:100%;height:100%;object-fit:cover;">
       <source src="assets/videos/barrel.mp4" type="video/mp4">
     </video>
   </button>
   ```
4. Keep the `data-filter="surf|dive|sail|freedive|storm"` and `data-type="photo|video"` attributes on each card — that's what the filter tabs use.
5. Add or remove `<button class="card ...">` blocks freely; the grid and filters adjust automatically.

Keep video files reasonably compressed (H.264 MP4, under a few MB where possible) — Cloudflare Pages has a 25 MB per-file limit on the free tier.

## Deploy to Cloudflare Pages

**Option A — Dashboard (fastest, no CLI):**
1. Go to the Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Upload assets**.
2. Give the project a name (e.g. `finaltide`).
3. Drag in the whole `finaltide` folder (or a zip of it).
4. Click **Deploy**. You'll get a `finaltide.pages.dev` URL immediately.
5. Add a custom domain later under the project's **Custom domains** tab if you own one.

**Option B — Wrangler CLI:**
```bash
npm install -g wrangler
wrangler login
cd finaltide
wrangler pages deploy . --project-name=finaltide
```

**Option C — Git integration (auto-deploys on push):**
1. Push this folder to a GitHub/GitLab repo.
2. In the Cloudflare dashboard: **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Pick the repo. Leave the build command empty and set the output directory to `/` (root), since there's no build step.
4. Deploy — every future push updates the live site automatically.

## Customizing

- **Colors, fonts, copy** are all in the `<style>` block and the section markup at the top of `index.html` — search for `--abyss`, `--tide`, `--flare` in `:root` to change the palette.
- **Nav links / section order** can be reordered by moving the `<section>` blocks.
- **Stats in the About section** ("190+ Sessions Logged" etc.) are placeholder — update them to your real numbers.
- Replace `hello@finaltide.example` in the footer with your real contact email, and the social links with your real profiles.

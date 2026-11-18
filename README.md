# AnyMuse — Website

Production website for **AnyMuse**, a creative, production & marketing agency (est. 2020).
Built with Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS v4, GSAP and Motion.

> Design philosophy: **Simple to experience. Extraordinary to explore.**

---

## Quick start

```bash
npm install
cp .env.example .env.local     # fill in values (see DEPLOY.md)
npm run dev                    # http://localhost:3000
```

Production:

```bash
npm run build
npm start
```

---

## What's inside

| Route | Page |
|---|---|
| `/` | Homepage — cinematic hero, intro, interactive service selector, selected work, why AnyMuse, process, industries |
| `/about` | Story, evolution timeline, what we do, mission, values, approach, team |
| `/services` | All six divisions |
| `/services/[slug]` | Service detail (overview, deliverables, outcomes, related work) |
| `/work` | Filterable portfolio |
| `/work/[slug]` | Project detail (image or video, description, deliverables) |
| `/industries` | Sector-by-sector problems and solutions |
| `/contact` | Project inquiry form (real backend), email, WhatsApp |
| `/api/inquiry` | Form endpoint — validation, Supabase storage + DB, Resend email |
| `/sitemap.xml`, `/robots.txt`, `/manifest.webmanifest` | Generated |

---

## Editing content (no code knowledge needed)

All copy and data live in **`src/content/`** — presentation is separate from content.

| File | What it controls |
|---|---|
| `site.ts` | Company name, taglines, email, phone, WhatsApp number, location, social links, navigation |
| `services.ts` | The six divisions — titles, descriptions, deliverables, highlights, notes |
| `projects.ts` | Portfolio projects (`isDemo: true` marks demonstration entries) |
| `company.ts` | Team, values, approach/process steps, timeline, "why AnyMuse", industries |

### Adding a real project
1. Put the thumbnail in `public/work/<slug>/` (JPG/WebP, ≥1600px wide; keep under ~400 KB).
2. Add an entry to `src/content/projects.ts`:
   ```ts
   {
     slug: "my-project",
     title: "Project title",
     category: "Animation",          // one of the categories listed at the top of the file
     tags: ["2D animation", "Explainer"],
     client: "Client name" | null,    // null if not permitted
     year: "2025",
     summary: "One sentence.",
     description: ["Paragraph 1", "Paragraph 2"],
     deliverables: ["…"],
     thumbnail: "/work/my-project/cover.jpg",
     gallery: ["/work/my-project/01.jpg"],           // optional
     video: { embed: "https://www.youtube.com/embed/XXXX" }, // optional; or { src: "/work/my-project/film.mp4" }
     aspect: "landscape",            // wide | landscape | portrait | square — controls grid composition
     isDemo: false,
     featured: true,                 // shows on the homepage (first 4 featured are used)
   }
   ```
3. Delete the demo entries when you have 4+ real projects.

Video never autoplays with audio; the player uses native controls (or the YouTube/Vimeo embed).

### Team photos
Replace `public/team/<name>.svg` with photos (4:5 ratio, e.g. 800×1000) and update `avatar` paths in `company.ts`.

### Logo
The header/footer logo is typographic (so it always uses the brand typeface). To use your official logo file instead, drop it in `public/` and swap the `<Logo />` component in `src/components/ui/index.tsx` for an `<img>`.

---

## Design system

- **Colours** (`src/app/globals.css` → `@theme`): `ink` #0b0b0c, `paper` #fff, `bone` #f3f1ec, `ash`, `mist`, `smoke`, `slate`, `red` #c8202f.
- **Type**: Archivo Variable (display, width axis), Inter Variable (body), Instrument Serif italic (accent). Self-hosted via Fontsource — no external font requests.
- **Motion**: GSAP (hero choreography, scroll reveals, SplitText, timelines) + Motion (menu, page transitions, service selector, portfolio filtering). Everything respects `prefers-reduced-motion`.
- **Utilities**: `font-display`, `text-display-xl/lg/md/sm`, `eyebrow`, `container-x`, `section-y`, `grid-12`, `.btn`, `.chip`, `.field`.

### Motion primitives (`src/components/motion/`)
- `<Reveal>` — scroll-triggered fade/lift (optionally staggers children)
- `<SplitHeading>` — masked line / word / character reveals
- `<MaskImage>` — image wipe reveal with subtle parallax

---

## Project structure

```
src/
  app/                 routes, layout, metadata, sitemap, robots, API
  components/
    layout/            Header (full-screen menu), Footer, PageHero
    home/              Hero, ServiceSelector, ServiceVisual, Sections, ProcessLine
    work/              ProjectCard, WorkGrid
    about/             Timeline
    industries/        IndustryNav
    contact/           InquiryForm
    motion/            Reveal, SplitHeading, MaskImage
    ui/                Button, Logo, Eyebrow, SectionHeader, icons
  content/             ALL editable content
  lib/                 gsap setup, inquiry schema, utils
public/                static assets, demo visuals, icons, OG image
scripts/               generate-demo-visuals.mjs
supabase/schema.sql    database + storage setup
```

---

## Scripts

| Command | Purpose |
|---|---|
| `npm run dev` | Development server |
| `npm run build` | Production build (also type-checks) |
| `npm start` | Serve the production build |
| `npm run typecheck` | TypeScript only |
| `node scripts/generate-demo-visuals.mjs` | Regenerate placeholder visuals/icons |

See **DEPLOY.md** for hosting, domain and backend setup, and **NEEDS-FROM-YOU.md** for the information still required before launch.

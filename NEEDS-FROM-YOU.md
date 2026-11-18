# What I still need from you

Everything below is clearly placeholdered in the code so the site is honest until you supply it.
Search the project for `[[PLACEHOLDER]]` and `[[CONFIRM]]` to find each spot.

## Must have before launch

| # | Item | Where to put it |
|---|---|---|
| 1 | **Contact email** (currently `hello@anymuse.example.com`) | `src/content/site.ts` → `contact.email`, and `INQUIRY_NOTIFY_EMAIL` env var |
| 2 | **Phone number** (currently `+91 [[PHONE NUMBER]]`) | `src/content/site.ts` → `contact.phoneDisplay` |
| 3 | **WhatsApp number** (digits with country code, e.g. `919876543210`) — button is hidden until set | `src/content/site.ts` → `contact.whatsapp` |
| 4 | **Studio location** — "New Delhi, India" is assumed | `src/content/site.ts` → `contact.location` |
| 5 | **Final domain** | `NEXT_PUBLIC_SITE_URL` env var |
| 6 | **Real portfolio projects** — thumbnails, descriptions, deliverables, client names where permitted, video links. All 10 current projects are labelled *Demo* and must be replaced. | `src/content/projects.ts`, `public/work/` |
| 7 | **Supabase + Resend credentials** (see DEPLOY.md) | Vercel environment variables |

## Should confirm

| # | Item | Notes |
|---|---|---|
| 8 | **The five review steps** — your five-step review process is real, but the original site never named the steps. I used a working structure (Understand → Concept → Create → Review → Deliver) and described the review as an internal five-step check. Rename/rewrite freely. | `src/content/company.ts` → `process` |
| 9 | **Timeline milestones** — only 2020 is a confirmed date; other entries are undated phases drawn from your story. Add years if you want them. | `src/content/company.ts` → `timeline` |
| 10 | **Team** — six members as on the old site. The old site said "seven people"; I did not invent a seventh. Confirm current roles/titles; supply photos if you want to replace the monogram tiles. | `src/content/company.ts` → `team`, `public/team/` |
| 11 | **Industries** — ten sectors proposed from your brief. Remove any you don't actually want to target. | `src/content/company.ts` → `industries` |
| 12 | **Budget bands** in the inquiry form (₹ ranges are my suggestion) | `src/lib/inquiry.ts` → `BUDGET_OPTIONS` |
| 13 | **Printing partners statement** — the site says print/fabrication is produced through production partners managed by AnyMuse. Confirm wording. | `src/content/services.ts` → printing `note` |
| 14 | **Marketing scope** — the site says you do creative strategy + production, not media buying or performance guarantees. Confirm. | `src/content/services.ts` → marketing `note` |
| 15 | **Social links** (Instagram, LinkedIn, YouTube, Behance) — hidden until filled | `src/content/site.ts` → `social` |
| 16 | **Official logo file** — the site currently renders a typographic wordmark with red slashes in the spirit of your logo. If you want the exact original artwork, send the SVG/PNG. | `public/logo.svg`, `src/components/ui/index.tsx` |

## Nice to have

- A real **brand film or showreel** (30–60s, MP4 or YouTube link) for the homepage hero or the Work page.
- **Testimonials** — none are shown because none were supplied (and none were invented). Add real ones and I can add a quiet testimonial section.
- **Case-study depth** for 3–4 flagship projects (problem → approach → result) to strengthen the portfolio.
- **Privacy policy / terms** pages if you collect inquiries at scale (short pages; link from the footer).

# Deployment guide

The site is a standard Next.js 15 app. The recommended host is **Vercel** (zero-config, free tier is enough to start). Netlify, Cloudflare Pages (with the Next adapter), Railway or any Node host also work.

## 1. Accounts you need

| Service | Why | Cost |
|---|---|---|
| **GitHub** (or GitLab/Bitbucket) | Source code hosting; Vercel deploys from it | Free |
| **Vercel** | Hosting, HTTPS, CDN, preview deployments | Free (Hobby) → Pro if you need team seats |
| **Supabase** | Stores form submissions + uploaded reference files | Free tier (500 MB DB, 1 GB storage) |
| **Resend** | Sends inquiry notification emails + acknowledgement to the sender | Free tier (3,000 emails/month) |
| **Domain registrar** | Your custom domain (e.g. anymuse.in) | ~₹800–1,500/yr |

The form works with *either* Supabase or Resend configured. With both you get storage and email. With neither, the form shows an honest "not set up yet" error with your email/WhatsApp as fallback — it never fakes success.

## 2. Push the code

```bash
git init
git add .
git commit -m "AnyMuse website"
# create an empty repo on GitHub, then:
git remote add origin git@github.com:<you>/anymuse-website.git
git push -u origin main
```

## 3. Supabase (database + file storage)

1. Create a project at supabase.com → note the **Project URL** and the **service_role** key (Project Settings → API).
2. Open **SQL Editor → New query**, paste the contents of `supabase/schema.sql`, run it.
   This creates the `inquiries` table (RLS enabled, no public access) and a private `inquiry-files` bucket.
3. You can view submissions in **Table Editor → inquiries**. Files are under **Storage → inquiry-files**.

> The service_role key is only used server-side in `/api/inquiry`. Never prefix it with `NEXT_PUBLIC_`.

## 4. Resend (email)

1. Create an account at resend.com → **API Keys → Create**.
2. **Domains → Add domain** (e.g. `anymuse.in`) and add the DNS records it shows (SPF/DKIM) at your registrar. Until verified, you can only send from `onboarding@resend.dev` and only to your own account email — fine for testing.
3. Set `INQUIRY_FROM_EMAIL` to an address on the verified domain, e.g. `AnyMuse Website <noreply@anymuse.in>`.

## 5. Environment variables

Copy `.env.example` → `.env.local` locally, and add the same keys in **Vercel → Project → Settings → Environment Variables**:

```
NEXT_PUBLIC_SITE_URL=https://www.anymuse.in
SUPABASE_URL=https://xxxx.supabase.co
SUPABASE_SERVICE_ROLE_KEY=eyJ...
RESEND_API_KEY=re_...
INQUIRY_NOTIFY_EMAIL=hello@anymuse.in
INQUIRY_FROM_EMAIL="AnyMuse Website <noreply@anymuse.in>"
```

## 6. Deploy on Vercel

1. vercel.com → **Add New → Project → Import** your repo. Framework is auto-detected (Next.js). Keep defaults.
2. Add the environment variables above, click **Deploy**.
3. Every push to `main` redeploys; every pull request gets a preview URL.

## 7. Custom domain

1. Vercel → Project → **Settings → Domains → Add** `anymuse.in` and `www.anymuse.in`.
2. At your registrar, add the records Vercel shows:
   - `A` record for the apex (`@`) → `76.76.21.21`
   - `CNAME` for `www` → `cname.vercel-dns.com`
3. Set one as the primary (Vercel redirects the other). HTTPS is automatic.
4. Update `NEXT_PUBLIC_SITE_URL` to the final URL and redeploy (this fixes canonical URLs, sitemap and Open Graph links).

## 8. Verify after deploying

- `https://yourdomain/api/inquiry` → should return `{"ok":true,"storage":true,"email":true}`
- Submit a test inquiry → row in Supabase + email in your inbox + acknowledgement to the test address.
- `https://yourdomain/sitemap.xml` and `/robots.txt` load.
- Submit the sitemap in **Google Search Console** (add the domain property, verify via DNS TXT record).
- Run Lighthouse (Chrome DevTools → Lighthouse) on `/` and `/work`.

## 9. Going forward

- **Content changes**: edit files in `src/content/`, commit, push → live in ~1 minute.
- **Images**: add to `public/`, use WebP/JPG, keep hero/thumbnail images ≤ 400 KB.
- **Video**: host on YouTube (unlisted) or Vimeo and use `video.embed`; or upload MP4 (H.264, ≤ 20 MB) to Supabase Storage / Cloudflare R2 and use `video.src`.
- **Inquiry management**: the `status` column in `inquiries` (`new`, `contacted`, `proposal_sent`, `won`, `lost`) is there for a simple pipeline. If you later want a proper admin UI, Supabase's Table Editor works today; a small Next.js admin page can be added behind Supabase Auth.
- **WhatsApp**: set `contact.whatsapp` in `src/content/site.ts` to digits only with country code (e.g. `919876543210`); the button appears automatically.

## Performance notes

- Fonts are self-hosted (no Google Fonts round-trip).
- All demo visuals are SVG (a few KB each). Real photos should be compressed; consider `next/image` once you have raster images.
- Animations are GPU-friendly (transforms, clip-path, opacity), lazy on scroll, and disabled under `prefers-reduced-motion`.
- JS: ~215 KB first load on the homepage (GSAP + Motion + React). Inner pages ~155 KB.

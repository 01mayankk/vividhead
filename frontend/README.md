# VividHead Frontend (Vercel / Next.js)

Next.js 14 App Router client for VividHead, featuring anti-gravity glassmorphism visuals, Framer Motion transitions, and neon mesh overlay rendering.

## Environment Variables

Create `.env.local` in `frontend/`:

```bash
NEXT_PUBLIC_API_URL=http://localhost:7860
```

Use your deployed Hugging Face endpoint in production.

## Local Development

```bash
cd frontend
npm install
npm run dev
```

Open <http://localhost:3000>.

## Vercel Deployment

1. Import `frontend/` as a Vercel project.
2. Add `NEXT_PUBLIC_API_URL` env variable in project settings.
3. Deploy with default Next.js preset.
4. Prefer Edge runtime for lightweight future API routes when added.

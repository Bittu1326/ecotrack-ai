# EcoTrack AI ♻️

**Give your e-waste a second life.** An AI-powered e-waste pickup and recycling coordination app — scan an item, get an instant identification and recycling estimate, choose a local recycler, schedule a doorstep pickup, and track it through to *Recycling Completed*.

## Quick start

```bash
npm install
npm run dev        # → http://localhost:3000
```

Production build:

```bash
npm run build
npm start
```

## The core journey

```
Landing → Dashboard → Dispose E-Waste → Take Photo / Upload / Demo Item
        → AI/Demo Analysis → Select Recycler → Schedule Pickup
        → Confirmation → Track Pickup → Recycling Completed ♻️
```

Fastest way to see it: open the app and click **Try Demo** — it jumps straight into a demo Laptop scan.

## Screens

| Route | Purpose |
|---|---|
| `/` | Landing page with Get Started / Try Demo |
| `/dashboard` | Impact stats, Dispose & Track actions, recent activity |
| `/scanner` | Three input modes: 📷 camera, 📁 upload, 🧩 demo items |
| `/analysis` | AI/demo identification result + battery safety warnings |
| `/recyclers` | Demo recycler cards with ratings and distance |
| `/schedule` | Address / date / time-slot form with validation, generates `ET-####` pickup ID |
| `/confirmation` | Full pickup summary with Track Pickup action |
| `/tracking` | 6-stage timeline with **Advance Demo Status** |

## AI integration (optional)

The app runs fully in **Demo Mode** with no API key — realistic predefined results, clearly badged in the UI. To enable real AI vision, set an OpenAI-compatible key at build time:

```bash
NEXT_PUBLIC_VISION_API_KEY=sk-... npm run build
```

Missing or failing keys never crash the app; they fall back to Demo Mode automatically.

## Camera behavior

Camera access is requested only when the user taps *Enable Camera*, prefers the rear (`environment`) camera on mobile, and degrades gracefully: permission denial, missing devices, and unsupported browsers all show friendly messages while Upload and Demo paths stay fully functional.

## State & persistence

- **localStorage** — active pickup, completed pickups (survives restarts)
- **sessionStorage** — journey draft: item, analysis, recycler (survives accidental refresh; photo data URLs intentionally not persisted)
- **Memory only** — captured/uploaded photo previews (never leave the device)

## Testing

```bash
npm run test:e2e   # 28 Playwright tests, desktop + mobile viewports
npm run lint
npx tsc --noEmit
```

The E2E suite walks the complete journey for all three input paths, camera permission denial, invalid uploads, all six demo items (incl. the battery warning), form validation, and state persistence across refresh and restart. See [TESTING.md](TESTING.md) for the full QA checklist and a 60-second demo script.

> **Note:** Recyclers, ratings, distances, and impact stats are prototype/demo data — not real-world verified services.

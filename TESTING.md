# EcoTrack AI — QA Checklist & Demo Script

Regression checklist for the core journey. Automated E2E coverage: `npx playwright test` (headless Chromium, camera mocked, no camera hardware needed).

## 1. Core journey (all three entry paths)

Path A — Demo item:
1. `/` → **Get Started** → `/dashboard`
2. **Start Scanning** → `/scanner`
3. **Demo Item** tab → **Laptop** → auto-advance to `/analysis`
4. Verify: Demo Mode badge, Laptop, Electronics, Used – Good, High, ₹1,500
5. **Select Recycler →** → `/recyclers` → pick **GreenCycle Recycling**
6. `/schedule` → fill valid address, date, slot → **Confirm Pickup**
7. `/confirmation` → Pickup ID `ET-####`, all details correct
8. **🚚 Track Pickup** → `/tracking`
9. Click **⏭ Advance Demo Status** ×5 → reaches **Recycling Completed ♻️**

Path B — Upload: scanner → **Upload Image** tab → choose JPG/PNG/WEBP → preview shows → **✓ Use Photo** → analysis
Path C — Camera: scanner → **Take a Photo** → Enable Camera → allow permission → preview → **📸 Capture** → preview → **✓ Use Photo** → analysis (or **↺ Retake**)

## 2. Camera fallbacks
- Deny permission → friendly message, upload/demo tabs still work
- No camera device → friendly error, Try Again offered, other tabs work
- `http://` non-localhost → camera blocked, app otherwise fine
- Permission requested **only** after tapping **Enable Camera** (never on page load)

## 3. Upload handling
- JPG / JPEG / PNG / WEBP accepted and previewed
- GIF/other types rejected with message; >8 MB rejected
- Upload → analysis proceeds and result appears

## 4. Demo Mode
- No API key → no crashes, **🧪 Demo Mode (no API key)** badge visible
- All 6 demo items produce realistic results; **Battery** shows the hazardous-materials warning

## 5. Navigation & state
- Every button/link/back/continue/select reaches its target
- Refresh mid-journey → item/analysis/recycler survive (sessionStorage); photo thumbnail gracefully omitted
- Pickup ID, details, and stage survive refresh (localStorage)
- After Recycling Completed, dashboard shows the completed pickup; starting a new journey resets cleanly

## 6. Forms
- Submit empty → address, date, and slot errors; no navigation
- Past date rejected; today accepted
- Confirmation only with valid input

## 7. Commands
```bash
npm run build   # production build
npm run lint    # eslint
npm run test:e2e   # 44 Playwright tests (core journey + extensions)
npm run dev     # run the app
```

## 8. Extension features (Prompt 3)
- **/impact** — Environmental Impact Dashboard: live stats that grow when a pickup completes, category breakdown, Eco Beginner / Green Warrior / Eco Champion levels, prototype-estimate disclaimer
- **/certificate/[pickupId]** — Recycling Certificate with Certificate ID (`ECO-YYYY-####`), completion date, and Print / Save via `window.print()`
- **/pickups** — My Pickups history; opens per-pickup tracking at `/tracking/[pickupId]`
- **/recycler** — Recycler Dashboard: Pending / Today / Collected / Completed tiles + pickup table with status dropdown & one-tap advance; updates sync to user-side tracking instantly (same browser profile, incl. across tabs)

## 8. 60-second hackathon demo script
1. Landing → **Try Demo** (jumps straight into a Laptop scan)
2. Point out the **🧪 Demo Mode** badge and the ₹1,500 estimate
3. **Select Recycler →** → GreenCycle → **Select**
4. Fill address, pick tomorrow, pick a slot → **Confirm Pickup**
5. Show the **ET-####** ID → **Track Pickup**
6. **⏭ Advance Demo Status** through all six stages to **Recycling Completed ♻️**
7. Optional: dashboard → new scan via **Upload Image** or **Take a Photo** (mobile: rear camera)

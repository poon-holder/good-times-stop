# good-times-stop — Project Notes

**Live:** https://good-times-stop.vercel.app/papa-call.html
**GitHub:** https://github.com/poon-holder/good-times-stop
**Source:** `delivery-dashboard/public/papa-call.html`
**Deploy:** `cd delivery-dashboard/public && npx vercel --prod --yes`

---

## Workflow

1. p describes a feature or change via Telegram
2. Rune edits `delivery-dashboard/public/papa-call.html` directly (single-file app)
3. Deploy: `npx vercel --prod --yes` from `delivery-dashboard/public/`
4. p tests on device at `good-times-stop.vercel.app/papa-call.html`
5. Rune updates this notes file and pushes to GitHub

No build step, no framework — plain HTML/CSS/JS. All state in `localStorage`.

---

## What's Built (Current State)

| Feature | Status |
|---------|--------|
| Floating physics bubbles with animated gradient background | ✅ Done |
| Touch drag — hold & drag moves bubble, quick tap fires action | ✅ Done |
| Built-in bubbles: Call Papa (WhatsApp), Call Florence (WhatsApp), Camera, Photo | ✅ Done |
| Settings panel (cog button) | ✅ Done |
| Per-bubble toggle (show/hide) and delete | ✅ Done |
| Add bubble form with function-type dropdown | ✅ Done |
| Function types: WhatsApp call, Show picture (file/camera picker), Embedded video, Open camera, PDF/link | ✅ Done |
| Gravity slider (0–100) per bubble | ✅ Done |
| Random gravity fluctuation toggle per bubble | ✅ Done |
| Picture bubble: stores image as base64, shows as circular photo in bubble | ✅ Done |
| GitHub repo: poon-holder/good-times-stop | ✅ Done |

---

## Core Bubble Visual Identity

Every bubble should be visually self-describing — you can tell what it does just by looking at it. The glass/translucent aesthetic must *envelop* the content inside, not compete with it.

### Picture bubble
- Photo fills the bubble interior — circular, portrait crop
- Glass/translucency wraps around the photo (photo is *inside* the bubble)
- Tap: opens photo full-screen overlay
- **Status:** Base version done. Glass-wrap aesthetic to be refined.

### Call bubble (WhatsApp)
- Option to attach a contact photo as the bubble fill
- Phone/call icon overlaid as a small badge (bottom-right corner) — does not obscure the face
- Contact face stays clearly visible through the glass
- **Status:** Not yet implemented — currently just icon + label

### Video bubble (YouTube)
- Bubble shows YouTube thumbnail: `https://img.youtube.com/vi/{VIDEO_ID}/hqdefault.jpg`
- Tap: fullscreen video overlay pops up, video plays
- Dismiss: overlay closes, user returns to bubble screen
- **Status:** Currently opens URL in new tab. Full overlay not yet built.

#### YouTube embed parameters (all confirmed):
| Param | Value | Reason |
|-------|-------|--------|
| `autoplay` | `0` | Don't autoplay on load; trigger via IFrame API on user tap |
| `mute` | `0` | **Unmuted from the start** |
| `controls` | `0` | No player buttons visible |
| `fs` | `1` | Fullscreen allowed |
| `rel` | `0` | No related videos at end |
| `playsinline` | `1` | **Critical for iOS** — without this Safari breaks the overlay |
| `enablejsapi` | `1` | Required for IFrame API volume control |
| `iv_load_policy` | `3` | No annotation overlays |
| `modestbranding` | `1` | No YouTube logo watermark |
| `disablekb` | `1` | No keyboard shortcuts |
| `origin` | app domain | IFrame API security |

Volume capped at **25%** via `player.setVolume(25)` in `onReady` callback.

---

## Orientation

App is **primarily landscape**. Portrait should work but landscape is the design target. Video overlays especially must be landscape-optimised.

---

## Ideas Moving Forward

### Near-term
- [ ] **Video overlay** — fullscreen YouTube player in-app, closes on end/dismiss, landscape-locked
- [ ] **Call bubble contact photo** — add photo picker to WhatsApp call bubbles, badge icon overlay
- [ ] **Video bubble thumbnail** — extract + display YouTube thumbnail inside the bubble
- [ ] **Glass-wrap polish** — deepen the translucency effect so content feels truly inside the bubble

### Medium-term
- [ ] **Lock button** — bottom-bar button (alongside ↻ and ⚙) that password-gates the entire settings menu
  - PIN / pattern / passphrase (TBD)
  - Can't bypass via settings — auth required first
  - Password setup lives inside settings when unlocked
- [ ] **Bubble size control** — slider in settings per bubble (currently hardcoded per type)
- [ ] **Bubble colour picker** — custom colour per bubble instead of random palette

### Future / Coming Soon
- [ ] **Games** — already shown as "coming soon" in the add-bubble dropdown
- [ ] **Active hours** — restrict when the app is usable (already shown as coming soon in settings)
- [ ] **Usage limits** — per-day/week caps (already shown as coming soon in settings)
- [ ] **Curfew periods** — block access during certain times (already shown as coming soon in settings)
- [ ] **Reactivation frequency** — cooldown between uses (already shown as coming soon in settings)

---

## Lock Button (Detail)

- Dedicated lock button in bottom bar alongside ↻ refresh and ⚙ settings
- When locked: settings gear is visually disabled and non-functional
- Unlock: tap lock button → enter password → settings become accessible
- Password setup: inside settings (only reachable when unlocked)
- Cannot be bypassed through the settings menu itself

---

## Build Log

| Date | Change |
|------|--------|
| 2026-07-10 | Initial floating bubbles physics app, animated gradient background |
| 2026-07-10 | Touch drag + tap handler (`attachHandlers`) — drag moves, tap fires |
| 2026-07-10 | Settings panel: per-bubble toggle + delete |
| 2026-07-10 | Add bubble form with function-type dropdown (WhatsApp, picture, video, camera, PDF) |
| 2026-07-10 | Gravity slider (0–100) + Random fluctuation toggle per bubble |
| 2026-07-10 | Picture bubble: file/camera picker, base64 storage, circular photo in bubble |
| 2026-07-10 | Vercel project renamed → good-times-stop |
| 2026-07-10 | GitHub repo created: poon-holder/good-times-stop |
| 2026-07-10 | YouTube embed spec finalised (see table above) |

# PlayNow UI/UX Reference

Visual and interaction reference for the PlayNow family mission app.

**More Play. More Together.**

## Purpose

This repository is the PlayNow UI/UX reference implementation produced as a high-fidelity, clickable prototype.

Use it to inspect layout, hierarchy, motion, copy, and component behavior while implementing the design in production.

## Important

This is **not** the production PlayNow application.

Do **not** merge this repository into production, and do **not** treat it as the source of authentication, billing, or API behavior.

Implement the design in:

[KliporaHQ/PLAYHUB](https://github.com/KliporaHQ/PLAYHUB)

`KliporaHQ/PLAYHUB` is not modified by this repository.

This prototype does not call RevenueCat, does not talk to the production API, and does not change account, child, or subscription state in any real backend. Family data, saved missions, and the PlayNow+ selection live in memory for the session.

## Framework

| | |
|---|---|
| UI | React 19 |
| App framework | TanStack Start (file routes) + Vite |
| Styling | Tailwind CSS v4, tokens in CSS |
| Client state | Zustand |
| Icons | `lucide-react` |
| Language | TypeScript |
| Runtime | Node.js 22 |
| Package manager | npm |

## Install and run

```bash
npm install
npm run dev
```

`predev` and `prebuild` run `npm run assets`, which decodes `assets/encoded/*.b64` into `public/art/*.jpg` and `public/og.jpg`. Those image files are gitignored after decode. The committed bytes are the base64 files, so the illustrations are in the repository and are identical to the prototype.

`npm install` resolves the ranges in `package.json`. A lockfile is not committed.

```bash
npm run assets    # decode illustrations only
npm run typecheck
npm run build
```

Build output is the TanStack Start / Nitro production bundle (`npm run build`).

## Project structure

```text
src/components/playnow/ui.tsx       reusable components
src/components/playnow/screens.tsx  every screen
src/components/playnow/app.tsx      phone frame, screen index, navigation host
src/lib/playnow/data.ts             missions, categories, plans, screen inventory
src/lib/playnow/store.ts            in-memory demo state
src/styles.css                      design tokens, motion, surfaces
src/routes/__root.tsx               document shell, Poppins
src/routes/index.tsx                mounts the prototype
public/favicon.svg                  mark
assets/encoded/                     illustration bytes (base64)
scripts/materialize-assets.mjs      writes public images from those bytes
```

The rest of `src/lib`, `scripts/`, and `server/` is the app shell this prototype runs inside (routing, preview bridge, auth provider mounted but **auth disabled**). It is included so `npm run dev` matches the reference. It is not PlayNow product behavior to port.

## Screen inventory

All of these exist. Jump to any of them from the screen list beside the phone (wide layout) or **All screens** (narrow layout).

| Screen | Implementation | Notes |
|---|---|---|
| Splash | `Splash` | Mark, wordmark, tagline, pulse rings. Continues to onboarding. |
| Onboarding | `Onboard` | Five steps: promise, parent name, child, interests, first mission. Skip goes home. |
| Home | `Home` | Greeting, today's mission, vibe grid, quick ideas, family streak. |
| Explore | `Explore` | Search plus category mosaic and mission list. |
| Saved | `Saved` | Saved and Recent tabs. |
| Mission detail | `MissionDetail` | Hero, materials, prep, parent prompt, child prompt, sticky Start. |
| Active mission | `ActiveMission` | Countdown ring, current step, pause, next. |
| Mission completion | `Complete` | Celebration, streak, save, play again, next activity. |
| Family | `Family` | Parent, children, PlayNow+, preferences, security, help. |
| Child profile | `ChildScreen` | Avatar, age, interests, favorites, remove child. |
| PlayNow+ | `Paywall` | Benefits, monthly/annual, price, restore, legal copy. No dark patterns. |
| Empty saved | `Saved` with `empty-saved` | Heart-to-save empty state. |
| Empty children | `Family` with `empty-children` | Add-a-child empty state. |
| Empty search | `Explore` with `empty-search` | Query preset to a miss. |
| Empty recent | `Saved` with `empty-recent` | Recent tab with nothing played. |
| Loading | `LoadingScreen` | Skeleton of Home. |
| Offline | `NetworkError` | Try again, or open Saved. |
| Activity unavailable | `ActivityError` | Browse ideas. |
| Subscription unavailable | `SubscriptionError` | Plan unchanged. |
| Sign-in problem | `AuthError` | Try again, or continue as guest in this preview. |
| Design system | `SystemScreen` | Buttons, badges, chips, cards, ring, skeleton, modal, confirmation. |

Screen ids live in `screenIndex` in [src/lib/playnow/data.ts](src/lib/playnow/data.ts).

## Component inventory

Defined in [src/components/playnow/ui.tsx](src/components/playnow/ui.tsx):

| Component | Role |
|---|---|
| `Mark` | PlayNow logo mark |
| `Wordmark` | PlayNow wordmark |
| `PrimaryButton` | Dominant action. Purple-to-pink gradient. |
| `SecondaryButton` | Quiet action |
| `TextButton` | Low-emphasis action |
| `IconButton` | 44px icon target. Requires an accessible label. |
| `Badge` | Status and meta. Tones: purple, pink, blue, good, neutral, danger. |
| `Chip` | Selectable interest or filter |
| `MetaRow` | Duration, age, place, difficulty |
| `MissionCard` | `feature`, `tile`, or `row` |
| `CategoryCard` | Explore image card |
| `ChildCard` | Family list row |
| `Avatar` | Child photo or initial |
| `ProfileCard` | Parent card |
| `SubscriptionCard` | PlayNow+ entry |
| `ProgressRing` | Active-mission countdown |
| `ProgressDots` | Onboarding progress |
| `EmptyState` | Illustration, explanation, action |
| `LoadingSkeleton` | Branded shimmer skeleton |
| `ErrorState` | Error illustration plus next step |
| `BottomNavigation` | Home, Explore, Saved, Family. Sliding pill. |
| `Modal` | Bottom sheet inside the phone |
| `ConfirmationDialog` | Destructive confirm. Cancel is the safe default. |
| `SectionLabel` | Section heading |
| `RowButton` | Settings row |
| `SaveHeart` | Save / unsave |
| `CheckDot` | Benefit check |

Navigation icons are Lucide (`House`, `Compass`, `Bookmark`, `Users`), not emoji.

## Design tokens

Single source: [src/styles.css](src/styles.css) `@theme`.

Do not invent a second palette. These are the values the prototype uses.

### Color

| Token | Value | Use |
|---|---|---|
| `--color-purple` | `#7B3DFF` | Primary |
| `--color-purple-deep` | `#5A22D4` | Pressed / active label |
| `--color-purple-soft` | `#EFE7FF` | Selected chip, nav pill |
| `--color-pink` | `#FF4ED8` | Accent |
| `--color-pink-soft` | `#FFE4F7` | Soft accent surface |
| `--color-blue` | `#00C2FF` | Highlight |
| `--color-blue-soft` | `#E5F8FF` | Soft blue surface |
| `--color-ink` | `#0B0B1A` | Text and premium dark surfaces |
| `--color-ink-soft` | `#16132B` | Phone bezel, secondary dark |
| `--color-canvas` | `#F8F9FF` | App background |
| `--color-surface` | `#FFFFFF` | Cards |
| `--color-mist` | `#F1F0FA` | Secondary buttons, quiet fills |
| `--color-line` | `#E4E1F2` | Hairlines |
| `--color-muted` | `#5E5A78` | Supporting copy |
| `--color-faint` | `#8E8AA6` | Inactive nav, captions on dark |
| `--color-on-purple` | `#FFFFFF` | Text on the primary gradient |
| `--color-on-ink` | `#F8F9FF` | Text on dark |
| `--color-good` / `--color-good-soft` | `#0F7A52` / `#E5F6EE` | Easy |
| `--color-danger` / `--color-danger-soft` | `#B4234A` / `#FDE8EE` | Delete and destructive confirm |

### Gradient

Primary actions use `bg-linear-to-r` from `--color-purple` to `--color-pink`. Category cards use a dark fade (`from-ink`) over the photo. The stage behind the phone uses two restrained radial glows (purple, blue) on ink. Gradients are not used as decoration on every surface.

### Typography

Poppins, loaded in `src/routes/__root.tsx` (weights 400, 500, 600, 700). Fallback: `ui-sans-serif, system-ui, sans-serif`.

| Token | Size | Line height | Typical weight in UI |
|---|---|---|---|
| `display` | 2.25rem | 1.12 | 600 |
| `h1` | 1.75rem | 1.2 | 600 |
| `h2` | 1.375rem | 1.25 | 600 |
| `h3` | 1.125rem | 1.35 | 500 |
| `body` | 0.9375rem | 1.5 | 400, medium where it is a label |
| `caption` | 0.75rem | 1.35 | 500 |
| `button` | 1rem | 1.25 | 600 |
| `badge` | 0.6875rem | 1.2 | 600, uppercase, slight tracking |

### Radii

| Token | Value |
|---|---|
| `--radius-card` | 1.5rem |
| `--radius-control` | 1rem |
| `--radius-pill` | 999px |
| `--radius-phone` | 2.5rem |

### Shadow

| Token | Value |
|---|---|
| `--shadow-card` | `0 16px 40px -24px rgb(11 11 26 / 0.38)` |
| `--shadow-float` | `0 24px 60px -28px rgb(11 11 26 / 0.65)` |
| `--shadow-nav` | `0 10px 30px -16px rgb(11 11 26 / 0.35)` |

### Spacing and targets

Spacing uses the Tailwind 4px scale (`p-4`, `gap-3`, `px-5`). Tap targets are at least 44px (`min-h-11`, `size-11`). The phone frame targets 390×844 and shrinks to fit shorter desktop windows. Narrow layouts are full-bleed.

### Motion

Respects `prefers-reduced-motion`.

| Name | Timing | Where |
|---|---|---|
| Press (`.tap`) | 150ms ease-out, scale 0.96 | Buttons and cards |
| Enter (`.rise`) | 460ms, 50–60ms stagger | Screen sections |
| Nav pill | 200ms ease-out | Bottom navigation |
| Ring | 500ms stroke transition | Countdown |
| Shimmer | 1.5s loop | Skeletons |
| Logo pulse | 2.6s, second ring delayed 0.8s | Splash |
| Celebration | 900ms | Completion confetti. Hidden when reduced motion is on. |

### Icons

`lucide-react`. Stroke width 1.8 inactive, 2.4 on the active tab. Difficulty and status are text plus color, never color alone.

## Assets

| Path | What |
|---|---|
| `public/favicon.svg` | Logo mark |
| `assets/encoded/public__og.jpg.b64` | Share card |
| `assets/encoded/public__art__*.jpg.b64` | Mission, portrait, empty, and offline illustrations |
| `public/art/*.jpg` | Created locally by `npm run assets` |

Illustrations are miniature still-life photographs: volcano, fort, nature hunt, rainbow, dance, stars, notes, rocket, shadows, balance, splash, together, Leo, Ava, empty box, offline.

## Mock data

[src/lib/playnow/data.ts](src/lib/playnow/data.ts) holds ten missions, categories, vibes, interest options, and PlayNow+ prices.

[src/lib/playnow/store.ts](src/lib/playnow/store.ts) holds the demo family:

- Parent: Maya
- Children: Leo (6) and Ava (9)
- Saved: Blanket Fort Cinema, Backyard Star Count, Kindness Notes
- Plan prices shown: monthly $6.99, annual $49.99

Prices, names, and missions are **demo copy**. Nothing is billed. "Continue" on PlayNow+ only flips a local `subscribed` flag. Delete account and remove child do not call a server. Toasts say so.

## Environment variables

No secrets are required.

| Name | Required | Notes |
|---|---|---|
| `VITE_AUTH_ENABLED` | No | Set to `"false"` in `.grok/app-env.json`. `npm run dev` and `npm run build` read it through `scripts/with-app-env.mjs`. |
| `DATABASE_URL` | No | Leave unset. Migration step no-ops. |
| `GROK_AUTH_CLIENT_SECRET` | No | Do not set. The checked-in preview constant is the placeholder `EXAMPLE_PREVIEW_CLIENT_SECRET`. |
| `BETTER_AUTH_SECRET` | No | Not used by this UI prototype. |

There is no `.env` file. Do not add RevenueCat keys, API tokens, or signing keys here.

## Accessibility

- Visible labels on icon buttons
- `aria-current` on the active tab
- `aria-pressed` on chips, save, and plan choices
- Dialogs use `role="dialog"` or `role="alertdialog"`
- Timer updates are not announced every second
- Contrast pairs are tokenized (ink on canvas, on-ink on ink, on-purple on the gradient)
- Focus ring: 2px purple
- Reduced motion collapses animation

## Known limitations

- Design prototype only. Session state resets on refresh.
- Auth UI in `src/lib/auth` is the app shell, mounted with auth **off**. Do not copy it into PLAYHUB as a replacement for production auth.
- The "Created with Grok" preview chrome is injected by the builder when this app is served there. It is not a PlayNow screen.
- Illustrations are stored as base64 so the repository can carry the exact bytes. Run `npm run assets` (automatic before dev and build) before expecting `public/art` to exist.
- PlayNow+ does not contact a store. Restore purchases shows a prototype toast.
- Wide layout shows a reviewer screen index. Narrow layout shows an **All screens** bar. Those controls are for design review, not production navigation.
- Bottom navigation is hidden on splash, onboarding, mission detail, active mission, completion, child profile, paywall, errors, and the design-system screen.

## Secrets included

No.

# Hanpass × DAISO — My New Life in Korea (concept simulation)

## Project Background

This virtual pop-up is the final phase of a larger bootcamp project centred on **Hanpass**, a financial-services platform for foreign residents in Korea. After identifying usability issues in the current app, our team moved through two further phases.

### 1. App improvement
We redesigned parts of the Hanpass app experience to address the issues we found. Changes included [briefly describe the redesigned flows or features]. The aim was to make everyday financial tasks clearer and easier for newcomers to Korea.

### 2. Brand collaboration
With the product improvements defined, we looked at how to bring more new users to Hanpass. We evaluated potential partner brands and chose **DAISO** because [e.g. its everyday, affordable products reach the same newcomers who are setting up a life in Korea]. The result is **"My New Life in Korea"**, a proposed experiential pop-up that walks visitors through arrival, a first shopping trip and their financial life, with Hanpass shown at each stage.

This is an interactive virtual pop-up store for a **hypothetical** experiential marketing proposal. A visitor walks
through four booths, collects passport stamps, shops on a ₩10,000 budget, explores three financial-life
kiosks, completes a social mission, and spins a capsule lucky draw.
Project Link: https://claude.ai/artifact/D99qxSrC4ph6i7JAXYXrub
> This is a student/portfolio concept. Hanpass and DAISO have **not** approved, sponsored, launched or
> implemented this campaign. No real payment, remittance, booking, voucher, registration or data collection happens.

## Run it

Requires Node.js 20+.

```bash
npm install        # install dependencies
npm run dev        # start the dev server → http://localhost:5180
npm run typecheck  # TypeScript check only
npm run build      # type-check + production build into dist/
npm run preview    # serve the production build
```

## Experience flow

| Stage | What happens | Stamp |
|---|---|---|
| Entrance | Front facade, "Enter the experience" camera push-in | — |
| 01 Arrival | Staff narration → fictional arrival card → animated Starter Pass, identity sticker, baggage tag | 01 Arrival complete |
| 02 DAISO Shopping | Basket → 3-minute real countdown, ₩10,000 budget, add/remove/quantity, over-budget block → auto checkout on expiry → mock cashier → proposed 1% cashback toggle → collectible receipt (PNG download / print-to-PDF) | 02 DAISO shopping complete |
| 03 Financial Life | Salary story → choose one of three kiosks (remittance, travel, expenses); others optional | 03 Financial life complete |
| 04 Exit | Hanpass corner, All About DAISO, social mission (copy hashtags, Story preview, simulate completion) → capsule lucky draw | Completion badge |
| Complete | Fully stamped pass, receipt, chosen scenario, simulated reward, restart / review / strategy | |

Navigation: **Experience · Map · Starter Pass · Project Strategy**, a persistent progress tracker with
Back / Next, and a floating Korea Starter Pass. Header toggles: *Skip animation*, *Skip narration*, *Sound*
(off by default, synthesised in-browser, never autoplayed). `prefers-reduced-motion` is respected.

Progress is saved in `localStorage` (fictional name, nationality, avatar and journey state only).
Restart asks for confirmation.

## Architecture

```
src/
  state/
    types.ts              typed state + actions
    machine.ts            guarded reducer (state machine), selectors, budget/cashback math
    ExperienceContext.tsx provider, persistence, UI preferences
  data/                   ← EDIT SIMULATION CONTENT HERE
    scenes.ts             source renderings + list of missing assets
    stamps.ts             stamp/badge labels and order (see note on the stamp-count inconsistency)
    identity.ts           nationalities, avatar stickers, preset identities
    products.ts           simulated catalog, budget, timer length, proposed cashback rate
    financial.ts          remittance countries, travel routes, salary/expense categories
    finale.ts             prize categories, hashtags, external links, DAISO categories
    strategy.ts           case-study copy (missing inputs marked "Not supplied")
  components/
    EntranceScene, StoreMap, JourneySummary
    shell/     ExperienceShell, SceneStage (camera/parallax/hotspots), NarrationPanel, StageHeader
    pass/      StarterPass (chip + booklet), StampMark, StampMoment, Avatar
    arrival/   ArrivalExperience, IdentityForm
    shopping/  ShoppingChallenge, ShoppingBasket, MockCheckout, ReceiptPreview, ProductArt
    financial/ FinancialLifeExperience, RemittanceDemo, TravelDemo, ExpenseDemo, KioskFrame
    finale/    FinaleExperience, Corners (Hanpass / DAISO / SocialMission), LuckyDraw
    strategy/  StrategyCaseStudy
  lib/         format, sound (Web Audio), receipt (canvas PNG)
public/assets/scenes/   the five supplied renderings (renamed copies; originals left in place)
```

All transitions go through `reducer` in `state/machine.ts`. Stages unlock only in order, a stage can only be
stamped when its activity is done, the basket can never exceed ₩10,000, and the lucky draw needs the
(real or simulated) social mission.

## Source assets

Used (copied to `public/assets/scenes/`):

| File in this folder | Used as |
|---|---|
| `Hanpass × Daiso_ New Life in Korea.png` | Front facade, entrance scene, store map |
| `Booth 01 Arrival` | Booth 01 Arrival |
| `Booth 02 DAISO Shopping Challenge` | Booth 02 DAISO Shopping Challenge |
| `Booth 03 Hanpass Financial Life` | Booth 03 Hanpass Financial Life |
| `Booth 04 My New Life in Korea` | Booth 04 My New Life in Korea |

**Missing — placeholders used:**

- Original floor plan / layout drawing → the Map uses the front facade rendering, with a visible note.
- Standalone logo files → brand names set in plain type; no logos were redrawn or invented.
- Product photography → labelled illustration tiles.
- Research and KPI material → shown as "Not supplied" in Project Strategy.
- Instagram Story artwork → a simple generated preview.

## Content rules followed

- Every simulated, proposed or illustrative element carries a visible label (e.g. "Proposed campaign benefit — not a live offer", "DEMO ONLY — NO MONEY IS SENT", "SIMULATED REWARD — NOT REDEEMABLE").
- No exchange rates, fees, fares, availability, prize values, odds, redemption codes or store locations are shown.
- Only fictional name, a nationality from a list and an avatar are asked for. No passport, visa, address, phone, password or financial data.
- External links go only to the brands' official top-level sites.
- Stamp counting: the brief mentions a first, second and "fourth" stamp with no third defined. No activity was invented; one stamp per core activity (01–03) plus a completion badge at 04. Change it in `src/data/stamps.ts`.

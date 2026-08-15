# Resolution Centre — concept prototype

An in-app Resolution Centre for a Pakistani digital wallet, built for a Product Analyst
case study. It is a working product, not a slide deck: every state described in the PRD
can be driven live, annotated on screen, and walked end to end.

Concept only. It uses a generic "Wallet" wordmark and is not affiliated with, or branded
as, any real financial institution. No real customer data appears anywhere in it.

---

## The argument

When a payment fails, most wallets go quiet. The customer cannot tell whether the money
is lost, held, or on its way, so they call — and every silence becomes a support ticket,
a one-star review, and a reason to move money elsewhere.

The Resolution Centre answers inside the product instead:

1. **A live status** on every transaction — four defined states, never a blank one.
2. **A plain-language reason**, from a closed, product-owned taxonomy, in English or Urdu.
3. **A deadline we publish** at the moment of failure, and announce when we miss.
4. **A one-tap way out** — a dispute raised from the transaction itself, pre-filled and
   locked, with a reference number and a committed turnaround in under five seconds.
5. **Provisional credit** when we pass our own deadline: the balance is made whole and we
   carry the reconciliation.

---

## Quick start

```bash
cd prototype
npm install
npm run dev        # http://localhost:5173/resolutioncenter-prototype/
```

```bash
npm run build      # production build into dist/
npm run preview    # serve that build
npm run seal       # build, then encrypt it for public static hosting (see below)
```

Node 18 or newer. The `base` path in `vite.config.js` is set for deployment under
`/resolutioncenter-prototype/`, and the dev server honours it.

---

## What is in here

### The landing page

The entry point states the business case — *Recover the payment. Keep the customer.* —
beside the live product, with the proposed behaviour spotlit inside the device. **See how
it works** dives into the workspace: the device straightens, the copy clears, and the
workspace assembles around it.

### The workspace

Three zones, one idea each:

- **Left — the argument.** What this screen is for, in business terms. The copy changes
  with the state, not just the screen: the duplicate-report sheet argues about queue cost,
  the succession screen about regulatory exposure. 25 variants in total.
- **Centre — the product.** The real app, in a CSS iPhone stencil.
- **Right — the controls.** The journey you are in, which step you are on, and the flows
  you can jump to.

### Explainers

Toggle **Explainers** in the header, then hover anything marked on the screen. A target
ring appears on the element, a dotted elbow draws out to a callout, and the note types
itself in. 54 notes across every screen and state.

The annotations are anchored to the real DOM — each is keyed to a `data-ex` attribute on
the element it describes — so a note can only exist while the thing it points at is
actually on screen. Nothing is positioned by hand.

### Process flows

**Flows** in the header opens every journey end to end. Each has:

- a **track** — the steps in order, with the current one marked *You are here*;
- a **swimlane** — one row per actor (Customer · The app · Ops & policy · Bank/biller),
  so the hand-offs are visible, with connectors measured from the real cards;
- a **total SLA** in the header, and a per-step ETA drawn from the same commitments the
  app publishes;
- **Walk this flow** — a full-screen mode that steps the prototype through the journey at
  1.9s per step with the device alongside, pausable and stoppable at any point.

The same journey appears beside the prototype in the dock, so you always know where the
screen in front of you sits.

---

## Screens

| Screen | What it demonstrates |
|---|---|
| Transaction list | A status on every row; tap a chip for the reason inline |
| Transaction detail | Four-step live timeline, plain-language reason, SLA countdown |
| Dispute sheet | One tap from the transaction, pre-filled and locked |
| Ticket confirmation | Reference number, committed turnaround, what happens next |
| Account status | Restriction reason, the single action that clears it, what still works |
| My reports | Ticket threading and the in-app inbox |
| System status | The outage page — cause, ETA, next update |
| Send money | First payment to a new payee: activation, safety hold, scheduling |

---

## Journeys

| Journey | Total SLA |
|---|---|
| Failure → resolution | 2h auto-reversal · 24h if disputed |
| When we miss our own deadline | 2h, then provisional credit |
| Restricted account | 2h to verify · 24h dispute |
| Mass outage | Fix by 5:00 PM · ~3h |
| Dormant / succession | 2 working days to first contact |
| First payment to a new payee | Ready in ~10s · 5-min hold, stated |
| Edge cases (a set, not a journey) | Varies by case · 2h – 48h |

### First payment to a new payee

Added after a real bug report: sending to a newly added beneficiary sometimes errors, and
works if you wait a few seconds. That is a readiness problem, not a tap problem — the
payee exists in one service before it exists in the next, and a risk rule may hold it for
a few minutes. The product answer is to model the payee's readiness as a state, gate the
action on it, and never present a known transient state as a failure:

`Payee added → Activating → Safety hold, stated → Scheduled, not refused → Confirming → Paid, first attempt`

---

## Edge cases

Numbered exactly as in the PRD. Cases 5–9 are not missing from the set — they run inside
the journeys above.

| # | Case | What it shows |
|---|---|---|
| 1 | Offline | Cached status with a timestamp; SMS carries the same reason code |
| 2 | Partial reversal | Principal returned, fee retained; the dispute targets the fee only |
| 3 | Genuine fraud hold | Legitimate hold, reason shown without revealing detection logic |
| 4 | Duplicate dispute | A second report merges into the existing thread |
| 5 | SLA breached | An upstream bank misses its SLA; a revised commitment, never silence |
| 6 | Urdu · اردو | Low-literacy and Urdu-first users |
| 7 | Credit cap reached | The provisional-credit abuse guardrail, quoted to the customer |
| 8 | Mass outage | One system banner with a cause and an ETA |
| 9 | Dormant / succession | The dispute path branches to succession, not a generic ticket |
| 10 | Notification failed | SIM swap or DND; the in-app inbox becomes the source of truth |

**Urdu** switches status labels, reason lines and body copy; amounts and reference numbers
stay in Latin digits, as they are read in practice. The scroll area flips its scrollbar to
the reading edge. Strings are written for comprehension rather than literal translation —
have a native speaker check them before presenting.

---

## URL parameters

Any deep link skips the landing page, so headless captures are pixel-stable.

```
?s=detail&scenario=sla_breached&bare=1
```

| Param | Values |
|---|---|
| `s` | `home` `detail` `confirm` `reports` `account` `system` `send` |
| `scenario` | `normal` `resolved` `provisional` `offline` `partial` `fraud` `duplicate` `sla_breached` `abuse` `outage` `dormant` `notif_failed` |
| `sheet` | `1` opens the dispute sheet |
| `send` | `idle` `activating` `held` `queued` `confirming` `done` |
| `lang` | `en` or `ur` |
| `explain` | `1` turns on the annotation layer |
| `ex` | pins one annotation open, e.g. `home.chip` — for capturing a hover state |
| `flows` | `1` opens the process-flow view |
| `walk` | a flow key (`recover` `sla` `restricted` `outage` `succession` `newpayee`) — opens walking it |
| `demo` | `1` opens the device-stencil kit |
| `present` | `1` for the five-up view |
| `enter` | `1` skips the landing page |
| `scroll` | pixels to pre-scroll the phone's scroll area |
| `k` | the passphrase, for scripted captures past the gate |

---

## Structure

```
scripts/
  seal.mjs        inline + encrypt the build for static hosting
src/
  main.jsx        entry point
  gate.jsx        passphrase wall for unsealed builds (SHA-256 digest only)
  App.jsx         shell, state, routing, the narrative copy for every screen and state
  landing.jsx     the entry page
  screens.jsx     all eight screens
  ui.jsx          chrome, chips, banners, timeline, brand mark
  iphone.jsx      the device stencil — a reusable component, no image
  demo.jsx        the device kit page
  explainer.jsx   the annotation layer and its 54 notes
  flows.jsx       journeys, swimlanes, walk mode, the edge-case set
  data.js         seed data, reason-code taxonomy, scenarios, Urdu strings
  icons.jsx       line-icon set, 1.6px stroke
  backdrop.svg    the page artwork
  styles.css      design tokens and component styles
```

---

## Decisions worth defending

- **One source of truth.** All seed data, reason codes and scenario definitions live in
  `data.js`, so no two screens can disagree about the same payment. That is the product
  argument as much as an engineering one — the app, the SMS and the agent console reading
  from one taxonomy is what stops channels contradicting each other.
- **A closed reason-code taxonomy.** Free text never becomes the routing key. Ops triage
  the same codes the customer reads.
- **Annotations anchored to the DOM.** Notes are keyed to `data-ex` attributes on real
  elements and measured at runtime, so they cannot drift out of alignment or describe
  something that is not on screen.
- **Flows as data.** Journeys, lanes, ETAs and SLAs are declared in `flows.jsx`; the track,
  the swimlane, the dock rail and the walkthrough are all renderings of the same array.
- **The device is a component, not an image.** `iphone.jsx` builds the shell in CSS —
  titanium/black/white finishes, a real 3D edge when tilted, a fixed 390 × 844 screen — so
  the UI inside is never scaled or distorted, and it stays sharp at any size.
- **No invented metrics.** Every business claim in the copy is directional — support cost,
  contact volume, retention, regulatory exposure. Fabricated percentages are the fastest
  way to lose the room.

---

## Accessibility and motion

Interactive controls are real buttons with `aria-pressed` where they toggle; sections are
labelled; the scenario note is a live region. All decorative motion — the drifting
backdrop, the swimmer on the current step, the typewriter, the marching connectors —
is disabled under `prefers-reduced-motion: reduce`.

Everything scales to fit the viewport, so nothing is cut off or pushed below the fold at
any window size. Below 1260px the narrative column drops; below 1180px the landing labels
drop; below 1200px the walk-mode device column drops.

---

## Known limitations

- The prototype is front-end only. There is no backend, no persistence, and no real
  payments — states are switched, not computed.
- Urdu strings need a native-speaker review before this is shown to anyone.
- `crypto.subtle` requires a secure context, so the gate and the sealed build work on
  `https://` and `localhost`, but not on a plain `http://` host.
- The five-up view and the device kit are presentation tools, not part of the product.

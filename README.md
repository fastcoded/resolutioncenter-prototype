# Resolution Centre - concept prototype

An in-app Resolution Centre for a Pakistani digital wallet, built for a Product Analyst
case study. Concept only. It uses a generic "Wallet" wordmark and is not affiliated with, or branded
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

## What is in here

### The landing page

The entry point states the business case — *Recover the payment. Keep the customer.* —
beside the live product, with the proposed behaviour spotlit inside the device. **See how
it works** dives into the workspace: the device straightens, the copy clears, and the
workspace assembles around it.


### Explainers

Toggle **Explainers** in the header, then hover anything marked on the screen — or tap it,
on a device without a pointer. A target ring appears on the element, a dotted elbow draws
out to a callout, and the note types itself in. 67 notes across every screen and state.

The annotations are anchored to the real DOM — each is keyed to a `data-ex` attribute on
the element it describes — so a note can only exist while the thing it points at is
actually on screen. Nothing is positioned by hand.

On a touch device the callout has no margin to fly into, so it is pinned inside the device
as a sheet, and the tap that opened it is swallowed rather than also driving the prototype.
Turn Explainers off to use the screen normally.


### Process flows

**Flows** in the header opens every journey end to end. Each has:

- a **track** — the steps in order, with the current one marked *You are here*;
- a **swimlane** — one row per actor (Customer · The app · Ops & policy · Bank/biller),
  so the hand-offs are visible, with connectors measured from the real cards;
- a **total SLA** in the header, and a per-step ETA drawn from the same commitments the
  app publishes;
- **Walk this flow** — a mode that shows that journey alone and steps the prototype through
  it at 1.9s per step with the device alongside, pausable and stoppable at any point. (The
  device column is dropped below 1200px, so the steps still walk but without the preview.)

The same journey appears beside the prototype in the dock, so you always know where the
screen in front of you sits.

---

## Screens

| Screen | What it demonstrates |
|---|---|
| Transaction list | A status on every row; tap a chip for the reason inline |
| Transaction detail | Four-step timeline, plain-language reason, and the deadline stated as a clock time |
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
| Edge cases — five of them, a set rather than a journey | Varies by case · 2h – 48h |

### First payment to a new payee

Added after a real bug report: sending to a newly added beneficiary sometimes errors, and
works if you wait a few seconds. That is a readiness problem, not a tap problem — the
payee exists in one service before it exists in the next, and a risk rule may hold it for
a few minutes. The product answer is to model the payee's readiness as a state, gate the
action on it, and never present a known transient state as a failure:

`Payee added → Activating → Safety hold, stated → Scheduled, not refused → Confirming → Paid, first attempt`

---

## Edge cases

The PRD numbers ten, and the numbering here is exactly that one. The prototype reaches
them in two places rather than one list: **five stand alone** in the *Edge cases* set on
the flows page — labelled *the rarer ones* — and **four are steps inside the journeys
above**, because that is where they actually occur. A case that only happens partway
through a journey is more honest as a step in it than as a tile on its own.

| # | Case | Where it is | What it shows |
|---|---|---|---|
| 1 | Offline | the set | Cached status with a timestamp; SMS carries the same reason code |
| 2 | Partial reversal | the set | Principal returned, fee retained; the dispute targets the fee only |
| 3 | Genuine fraud hold | the set | Legitimate hold, reason shown without revealing detection logic |
| 4 | Duplicate dispute | the set | A second report merges into the existing thread |
| 5 | SLA breached | *When we miss our own deadline* | An upstream bank misses its SLA; a revised commitment, never silence |
| 6 | Credit cap reached | *When we miss our own deadline* | The provisional-credit abuse guardrail, quoted to the customer |
| 7 | Mass outage | *Mass outage* | One system banner with a cause and an ETA |
| 8 | Dormant / succession | *Dormant / succession* | The dispute path branches to succession, not a generic ticket |
| 9 | Notification failed | the set | SIM swap or DND; the in-app inbox becomes the source of truth |



## Layout at other sizes

On a laptop the whole workspace is fitted to the viewport, so nothing is cut off or pushed
below the fold. The columns are shed as the window narrows:

| Width | What changes |
|---|---|
| below 1260px | the narrative column drops |
| below 1200px | the walk-mode device column drops |
| below 1180px | the landing labels drop |
| below 1080px | the landing device drops; the argument centres |
| below 760px | the workspace stacks — device above, dock below — and the page scrolls |

Below 760px the fitting rule inverts: the device is scaled to the width and the page is
allowed to scroll, because honouring the viewport height there would shrink it to something
nobody can read. The stylesheet and the measuring code switch on the same number, and they
have to agree — a scale computed for a layout that is not on screen is the bug this
replaced.

The flows page keeps its swimlane at full width and scrolls it inside its own box rather
than widening the page; the step track runs down the screen instead of across.

Verified with no horizontal overflow across every view at 320, 360, 390, 430 and 768px.
Note that Chrome on Windows clamps a window to about 504px, so `--window-size=390` renders
at 504 and crops the screenshot — narrow widths have to be tested in an iframe or with
device emulation, not by resizing a headless window.

---

## Known limitations

- The prototype is front-end only. There is no backend, no persistence, and no real
  payments — states are switched, not computed.
- Urdu strings need a native-speaker review before this is shown to anyone.
- The five-up view and the device kit are presentation tools, not part of the product.
- Hints are hover-and-focus affordances. On a touch device only the non-interactive ones
  can open, since a control's tap belongs to the control.
- The prototype is designed for a laptop and holds up on a phone; it is not a mobile-first
  layout. The narrative column and the landing device are dropped rather than reflowed,
  because the argument they carry is already in the deck.

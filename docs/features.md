# Tend — feature definition, V1

**Status:** draft 3, for Albert to validate · **Date:** 2026-11-21
**Inputs:** Orvys `product-spec.md`, `bsport-inventory.md` (walk of TUBA's live back office, 20 Nov 2026), `admin-ux-analysis.md` (jobs J1–J8).

This is the only planning document for now. Order of work: **features (this file) → designs → validation → development.** Nothing is built before its design is validated.

---

## 1. What V1 is

**Customer:** one studio, TUBA. Owner, front desk, teachers, members.

**Gate:** TUBA runs a full week on Tend and BSport is switched off.

**Rule for scope:** a feature is in V1 only if TUBA uses it in BSport today (inventory §8) or TUBA cannot operate a week without it. Everything else waits, however good the idea.

**What changes from Orvys**

| | Orvys | Tend |
|---|---|---|
| Order | Schema first, screens after | Designs validated first |
| Scope | 10 modules, 3 versions | One studio leaves BSport |
| Unit of design | Database tables | The eight staff jobs and the member's booking |
| Look | Own styles | Base design system (tokens in `base/src/app/globals.css`) |
| Stack | Decided up front | Decided after designs are validated |

**What is kept from Orvys:** the business rules already decided for passes, credits, refunds, cut-offs and renewals (spec §4–§5 and the `intent/` files). They are rules, not screens, and were expensive to get right. They are re-read feature by feature when development starts, not copied wholesale.

---

## 2. Features

Three surfaces. `S` = staff back office, `M` = member web, `P` = platform (no screen of its own).

### Staff back office

| # | Feature | What it does | Job | BSport at TUBA |
|---|---|---|---|---|
| S1 | **Member search** | Search bar in the header on every page: name, email, phone. Opens the member. Creates the member when there is no match. | J1 | Used |
| S2 | **Today** | Landing page. The day's classes as rosters with fill. A short "needs attention" list: class with no teacher, failed payments, passes that did not renew. | J2, J7 | Used (Home, Calendar) |
| S3 | **Class roster** | Attendees with the pass used; no credits shown. Each member is Present or Absent, starting from the studio default; members still Absent when the roll call is confirmed are no-shows. Present and Absent counts appear only after the roll call. The waitlist sits under the booked list and shows only when the class is full: add, remove, book. A second view lists members no longer coming (cancelled, or removed from the waitlist). Edit the class, replace the teacher for this class, and per member: open profile, sell, email or call, move, note for the teacher, cancel with or without the credit. | J2, J1 | Used |
| S4 | **Schedule** | Week and day calendar. Create a class, one-off or recurring. Edit this class or the series. Move or cancel a class, with the members affected shown first. | J5 | Used |
| S5 | **Teacher cover** | Put a substitute on one class. The usual teacher stays on the series. Payroll follows who taught. | J3 | Used (done as "change teacher") |
| S6 | **Member profile** | One page: contact, active passes and subscriptions, next bookings, past bookings, purchases and invoices, notes. Sell and Book always in reach. | J1, J4 | Used |
| S7 | **Pass actions** | Pause, extend, cancel (now or at period end), give a credit back. Each asks for a reason and is recorded with who did it. | J4 | Used |
| S8 | **Catalog** | Class types (name, duration, capacity, booking window, cancellation deadline, which passes cover it). Passes (credits or unlimited, validity, price, tax, new clients only). Subscriptions (monthly price, renewal, commitment). Promotion codes. | — | Used |
| S9 | **Desk sale** | Sell a pass, a subscription or a product to a member: charge their saved card, send a payment link, or record cash / transfer / terminal. Reached from the profile, the roster row and the booking search. | J1, J8 | Used |
| S10 | **Money** | Payments list, invoices, refunds with a reason, failed payments, payouts. Monthly export for the accountant. | J8 | Used |
| S11 | **Payroll** | Rate per teacher per class type. Monthly run from classes taught, substitutes included. Export. | J8 | Used |
| S12 | **Reports** | Four only: revenue by product, bookings and fill by class, active members and subscriptions, unused credits. CSV export. | J8 | Used (subset of 30+) |
| S13 | **Staff and roles** | Invite by email. Four roles: Owner, Manager, Front desk, Teacher. Role change in a dialog; Owner changes are confirmed. | J6 | Used |
| S14 | **Settings** | One page each for: studio details, rooms, booking rules (including whether members start as Absent or Present), waitlist, invoices and tax, messages. | — | Used (22 pages there) |
| S15 | **Teacher view** | A teacher sees their own classes and rosters, and marks attendance. Phone first. | J2 | Used |
| S16 | **Inbox** | One shared queue for member requests. Emails to the studio address arrive in it and are answered from it. Requests from the desk, phone, WhatsApp and Instagram are logged by hand in a few seconds. Each request has an owner, a status (open, waiting, done) and its age; open requests older than 24 hours show on Today. The member's passes and bookings sit beside the conversation, so pause, extend, cancel and credit back (S7) are done without leaving it. Saved replies. Notes for staff. | J4 | Locked by plan |
| S17 | **Products and stock** | Merch sold at the desk. A product has a name, a price, a tax rate and a number in stock; sizes are separate products. A sale takes one off, a refund puts it back, staff correct the count with a reason. A product at zero can still be sold after a confirmation. Sales show on the member's purchases and invoices and as their own line in the revenue report. | J8 | Lightly used (one webshop item) |
| S18 | **Establishments and rooms** | A studio has one or more establishments (addresses), each with one or more rooms. Every class is in one room of one establishment. The schedule and Today show one establishment at a time; with a single establishment the choice is hidden. Decided 21 Nov 2026. | J5 | Used (Venues) |

### Member web

One responsive web app on the studio's address. No native app in V1 (see open decision D1).

| # | Feature | What it does |
|---|---|---|
| M1 | **Timetable** | Browse classes by day, filter by class type and teacher. Shows spots left. Visible without an account. |
| M2 | **Account** | Sign up and sign in by email link or password. Profile, consent. |
| M3 | **Book** | Book with a pass that covers the class. Join the waitlist when full. Buy and book in one flow when there is no pass. |
| M4 | **Buy** | Buy a pass or start a subscription by card. Promotion code. Saved card. |
| M5 | **My bookings** | Upcoming and past. Cancel within the deadline; a late cancel shows what it costs before confirming. |
| M6 | **My passes** | Credits left, end date, next payment date, invoices, update card. |

### Platform

| # | Feature | What it does |
|---|---|---|
| P1 | **Booking rules** | Booking window, cut-off, cancellation deadline, late-cancel penalty, no double booking, pass eligibility, waitlist promotion. No booking without a pass that covers the class: at the desk, staff sell first, then book. A credit is taken when the class is booked and returned on a cancel in time. When several passes cover a class, the pass is chosen at booking: preselected (unlimited subscription first, otherwise the pass that ends soonest) and changeable. Staff can change the pass used on a booking afterwards, also after the class, in one step that returns the credit to the first pass and takes it from the second. One set of rules for staff and members. |
| P2 | **Payments** | Card payments, recurring subscription billing, retries on failure, booking block after repeated failure, refunds. |
| P3 | **Invoices** | Sequential numbered invoices with VAT, credit notes on refund. |
| P4 | **Messages** | Outgoing email only. About 12 messages, not BSport's 63: account, booking confirmed / cancelled / reminder, waitlist offer, class cancelled or moved, receipt, payment failed, pass expiring, renewal. |
| P4b | **Incoming email** | Mail sent to the studio address is received, matched to a member by sender address, and threaded with replies. Needed by S16. |
| P5 | **Record of changes** | Who did what and when, for every money, pass, booking and role change. |
| P6 | **Move from BSport** | One import for TUBA: members, active passes with credits left, subscriptions, future bookings, timetable. Run once, checked by hand. Not a product feature. |
| P7 | **Privacy** | Member data export and erasure, consent record, EU hosting. |

---

## 3. Not in V1

Decided out because TUBA does not use it, or can run a week without it.

| Left out | Why |
|---|---|
| AI front desk, owner copilot, MCP server | The differentiator, but not what blocks leaving BSport. Second release. |
| Native white-label app | Web first. See D1. |
| Spot and apparatus booking (pick your reformer) | TUBA's BSport plan has one number, "total spots". Second release. |
| Several studios as separate customers, platform master admin, take rates | One studio as customer. Built when studio two signs. Several establishments inside one studio are in V1 (S18). |
| Website builder, on-demand video, livestream | Not used. |
| Gift cards, selling merch online, packs, instalments, expenses, purchase orders, barcodes | Not used. Merch at the desk with stock is in V1 (S17), decided 21 Nov 2026. |
| Appointments / private sessions | One service, lightly used. See D4. |
| Marketing: campaigns, segments, automations, referrals | Two workflows active. See D5. |
| WhatsApp, Instagram and SMS connected to the inbox; push | The inbox (S16) receives email and takes the other channels as hand-logged requests. Connecting WhatsApp or Instagram needs Meta business accounts and approval. See D10. |
| Replies drafted by AI | Second release, with the AI front desk. |
| Member self-serve pause and cancel | Staff do it in one click (S7). Self-serve comes second. |
| Custom forms and waivers | See D6. |
| Tags as eligibility, linked profiles, custom dashboards, 26 other reports | Not needed for the week. |
| Aggregators (ClassPass, Urban Sports) | Not used. |
| Languages other than the one TUBA runs in | See D7. |

---

## 4. Open decisions

Each one changes scope or design. Recommendation given where there is a basis for one.

| # | Decision | Recommendation |
|---|---|---|
| D1 | Do TUBA members book in a BSport branded app today? If yes, is a web app on the phone's home screen acceptable at switch? | Unknown, not in the inventory. Web only if acceptable; a native app roughly doubles the member surface. |
| D2 | How do the saved cards of current subscribers move from BSport to the new payment provider? | **The largest risk to the gate.** Unknown. If cards cannot be transferred, every subscriber must re-enter a card. Needs an answer from BSport before development. |
| D3 | Payment provider: Stripe, as in the Orvys spec? | Stripe. Card only at start, as TUBA today. SEPA later. |
| D4 | Appointments: drop the one service, or keep taking it outside Tend? | Drop from V1. |
| D5 | Which two Audience workflows are active, and can they stop for a while? | Unknown. Needs the two names. |
| D6 | Does TUBA require a signed waiver or health form at sign-up? | Unknown. If yes, one fixed consent checkbox in M2, no form builder. |
| D7 | Which languages do staff and members need on day one? | Unknown. |
| D8 | Payroll in V1, or keep the monthly run in a spreadsheet from an export for the first months? | Export first. It removes S11 from the gate without blocking the switch. |
| D10 | Inbox channels: is email plus hand-logging enough for V1, or must WhatsApp messages arrive in the inbox by themselves? | Email plus hand-logging. WhatsApp connected is the first addition after the switch. Unknown: how the volume splits across the four channels today. |
| D11 | Which address do members write to today, and can its mail be forwarded to Tend? | Unknown. Decides how P4b is set up. |
| D9 | Late-cancel penalty at TUBA: credit lost, a fee, or both? No-show the same? | Unknown. Decides P1 and the M5 cancel screen. |
| D12 | May staff book a waitlisted member over capacity at any time, or only when a spot is free? | Assumed: any time, after a confirmation. |
| D13 | Merch at the desk only, or also on the member booking site? | Desk only. |
| D14 | Keep the one-tap "Replace for this class" on the roster, or change teachers only through "Edit class"? | Keep it. |
| D15 | Several establishments: included for every studio, or a higher pricing tier? Albert, 21 Nov: not decided yet. | Build it for everyone; a tier only limits how many a studio may create, so the decision can wait. |
| D16 | With several establishments: are passes valid everywhere or per establishment, and do front desk staff see only their own? | Unknown. Assumed: passes valid everywhere, staff see all. |
| D17 | How many establishments and rooms does TUBA have today? | Unknown. |

---

## 5. Design phase

Designs are on a design canvas (an artifact), built with the Base tokens. Each batch is validated before the next starts.

Batch 1 canvas: https://claude.ai/artifact/D3uGzGLt9KQJD98PM9vMvh

| Batch | Screens | Why this order |
|---|---|---|
| 1. Desk and inbox | Shell and navigation, member search (S1), Today (S2), class roster (S3), member profile (S6) with pass actions (S7), inbox (S16) | The screens used every hour. If these are wrong, nothing else matters. **Drawn, awaiting validation.** |
| 2. Member | Timetable (M1), book and buy (M3, M4), my bookings and passes (M5, M6), sign-in (M2) | The other half of the daily loop. Phone first. |
| 3. Set-up | Schedule (S4, S5), catalog (S8), desk sale (S9), products and stock (S17). **Schedule, class edit form, desk sale and products list drawn early, on the Batch 1 canvas, awaiting validation.** Still to draw: class types, passes, subscriptions, promotion codes. | What makes the week bookable and sellable. |
| 4. Month end | Money (S10), payroll (S11), reports (S12), staff (S13), settings (S14), teacher view (S15) | Used weekly or monthly. |

Development starts on a batch only once its designs are validated.

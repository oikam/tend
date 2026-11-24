# Tend — feature definition, V1

**Status:** draft 5, for Albert to validate · **Date:** 2026-11-24 · synced with the design canvas (version 94)
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
| S2 | **Notifications** | Replaces the Today page (removed 22 Nov 2026). A bell in the header with a red count, a short panel and a full page. Two colours only: red for payment problems (amount shown), blue for everything else. A notification opens the thing it is about; it carries no buttons of its own. | J7 | Used (Home) |
| S3 | **Class roster** | Attendees with the pass used; no credits shown. Each member is Present or Absent, starting from the studio default; members still Absent when the roll call is confirmed are no-shows. Present and Absent counts appear only after the roll call. After the roll call a member can still be corrected (each change is recorded) or the roll call reopened. The waitlist sits under the booked list and shows only when the class is full: add, remove, book. When the class is over the waitlist closes: members still on it "did not get a place" and no credit is taken. A second view lists members no longer coming (cancelled, or removed from the waitlist). Edit the class, replace the teacher for this class, and per member: open profile, sell, email or call, move, change the pass used, note for the teacher, cancel with or without the credit. | J2, J1 | Used |
| S4 | **Schedule** | **Landing page.** Day view by default (a list of the day's classes with a recap row: classes, bookings, fill, waitlist); week view as a second tab. A week and month picker to jump to any date. A full class shows "Full · N waiting". Each class opens its roster or its edit form in one click. Create a class, one-off or repeating (repeat options as in Google Calendar, including a custom repeat). Edit one class, all similar classes, or chosen ones: the list of affected classes is shown, all ticked by default, untick to leave one out. Move or cancel a class, with the members affected shown first. Per class: type, level, teacher (always required: a class cannot exist without one), establishment and room, start and end, spots, waitlist spots, credits to book, hidden from members or not. Duplicate opens the new-class form filled in. | J5 | Used |
| S5 | **Teacher cover** | Put a substitute on one class. The usual teacher stays on the series. Payroll follows who taught. | J3 | Used (done as "change teacher") |
| S6 | **Member profile** | Header: name, contact, member since, classes attended, last class, next class. A red banner when a payment failed, with two actions: **Record a payment** (cash, card at the desk or bank transfer; the payment is added to the invoice, which is never edited; the automatic retry for that invoice stops; receipt and card-update link emailed, both ticked by default) and **Try the card again**. Tabs: **Overview** (passes, upcoming bookings, notes and documents), **Bookings**, **Billing** (payment methods, purchases with invoice or failure), **Communications** (emails and inbox requests), **Notes and documents**, **History**. Every booking, upcoming or past, has the same Present / Absent switch as the roster; other booking actions sit in one menu: move, change pass, open the class, cancel. Cancelling a class already attended asks whether the credit goes back. A late cancel shows its penalty ("Late cancel · € 10 fee") and can be waived (see P1). More actions: edit contact details, merge with another member, archive, export data, delete data. Sell and Book always in reach. | J1, J4 | Used |
| S7 | **Pass actions** | Pause, extend, cancel (now or at period end), give a credit back. Each asks for a reason and is recorded with who did it. | J4 | Used |
| S8 | **Catalog** | Class types (name, duration, capacity, booking window, cancellation deadline, which passes cover it). Passes (credits or unlimited, validity, price, tax, new clients only). Subscriptions (monthly price, renewal, commitment). Promotion codes. | — | Used |
| S9 | **Desk sale** | Sell a pass, a subscription or a product to a member: charge their saved card, send a payment link, or record cash / transfer / terminal. Reached from the profile, the roster row and the booking search. | J1, J8 | Used |
| S10 | **Money** | Payments list, invoices, refunds with a reason, failed payments, payouts. Monthly export for the accountant. | J8 | Used |
| S11 | **Payroll** | Rate per teacher per class type. Monthly run from classes taught, substitutes included. Export. | J8 | Used |
| S12 | **Reports** | Four only: revenue by product, bookings and fill by class, active members and subscriptions, unused credits. CSV export. | J8 | Used (subset of 30+) |
| S13 | **Staff and roles** | Invite by email. Four roles: Owner, Manager, Front desk, Teacher. Role change in a dialog; Owner changes are confirmed. | J6 | Used |
| S14 | **Settings** | One page each for: studio details, rooms, booking rules (including whether members start as Absent or Present), waitlist, invoices and tax, messages. | — | Used (22 pages there) |
| S15 | **Teacher view** | A teacher sees their own classes and rosters, and marks attendance. Phone first. | J2 | Used |
| S16 | **Inbox** | One shared queue for member requests. Emails to the studio address arrive in it and are answered from it. Requests from the desk, phone, WhatsApp and Instagram are logged by hand in a few seconds. Each request has an owner, a status (open, waiting, archived) and its age; open requests older than 24 hours raise a notification (S2). Replies have basic formatting (bold, italic, lists, links) and file attachments. The member's passes and bookings sit beside the conversation, so pause, extend, cancel and credit back (S7) are done without leaving it. Saved replies. Notes for staff. | J4 | Locked by plan |
| S19 | **Notes and documents** | Per member. Notes for staff (a note can be shown to teachers on the roster). Documents: waiver, medical certificate, prenatal clearance and the like, as PDF or photo, with an optional end date; restricted documents are visible to fewer roles; an expired document is shown in red. | J1 | Partly used |
| S17 | **Products and stock** | Merch sold on site, online (app and website), or both; set per product. A product has an image, a name, a price, a tax rate, a status (active or inactive; an inactive product cannot be sold anywhere) and a number in stock; sizes are separate products. A sale takes one off, a refund puts it back, staff correct the count with a reason. A product at zero can still be sold after a confirmation. Sales show on the member's purchases and invoices and as their own line in the revenue report. | J8 | Lightly used (one webshop item) |
| S18 | **Establishments and rooms** | A studio has one or more establishments (addresses), each with one or more rooms. Every class is in one room of one establishment. The schedule shows one establishment at a time; with a single establishment the choice is hidden. Decided 21 Nov 2026. | J5 | Used (Venues) |

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
| M7 | **Shop** | Buy merch on the app or website, paid by card, collected at the studio (no shipping). Shows only active products marked as sold online and in stock; an online order takes the item off the stock at once. Decided 24 Nov 2026. |

### Platform

| # | Feature | What it does |
|---|---|---|
| P1 | **Booking rules** | Booking window, cut-off, cancellation deadline, late-cancel penalty, no double booking, pass eligibility, waitlist promotion. No booking without a pass that covers the class: at the desk, staff sell first, then book. A credit is taken when the class is booked and returned on a cancel in time. When several passes cover a class, the pass is chosen at booking: preselected (unlimited subscription first, otherwise the pass that ends soonest) and changeable. Staff can change the pass used on a booking afterwards, also after the class, in one step that returns the credit to the first pass and takes it from the second. Each class has a number of credits needed to book it, taken from its class type and changeable per class, in steps of half a credit (0, 0.5, 1, 1.5 ...); a booking keeps what it was charged. One set of rules for staff and members. **Attendance and statuses:** a booking with a place is Present or Absent (Absent = no-show; the words "Booked" and "Attended" are not used); a waitlisted booking shows its rank; after the class it is Present, Absent or "Did not get a place". A cancelled booking is Cancelled (credit returned), Late cancel (penalty applied) or Class cancelled (credit returned). **Late-cancel penalty:** either a credit or a fee, never both (Albert, 23 Nov 2026). Staff can waive it, for example after a complaint: a credit goes back to the pass; a paid fee is refunded with a credit note, the original invoice unchanged. Every waiver asks for a free-text reason and can be linked to the member's inbox request. |
| P2 | **Payments** | Card payments, recurring subscription billing, retries on failure, booking block after repeated failure, refunds. On a failed payment the member is emailed a link to update the card automatically. Payments taken outside the card (cash, desk terminal, transfer) are recorded against the invoice. No partial payments in V1 (see D18). |
| P3 | **Invoices** | Sequential numbered invoices with VAT, credit notes on refund. |
| P4 | **Messages** | Outgoing email only. About 12 messages, not BSport's 63: account, booking confirmed / cancelled / reminder, waitlist offer, class cancelled or moved, receipt, payment failed, pass expiring, renewal. |
| P4b | **Incoming email** | Mail sent to the studio address is received, matched to a member by sender address, and threaded with replies. Needed by S16. |
| P5 | **Record of changes** | Who did what and when, for every money, pass, booking, attendance and role change. Staff give reasons as free text, never from a fixed list. Emails to the member that follow a staff action are ticked by default and can be unticked. |
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
| Gift cards, shipping merch, packs, instalments, expenses, purchase orders, barcodes | Not used. Merch with stock is in V1, on site (S17, 21 Nov 2026) and online with pickup at the studio (M7, 24 Nov 2026). |
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
| D9 | Late-cancel penalty at TUBA: credit lost or a fee? No-show the same? | Partly decided 23 Nov 2026: late cancel exists, and the penalty is one or the other, never both. Still unknown: which one TUBA uses, the deadline, and whether a no-show costs the same. |
| D12 | May staff book a waitlisted member over capacity at any time, or only when a spot is free? | Assumed: any time, after a confirmation. |
| D13 | Merch at the desk only, or also on the member booking site? | **Decided 24 Nov 2026 (Albert): both.** Each product is sold on site, online, or both. Open: how staff see and hand over online orders, and what happens when an order is not collected. |
| D14 | Keep the one-tap "Replace for this class" on the roster, or change teachers only through "Edit class"? | Keep it. |
| D15 | Several establishments: included for every studio, or a higher pricing tier? Albert, 21 Nov: not decided yet. | Build it for everyone; a tier only limits how many a studio may create, so the decision can wait. |
| D16 | With several establishments: are passes valid everywhere or per establishment, and do front desk staff see only their own? | Unknown. Assumed: passes valid everywhere, staff see all. |
| D17 | How many establishments and rooms does TUBA have today? | Unknown. |
| D18 | Do members keep an account balance (prepaid money, partial cash payments, refunds kept as credit)? | Unknown. Until decided: no balance, no partial payments, refunds go back to the original payment method. |
| D19 | Health documents: which roles may open restricted documents, and what happens when one expires (warning only, or booking blocked)? | Unknown. Assumed: owner and manager only; warning only. |
| D20 | Can staff notes in the inbox carry attachments too, or only replies to members? | Unknown. |

---

## 5. Design rules

Decided on the canvas, applied to every screen.

- **Buttons:** solid black for the one main action of a screen or pop-up; white with a grey outline for other actions; white with red text for an action that removes or cancels; solid red only to confirm inside an "are you sure" step.
- **Chosen option:** pale blue (background, blue outline, blue text). Never dark.
- **Present / Absent switch:** the same everywhere (roster and member profile): grey track, Present chosen = teal, Absent chosen = white with a grey outline.
- **Row actions:** at most one visible action per row; the rest go in a "⋯" menu.
- **Pop-ups:** a close cross top right; height follows the content.
- **Notifications:** red for payments, blue for everything else.

## 6. Design phase

Designs are on a design canvas (an artifact), built with the Base tokens. Each batch is validated before the next starts.

Batch 1 canvas: https://claude.ai/artifact/D3uGzGLt9KQJD98PM9vMvh

| Batch | Screens | Why this order |
|---|---|---|
| 1. Desk and inbox | Shell and navigation, member search (S1), notifications (S2), schedule (S4, landing), class roster before and after the roll call (S3), member profile with its tabs, booking menu, cancel, waive penalty and record a payment (S6), pass actions (S7), inbox (S16), notes and documents (S19) | The screens used every hour. If these are wrong, nothing else matters. **Drawn, being reviewed with Albert.** |
| 2. Member | Timetable (M1), book and buy (M3, M4), my bookings and passes (M5, M6), shop (M7), sign-in (M2) | The other half of the daily loop. Phone first. |
| 3. Set-up | Class edit and new class with repeat (S4, S5), catalog (S8), desk sale (S9), products and stock (S17). **Class forms, desk sale and products list drawn early, on the Batch 1 canvas.** Still to draw: change-pass pop-up, class types, passes, subscriptions, promotion codes. | What makes the week bookable and sellable. |
| 4. Month end | Money (S10), payroll (S11), reports (S12), staff (S13), settings (S14), teacher view (S15) | Used weekly or monthly. |

Development starts on a batch only once its designs are validated.

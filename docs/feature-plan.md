# Team Rap: Feature Set Plan

Status: **Draft for review** (2026-10-29). Updated with the [pain-points research](../research/teamsnap-pain-points.md); audience decided: **youth teams first**; launch sport: **alpine ski racing clubs**. Restructured 2026-10-30 into a release schedule (R1 to R7), with the mobile app as its own release, R2.

Team Rap is a team management app in the same space as TeamSnap, and it aims to be clearly better. Release 1 is a lean web MVP that replaces TeamSnap's core, and later releases add the rest. A native mobile app comes later, so the architecture is API-first from day one.

---

## 1. What TeamSnap does today

| Area | TeamSnap capability |
|---|---|
| Scheduling | Games, practices, events, calendar subscribe/sync |
| Availability | RSVP per event, availability tracking (partly behind the paid tier) |
| Roster | Players, contacts, family members, roster size limits on free tier |
| Communication | Team chat, email/push alerts, announcements |
| Assignments | Snacks, volunteers, other duties |
| Coaching | Drills and practice plans (pro drills on paid tier) |
| Media | Photo storage |
| Clubs & leagues | Registration and payments, org-wide comms, parent app, live streaming, health checks |
| Pricing | Free tier for single teams; paid upgrade; clubs/leagues need a sales call (no public pricing) |

## 2. What users complain about

These come from app store reviews, review sites, forums, and a court filing. The full report, with quotes and frequency and severity ratings, is in `/mnt/project-files/research/teamsnap-pain-points.md`.

1. **Ads in the app**, including ads during RSVP flows ("advertisements on every click").
2. **Unreliable notifications.** Some get alerts "approximately 30% of the time" and miss cancellations.
3. **Too much notification noise.** One change triggers several emails plus a push.
4. **Sync bugs.** Edits on web don't show in the app; RSVPs don't refresh.
5. **Paywalled basics.** Availability tracking, roster size, and simple branding cost extra. "Basic customization should be free."
6. **Confusing navigation** after redesigns, and multiple accounts per team or child.
7. **Feature parity gaps.** For example, attachments only work from a computer.
8. **Opaque pricing** for clubs and leagues (sales process only), which puts off small rec leagues.
9. **Enterprise drift.** The product grows for big orgs and gets heavier for a single volunteer coach.
10. **Junk fees and billing traps.** Processing and "convenience" fees show up only at checkout (subject of the 2024 *Jovel v. TeamSnap* class action, which is still only allegations). Other problems are surprise yearly charges, cancellation that's hard to do, and a whole club locked out with no warning when a card expires.
11. **Support you can't reach.** There's no phone line, help is mostly bots, and email replies are slow or never come.
12. **Forced migrations that take features away.** TeamSnap ONE (2025–26) launched without the web portal, recurring-series edits, bulk edits, group email, and CSV import.
13. **Scattered messaging that's missing basics.** There's no reply-all, members can't be removed from a chat, and there are no threads, pins, or admin-only announcements, so teams move to WhatsApp.
14. **Accounts that won't go away.** There are duplicate accounts per child, old teams that can't be hidden, and account deletion that loops forever.
15. **People forced onto an app they didn't choose.** Parents who are reluctant or don't have a smartphone don't RSVP, and coaches struggle to load rosters and get families signed up.
16. **Spam.** Families are sent marketing and "prize survey" emails after a team invites them.
17. **Thin calendar feeds.** Feeds lag, leave out arrival time and uniform, and get time zones wrong.

The root cause behind many of these is that **the club is TeamSnap's customer and families are forced to use it.** Team Rap has to win over the people who never chose it.

## 3. Where Team Rap does better (product principles)

| # | Principle | Answer to complaint |
|---|---|---|
| P1 | **No ads, ever.** Not on any plan, and never inside core flows like RSVP. | 1 |
| P2 | **Notifications you can trust.** Delivery is tracked per recipient, with a fallback channel when push fails. Organizers can see who hasn't seen a cancellation. | 2 |
| P3 | **One change, one alert.** Smart batching, a daily digest option, quiet hours, and per-team and per-channel preferences. | 3 |
| P4 | **Real-time by default.** Everything syncs live across tabs and devices, with no stale RSVPs. | 4 |
| P5 | **The core is free.** Scheduling, RSVP and availability, roster (no small cap), chat, and basic branding (colors and logo) are free for a team. | 5 |
| P6 | **One family account.** One login covers several kids, teams, and sports, with a single merged calendar. | 6 |
| P7 | **Zero-friction RSVP.** Reply "yes" or "no" from an email or SMS link without logging in or installing anything. | 2, 6 |
| P8 | **Same features on every device.** The web app is mobile-first and responsive, and the native app gets the same features. | 7 |
| P9 | **Public, simple pricing.** No sales call needed for a small league. | 8 |
| P10 | **Built for volunteer coaches and club admins first.** A club is set up in under 30 minutes, and org features stay out of the way until you need them. | 9, 15 |
| P11 | **No junk fees, no traps.** The price shown is the price paid, with any fee shown up front. Cancel in one click, choose monthly or seasonal billing, get a warning and a grace period before any lapse, and get prorated refunds. | 10 |
| P12 | **Support from a real person.** A person replies within one business day through async chat in the app, and a public status page shows outages. | 11 |
| P13 | **Never take features away.** Changes are opt-in, every feature works on both web and mobile, and teams can export their data anytime. | 12 |
| P14 | **Everyone can participate without the app.** Members who use only SMS or email get alerts and can RSVP like anyone else. | 15 |
| P15 | **Your data is yours.** No marketing to invited members, youth data is never sold or shared, and anyone can delete their own account. | 14, 16 |

## 4. Release schedule

### Launch sport: alpine ski racing (decided 2026-10-29)

Shawn's daughter races, and her club is why Shawn dislikes TeamSnap. A ski club works differently from a team that plays games:

- **The unit is a club, not a team.** One club runs training groups by age class (U10, U12, U14, U16, U19/FIS). Coaches move between groups, and siblings are often in different groups.
- **Training is the everyday event.** Sessions happen on the hill, in dryland, or at camps, and plans change at 6 a.m. because of weather.
- **Races are external.** Athletes enter through [adminskiracing.com](https://adminskiracing.com) or [Zone4](https://zone4.ca) with a US Ski & Snowboard license. Results go on [live-timing.com](https://live-timing.com).
- **Race weekends are trips,** with shared condos and carpools, and **volunteer hours are required** of every family.

The core data model is sport-neutral (organization, then groups, then members, then events), and ski racing is the first sport *profile* on top of it.

### How releases work

- **Each release ships on its own and is usable as-is.** No release depends on a later one to be worth using.
- **Each release has a goal and an exit test.** The next release starts once the exit test passes, so scope doesn't grow mid-release.
- **The principles in section 3 apply from Release 1.** Most cost nothing (no ads, no spam, account deletion). Ones that are expensive, such as the support promise and SMS, scale up with later releases.
- **Release 1 is web only (an installable PWA).** The native app is its own release, R2, and every release after it ships on web and mobile together.
- **Timing is a proposal.** It's tied to the ski calendar (season roughly December to April), not to fixed dates. Shawn to confirm.

| Release | Theme | Proposed timing |
|---|---|---|
| **R1** | True MVP: replace TeamSnap's core | Build over winter 2026–27, pilot with one club late in the 2026–27 season |
| **R2** | Mobile apps (iOS and Android) | Spring to summer 2027, ready before the 2027–28 season |
| **R3** | Ski club essentials | Start of the 2027–28 season (fall 2027) |
| **R4** | Payments and Pro | During the 2027–28 season |
| **R5** | Club admin and registration | Before 2028–29 registration opens (summer 2028) |
| **R6** | Coaching and training | 2028–29 season |
| **R7** | Expansion | 2029 onward |

---

### R1: True MVP, replacing TeamSnap's core (web)

**Goal:** a ski club can drop TeamSnap for day-to-day use: who's in which group, what's on, who's coming, and what changed.

**In scope**
- **Club and groups:** one club with training groups, coaches assigned to groups, and a few admins
- **Roster:** athletes and their guardians, contact info hidden from other families by default, and bulk import from CSV or a pasted list
- **Accounts:** email magic link with a code fallback, one family login for several kids, and invites by link or email. Self-serve account deletion
- **Schedule:** training sessions, races, and other events for each group or the whole club, recurring sessions with edit for one session or the whole series, and location with a map link
- **Cancel or change alert:** one action notifies every affected group, as high-priority
- **Availability:** Yes / No / Maybe per event, one-tap reply from the email link without logging in, and a coach view of who hasn't answered
- **Messaging:** club and group announcements that only coaches and admins can post, plus simple group chat
- **Who can message (club setting):** by default, only parents/guardians, coaches, and admins can chat, which matches how clubs like NASA run TeamSnap today. Athletes can still read announcements and see their schedule. A club can turn on athlete chat per group, for example for U16 and older, and decide whether athletes chat only in coach-supervised group chats. Athletes can never send private one-to-one messages to adults, which follows SafeSport-style youth messaging rules
- **Notifications:** email and web push, per-user preferences, and alerts that can't be muted for cancellations
- **Calendar:** ICS feed per group and a merged feed per family
- **No ads,** and no marketing email to invited members

**Deliberately left out of R1:** SMS, race entry tracking, trips and lodging, volunteer hours, payments, native apps, media, stats, and anything for coaching.

**Exit test:** one pilot club runs a month or more of the season on Team Rap without falling back to TeamSnap, and parents rate it at least as easy to use.

### R2: Mobile apps

**Goal:** native iOS and Android apps with every R1 feature, so families who expect an app (as with TeamSnap) aren't asked to use a web app.

- **iOS and Android apps** (React Native / Expo) built on the same API as the web app, with the same features as R1 (P8)
- **Native push** with delivery tracking, falling back to email
- Sign in with Google or Apple, and stay signed in
- Calendar and share-sheet integration (add an event to the phone calendar, share a session)
- Offline read of the schedule and roster, for spotty signal at the ski hill
- App Store and Google Play listings, with no ads and no in-app upsell to parents

**Exit test:** most active families at the pilot club use the app, and nothing in R1 is web-only.

### R3: Ski club essentials

**Goal:** handle what TeamSnap doesn't do for ski clubs, which is races, trips, and volunteers.

- **Race calendar:** add races from an adminskiracing or Zone4 link, with entry deadline, disciplines, and eligible age classes
- **"Who's racing" tracker:** families mark "planning to race" and then "entered," with reminders before the official deadline. Entry itself still happens on the official site
- **Trips:** race weekends and camps, with lodging and room assignments, carpools, chaperones, and packing lists
- **Volunteers:** jobs, sign-ups, and a running hours total per family against the hours owed
- **Athlete profile:** age class computed from birth year, USSS license number with an expiry reminder, and emergency and medical notes visible only to coaches
- **Messaging upgrades:** race and trip chats, attachments, pins, and reply-by-email
- **SMS** for alerts and RSVP, with a small free allowance (start of the Pro tier)
- **Delivery tracking** with SMS fallback for critical alerts

**Exit test:** the pilot club runs its race weekends and volunteer tracking on Team Rap, and a second club signs on.

### R4: Payments and Pro

**Goal:** the first revenue.

- **Payments:** trip and lodging cost splits, race-fee reimbursements, and club dues, with any fee shown before checkout (Stripe Connect)
- **Pro tier launch:** SMS beyond the free allowance, plus extra storage

**Exit test:** payments are moving through the app at the pilot clubs.

### R5: Club admin and registration

**Goal:** the club runs its season sign-up on Team Rap.

- Season registration for programs, with waivers, e-signatures, and downloadable PDFs
- Payment plans and sibling discounts
- Season rollover: move athletes up an age class in bulk, and clone groups
- **Results import** from live-timing.com and USSS points lists into each athlete's history
- Equipment: tuning log, team gear orders, and a ski swap board
- Compliance tracking for coach certifications and SafeSport

### R6: Coaching and training

**Goal:** go beyond logistics and help athletes improve.

- **Coaching videos:** a library of drills and technique videos by discipline and age class
- **Training helper:** training plans per group, session plans, and suggested progressions
- **Video analysis:** upload clips tagged to an athlete, with coach comments and side-by-side comparison
- Athlete goals, coach notes, and progress over the season
- **AI coach assistant:** answers questions like "who's eligible and entered for Saturday's GS?", and drafts training plans and conditions updates (Pro)

### R7: Expansion

- More sport profiles: Nordic, freestyle and snowboard, other club sports, then ordinary team sports
- Tools for clubs that host races (registration, start lists, bibs), only if demand is confirmed
- Regional and multi-club race series views, with published pricing for regions
- Canada and Europe (Alpine Canada, FIS), localization, and multi-currency
- Opt-in fundraising and sponsor pages

## 5. Monetization (decided 2026-10-29)

- **R1–R3:** everything is free, with no ads. The goal is adoption. SMS has a small free allowance starting in R3.
- **R4, payments (main revenue):** a transparent fee on dues and other team payments, at or below Spond's US rate ($1.00 + 3.29%). The team chooses whether it absorbs the fee or passes it to families, and the fee is always shown before checkout (P11).
- **R4 onward, Pro tier (about $5–9 per team per month, or about $49–79 a year; for ski clubs, priced per training group or per club, still to be set):** only for features that cost Team Rap real money, such as SMS beyond the free allowance, the AI coach assistant, extra storage, and advanced stats. RSVP, availability, roster, chat, and branding stay free forever (P5). Billing is monthly or seasonal, and cancelling takes one click.
- **R5 onward, club plans:** published per-team pricing with no sales call (P9), plus registration fees under the same fee-transparency rule.
- **Later, optional:** opt-in sponsorship and fundraising pages, never inside core flows and never targeting youth data.

Source: section 8 of `/mnt/project-files/research/market-research.md`.

## 6. Architecture direction (leaves room for mobile)

Recommendation:

- **API-first backend** where every feature is an API the web app consumes, so the mobile app reuses it unchanged. TypeScript end to end (Node, with a typed API layer such as tRPC or REST + OpenAPI).
- **Web app** built with Next.js (React) as a responsive PWA.
- **Mobile later** with React Native / Expo, sharing types, validation, and API client packages in a monorepo (`apps/web`, `apps/mobile`, `packages/api`, `packages/shared`).
- **Postgres** as the main database. The data model is centered on households, teams, seasons, and events.
- **Real-time** over WebSockets for chat, RSVP, and schedule updates (P4).
- **Notification service** behind one interface that covers email, SMS, web push, and later APNs/FCM, with per-recipient delivery tracking (P2, P3).
- **Auth** with token-based sessions that work for both web and native clients.
- **Privacy** for youth data by design: COPPA awareness, minimal data on minors, and guardian-controlled visibility.

## 7. Open questions for Shawn

1. ~~**Target audience**~~ **Decided 2026-10-29: youth teams first.** Parents and guardians are the primary users, so guardian accounts, privacy for minors, and snack/carpool assignments get MVP polish first. Adult rec leagues follow.
2. ~~**Monetization**~~ **Decided 2026-10-29:** a payments margin plus a small Pro tier, with league plans later. See section 5.
3. ~~**SMS**~~ **Mostly decided:** email and push are free, and SMS is in Pro, with a small free allowance per team. The exact cap is still to be set.
4. ~~**Sports focus**~~ **Decided 2026-10-29:** alpine ski racing clubs first, on a sport-neutral core.
   - **What breaks in TeamSnap for your daughter's club?** Your specific pain points will set what makes the cut for R1.
   - **Release timing:** is a late 2026–27 season pilot for R1 realistic, and which club would pilot it?
   - **Pro pricing unit for clubs:** per training group, per athlete, or a flat price per club?
5. **Geography:** US-only at launch, or international?
6. **Support commitment:** P12 promises a person replies within one business day. Can you staff that at launch, or should the promise start smaller?
7. **Tech stack:** are you OK with the TypeScript / Next.js / Postgres direction, or do you prefer something else?

## Sources

- [Wintergreen ski race parents guide](https://www.wintergreenresort.com/wp-content/uploads/2023/05/Ski_Race_Parents_Guide.pdf)
- [How your ski club pulls off a training day (Ski Racing)](https://skiracing.com/how-your-ski-club-pulls-off-a-training-day/)
- [Example race entry: U14+ Open Series on adminskiracing.com](https://adminskiracing.com/event/223)

- [TeamSnap pain-points research report](../research/teamsnap-pain-points.md), which has the full source list

- [TeamSnap pricing page](https://teamsnap.com/pricing)
- [TeamSnap app reviews (JustUseApp)](https://justuseapp.com/en/app/393048976/teamsnap/reviews)
- [TeamSnap reviews on G2](https://www.g2.com/products/TeamSnap/reviews)
- [TeamSnap on Capterra](https://www.capterra.com/p/123208/teamsnap/)
- [Best TeamSnap alternatives (BenchApp blog)](https://stg.benchapp.com/blog/best-teamsnap-alternatives)

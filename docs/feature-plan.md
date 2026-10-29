# Team Rap: Feature Set Plan

Status: **Draft for review** (2026-10-29). Updated with the [pain-points research](../research/teamsnap-pain-points.md); audience decided: **youth teams first**; launch sport: **alpine ski racing clubs**.

Team Rap is a team management app in the same space as TeamSnap, and it aims to be clearly better. The MVP is a web app. A native mobile app comes later, so the architecture is API-first from day one.

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

## 4. Phased scope

### Launch sport: alpine ski racing (decided 2026-10-29)

Shawn's daughter races, and her club is why Shawn dislikes TeamSnap. TeamSnap is built around a team that plays games against opponents. A ski club works differently:

- **The unit is a club, not a team.** One club runs several programs and training groups by age class (U10, U12, U14, U16, U19/FIS, plus devo and masters). Coaches move between groups, and siblings are often in different groups.
- **Training is the everyday event.** Sessions happen on the hill (venue, discipline such as SL, GS, SG, or free skiing, and lane), in dryland, or at camps. Plans change at 6 a.m. because of weather, snow, or hill-space changes.
- **Races are external.** Each athlete enters through the race organizer, usually on [adminskiracing.com](https://adminskiracing.com) or [Zone4](https://zone4.ca), with a US Ski & Snowboard license and a regional membership. Deadlines are strict and late fees are high. Start lists and results go on [live-timing.com](https://live-timing.com). Race documents often go out over WhatsApp.
- **Race weekends are trips.** They involve multi-night travel, shared condos, carpools, chaperones, coach assignments, bib pickup, and arriving about 90 minutes before the start.
- **Volunteering is required.** Families owe course crew and race-worker hours each season.
- **Age class comes from birth year** (age as of December 31), and it changes every season.

So the MVP is built around **a club with training groups**, and club features move from Phase 3 into the MVP. The core data model stays sport-neutral (organization, then groups, then members, then events with types), and ski racing is the first sport *profile* on top of it. That lets other club sports (swimming, Nordic, gymnastics) or ordinary teams come later without a rewrite.

### Phase 1: MVP for ski clubs (web, installable PWA)

The goal is that a ski club can run a full winter season on Team Rap, and parents would rather use it than TeamSnap plus WhatsApp plus spreadsheets.

**Club, programs & groups**
- Club workspace with branding (free, P5), plus a set of programs and training groups for each season
- Athletes are placed in groups. Age class is computed from birth year, and a new season can roll athletes up a class in bulk
- Coaches are assigned to one or more groups, and there are several admins and schedule editors
- Bulk import of athletes and families from CSV or a pasted list, and CSV export that imports back cleanly (P13)
- Archive a season and clone its structure for the next one

**Accounts & identity**
- Sign in with an email magic link, or with Google or Apple. Sign-in falls back to a one-time code when a link expires
- Family/household accounts: one login covers every child in every group and handles more than one guardian (P6)
- Roles: club admin, head coach, coach, athlete (teen accounts optional), guardian, and read-only
- Invite by link, email, or SMS. SMS-only and email-only members are full participants (P7, P14)
- Self-serve account deletion and data export (P15). Opens to the current season
- Athlete profile: emergency contacts and medical notes visible only to coaches. USSS license number and expiry date, with a reminder before it expires. Regional membership

**Training schedule**
- Training sessions per group: on-hill (venue, discipline, meeting spot, lane), dryland, and camps
- Recurring sessions, with edit or delete for one session or the whole series
- **Conditions update:** one tap sends "on, delayed, moved, or cancelled" to every affected group at once, as a high-priority alert with delivery tracking (P2). Made for 6 a.m. weather calls
- Attendance and availability per session, with one-tap RSVP from email or SMS (P7) and nudges to people who haven't answered
- Merged family calendar and ICS feeds that include meeting spot, discipline, and what to bring

**Race calendar & entries**
- A club race calendar with each race's date, venue, disciplines, eligible age classes, entry deadline, and a link to the official entry page. Races can be added from a link to an adminskiracing or Zone4 event or from an iCal file, or entered by hand
- **"Who's racing" tracker.** Each family marks "planning to race" and then "entered" for each athlete. Coaches see who is eligible, who is planning to race, and who has actually entered, and the app reminds people before the official deadline. Entry itself happens on the official site; Team Rap doesn't replace the race organizer in the MVP
- Coach assignments per race, and bib pickup and captains' meeting reminders
- Results links to live-timing.com on each race

**Trips & logistics**
- A trip for each race weekend or camp, covering dates, departure, travel plan, and chaperones
- Lodging: shared condos with room assignments and cost-share notes. Payment collection comes in Phase 2
- Carpool sign-up and swap
- A packing and equipment checklist for each trip (skis tuned, helmet, race suit, and so on)

**Volunteers**
- The club sets volunteer jobs (course crew, gatekeeper, timing, hospitality) and the hours each family owes
- Sign-up per race or event, a running hours total per family, and reminders to families who are behind

**Communication**
- Club-wide and per-group admin-only announcement channels. Replies to emailed announcements come back into the thread, and reply-all works
- Group, trip, and race chats with member management, pins, and reactions (P4). Race documents are attached in the chat instead of sent over WhatsApp
- One inbox for announcements, chat, and alerts. Per-user preferences, quiet hours, and a digest (P3), with critical alerts like cancellations always delivered
- Delivery tracking with email or SMS fallback (P2)

**Platform**
- Mobile-first responsive UI, installable as a PWA with web push. Readable in the cold, on a phone, with gloves on: big tap targets and high contrast
- No ads (P1), and no marketing email to invited members (P15)
- Support from a person through async in-app chat, and a public status page (P12)

**Explicitly not in the MVP:** payments, club registration, entering races directly, live timing or results import, video, and native apps.

### Phase 2: Mobile and the money

- **Native iOS and Android apps** (shared codebase), with the same features as web (P8)
- **Payments**: program fees, trip and lodging cost splits, race-fee reimbursements, and dues, with a ledger per family (Stripe Connect). Follows P11, so any fee is shown before checkout
- **Club registration**: season sign-up for programs, waivers and e-signatures with downloadable PDFs, payment plans, and sibling discounts
- **Results import**: pull each athlete's results from live-timing.com and USSS points lists into the profile, with a season history
- **Files & media**: bulk photo upload and training video clips tagged to an athlete (on Pro if storage costs are high)
- **Coach tools**: training plans per group, per-athlete notes and goals, and attendance reports
- **Equipment**: tuning log, team gear orders, and a ski swap board

### Phase 3: Beyond the first ski clubs

- **Race organizer tools**: host-club race registration with license checks, fees, start lists, and bib management. This competes with adminskiracing, so first confirm the demand
- Regional and division views: multi-club race series calendars, published pricing for regions (P9)
- Integrations with US Ski & Snowboard license verification (if an API or partnership exists), plus Google and Outlook two-way calendar
- More sport profiles: Nordic, freestyle and snowboard, other club sports, then ordinary team sports (games, opponents, lineups)
- Compliance tracking: SafeSport and coach certification expiry
- Self-serve trial for clubs, so what buyers sign up for matches the demo

### Later ideas (unscheduled)

- AI assistant for coaches: "who's eligible and entered for Saturday's GS?", drafting trip plans and conditions updates
- Carpool and condo matching by location
- Fundraising and sponsor pages (opt-in, never ads in core flows)
- Canada and Europe support (FIS, Alpine Canada), localization, and multi-currency

## 5. Monetization (decided 2026-10-29)

- **MVP (Phase 1):** everything is free, with no ads. The goal is adoption.
- **Phase 2, payments (main revenue):** a transparent fee on dues and other team payments, at or below Spond's US rate ($1.00 + 3.29%). The team chooses whether it absorbs the fee or passes it to families, and the fee is always shown before checkout (P11).
- **Phase 2, Pro tier (about $5–9 per team per month, or about $49–79 a year; for ski clubs, priced per training group or per club, still to be set):** only for features that cost Team Rap real money, such as SMS beyond the free allowance, the AI coach assistant, extra storage, and advanced stats. RSVP, availability, roster, chat, and branding stay free forever (P5). Billing is monthly or seasonal, and cancelling takes one click.
- **Phase 3, league and club plans:** published per-team pricing with no sales call (P9), plus registration fees under the same fee-transparency rule.
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
   - **What breaks in TeamSnap for your daughter's club?** Your specific pain points will set MVP priorities.
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

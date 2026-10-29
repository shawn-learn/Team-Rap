# Team Rap: Feature Set Plan

Status: **Draft for review** (2026-10-29). Updated with the [pain-points research](../research/teamsnap-pain-points.md); audience decided: **youth teams first**.

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
| P10 | **Built for the volunteer coach first.** A team is set up in under 5 minutes, and org features stay out of the way until you need them. | 9, 15 |
| P11 | **No junk fees, no traps.** The price shown is the price paid, with any fee shown up front. Cancel in one click, choose monthly or seasonal billing, get a warning and a grace period before any lapse, and get prorated refunds. | 10 |
| P12 | **Support from a real person.** A person replies within one business day through async chat in the app, and a public status page shows outages. | 11 |
| P13 | **Never take features away.** Changes are opt-in, every feature works on both web and mobile, and teams can export their data anytime. | 12 |
| P14 | **Everyone can participate without the app.** Members who use only SMS or email get alerts and can RSVP like anyone else. | 15 |
| P15 | **Your data is yours.** No marketing to invited members, youth data is never sold or shared, and anyone can delete their own account. | 14, 16 |

## 4. Phased scope

### Phase 1: MVP (web, installable PWA)

The goal is that a volunteer coach can run a whole season on Team Rap and parents would rather use it than the group text.

**Accounts & identity**
- Sign in with an email magic link, or with Google or Apple
- Family/household accounts: one guardian manages several players, and a player can have more than one guardian
- Roles per team: owner/manager, coach, player, guardian, and fan (read-only)
- Invite by link, email, or SMS. Invitees can RSVP before creating an account (P7)
- SMS-only and email-only members are full participants who need no app (P14)
- Sign-in falls back to a one-time code when a magic link has expired
- Self-serve account deletion and data export (P15). Hide or archive past teams from your own view
- Opens to the current season's team, and remembers the last team you used

**Teams & roster**
- Create a team with sport, season, colors, and logo (free branding, P5)
- Roster with jersey number, positions, and guardian contacts
- Bulk roster import from CSV or a pasted list, and CSV export that imports back cleanly (P13)
- Privacy controls on contact info, which matters for youth teams. Minors' contact details are hidden by default
- Archive a season, then clone it for the next one

**Schedule**
- Games, practices, and generic events, including recurring practices, with edit or delete for one event or the whole series
- Locations with a map link, field or court notes, and arrival time
- Home/away, opponent, and uniform color
- Cancel or reschedule, which triggers a high-priority alert (P2)
- ICS calendar feed per team and a merged feed per family. Feeds include arrival time, uniform, home/away, location notes, and snack duty, and update quickly
- Times are stored with the venue's time zone

**Availability / RSVP**
- Yes / No / Maybe with an optional note, per event
- One-tap RSVP from the email or SMS link (P7)
- Polite automatic reminders that keep going until the person answers ("RSVP missing"), with a configurable lead time
- Coach view showing headcount by position and who hasn't answered

**Communication**
- Admin-only announcement channel (one-way from managers and coaches). Replies to emailed announcements come back into the thread, and reply-all works
- Team chat with real-time updates (P4), several named chats per team, adding and removing members, pinned messages, and reactions
- One inbox that merges announcements, chat, and alerts
- Notification center with per-user preferences, quiet hours, and a daily digest (P3). Noisy alert types can be muted without muting critical ones like cancellations
- Delivery tracking with email or SMS fallback when push isn't confirmed (P2)

**Assignments**
- Snacks, carpools, and volunteer slots per event, with sign-up and swap

**Platform**
- Mobile-first responsive UI, installable as a PWA with web push
- No ads (P1), and no marketing email to invited members (P15)
- Support: async in-app chat with a person, and a public status page (P12)

**Explicitly not in the MVP:** payments, registration, stats, media galleries, the league scheduler, and native apps.

### Phase 2: Mobile and the money

- **Native iOS and Android apps** (shared codebase), with the same features as web (P8)
- **Payments**: team dues, fee splitting, payment reminders, and a ledger (Stripe Connect). Follows P11, so any fee is shown before checkout
- **Files & photos**: attachments from any device, shared team albums, and bulk photo upload
- **Game day**: lineup and rotation builder (with equal-playing-time helper for youth), live score updates, simple stats
- **Attendance reports** across the season
- **Multi-team dashboard** for coaches and parents with several teams

### Phase 3: Clubs & leagues

- Online registration forms, waivers and e-signatures, with discounts and payment plans. Admins can download signed waivers as PDFs. Pricing is all-in, with no surprise fees at checkout (P11)
- Team-level invoicing, not only family by family
- Roster players on and off for tournaments and seasons, and keep one athlete profile across seasons with de-duplication
- Several schedule editors per team
- A self-serve trial, so what buyers sign up for matches the demo
- Org admin console: manage many teams, staff, and org-wide comms
- **League scheduler**: auto-generate schedules around field availability, blackout dates, and conflicts
- Public team and league pages (schedule, standings, results)
- Compliance integrations: background checks, certifications, concussion and safety training tracking
- Tournaments and brackets
- Integrations: Google and Outlook two-way calendar, live streaming partners, accounting exports

### Later ideas (unscheduled)

- AI assistant for coaches: practice plans, "who can play Saturday?", schedule-change drafting
- Carpool matching by location
- Fundraising and sponsor pages (opt-in, never ads in core flows)
- Localization and multi-currency

## 5. Monetization (decided 2026-10-29)

- **MVP (Phase 1):** everything is free, with no ads. The goal is adoption.
- **Phase 2, payments (main revenue):** a transparent fee on dues and other team payments, at or below Spond's US rate ($1.00 + 3.29%). The team chooses whether it absorbs the fee or passes it to families, and the fee is always shown before checkout (P11).
- **Phase 2, Pro tier (about $5–9 per team per month, or about $49–79 a year):** only for features that cost Team Rap real money, such as SMS beyond the free allowance, the AI coach assistant, extra storage, and advanced stats. RSVP, availability, roster, chat, and branding stay free forever (P5). Billing is monthly or seasonal, and cancelling takes one click.
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
4. **Sports focus:** should the MVP be sport-agnostic, or tuned to one or two sports first?
5. **Geography:** US-only at launch, or international?
6. **Support commitment:** P12 promises a person replies within one business day. Can you staff that at launch, or should the promise start smaller?
7. **Tech stack:** are you OK with the TypeScript / Next.js / Postgres direction, or do you prefer something else?

## Sources

- [TeamSnap pain-points research report](../research/teamsnap-pain-points.md), which has the full source list

- [TeamSnap pricing page](https://teamsnap.com/pricing)
- [TeamSnap app reviews (JustUseApp)](https://justuseapp.com/en/app/393048976/teamsnap/reviews)
- [TeamSnap reviews on G2](https://www.g2.com/products/TeamSnap/reviews)
- [TeamSnap on Capterra](https://www.capterra.com/p/123208/teamsnap/)
- [Best TeamSnap alternatives (BenchApp blog)](https://stg.benchapp.com/blog/best-teamsnap-alternatives)

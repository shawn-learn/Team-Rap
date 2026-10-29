# Team Rap: Feature Set Plan

Status: **Draft for review** (2026-10-29)

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

These come from app store reviews and review sites (sources at the bottom).

1. **Ads in the app**, including ads during RSVP flows ("advertisements on every click").
2. **Unreliable notifications.** Some get alerts "approximately 30% of the time" and miss cancellations.
3. **Too much notification noise.** One change triggers several emails plus a push.
4. **Sync bugs.** Edits on web don't show in the app; RSVPs don't refresh.
5. **Paywalled basics.** Availability tracking, roster size, and simple branding cost extra. "Basic customization should be free."
6. **Confusing navigation** after redesigns, and multiple accounts per team or child.
7. **Feature parity gaps.** For example, attachments only work from a computer.
8. **Opaque pricing** for clubs and leagues (sales process only), which puts off small rec leagues.
9. **Enterprise drift.** The product grows for big orgs and gets heavier for a single volunteer coach.

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
| P10 | **Built for the volunteer coach first.** A team is set up in under 5 minutes, and org features stay out of the way until you need them. | 9 |

## 4. Phased scope

### Phase 1: MVP (web, installable PWA)

The goal is that a volunteer coach can run a whole season on Team Rap and parents would rather use it than the group text.

**Accounts & identity**
- Sign in with an email magic link, or with Google or Apple
- Family/household accounts: one guardian manages several players, and a player can have more than one guardian
- Roles per team: owner/manager, coach, player, guardian, and fan (read-only)
- Invite by link, email, or SMS. Invitees can RSVP before creating an account (P7)

**Teams & roster**
- Create a team with sport, season, colors, and logo (free branding, P5)
- Roster with jersey number, positions, and guardian contacts
- Privacy controls on contact info, which matters for youth teams. Minors' contact details are hidden by default
- Archive a season, then clone it for the next one

**Schedule**
- Games, practices, and generic events, including recurring practices
- Locations with a map link, field or court notes, and arrival time
- Home/away, opponent, and uniform color
- Cancel or reschedule, which triggers a high-priority alert (P2)
- ICS calendar feed per team and a merged feed per family

**Availability / RSVP**
- Yes / No / Maybe with an optional note, per event
- One-tap RSVP from the email or SMS link (P7)
- Automatic reminders to people who haven't answered, with a configurable lead time
- Coach view showing headcount by position and who hasn't answered

**Communication**
- Team announcements (one-way from managers and coaches)
- Team chat with real-time updates (P4)
- Notification center with per-user preferences, quiet hours, and a daily digest (P3)
- Delivery tracking with email or SMS fallback when push isn't confirmed (P2)

**Assignments**
- Snacks, carpools, and volunteer slots per event, with sign-up and swap

**Platform**
- Mobile-first responsive UI, installable as a PWA with web push
- No ads (P1)

**Explicitly not in the MVP:** payments, registration, stats, media galleries, the league scheduler, and native apps.

### Phase 2: Mobile and the money

- **Native iOS and Android apps** (shared codebase), with the same features as web (P8)
- **Payments**: team dues, fee splitting, payment reminders, and a ledger (Stripe Connect)
- **Files & photos**: attachments from any device and shared team albums
- **Game day**: lineup and rotation builder (with equal-playing-time helper for youth), live score updates, simple stats
- **Attendance reports** across the season
- **Multi-team dashboard** for coaches and parents with several teams

### Phase 3: Clubs & leagues

- Online registration forms, waivers and e-signatures, with discounts and payment plans
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

## 5. Architecture direction (leaves room for mobile)

Recommendation:

- **API-first backend** where every feature is an API the web app consumes, so the mobile app reuses it unchanged. TypeScript end to end (Node, with a typed API layer such as tRPC or REST + OpenAPI).
- **Web app** built with Next.js (React) as a responsive PWA.
- **Mobile later** with React Native / Expo, sharing types, validation, and API client packages in a monorepo (`apps/web`, `apps/mobile`, `packages/api`, `packages/shared`).
- **Postgres** as the main database. The data model is centered on households, teams, seasons, and events.
- **Real-time** over WebSockets for chat, RSVP, and schedule updates (P4).
- **Notification service** behind one interface that covers email, SMS, web push, and later APNs/FCM, with per-recipient delivery tracking (P2, P3).
- **Auth** with token-based sessions that work for both web and native clients.
- **Privacy** for youth data by design: COPPA awareness, minimal data on minors, and guardian-controlled visibility.

## 6. Open questions for Shawn

1. ~~**Target audience**~~ **Decided 2026-10-29: youth teams first.** Parents and guardians are the primary users, so guardian accounts, privacy for minors, and snack/carpool assignments get MVP polish first. Adult rec leagues follow.
2. **Monetization:** examples are a free core plus a paid "Pro" team tier, a payment processing margin, per-org pricing for clubs, or a mix.
3. **SMS:** SMS costs money per message. Should it be free in the MVP, capped, or a paid feature?
4. **Sports focus:** should the MVP be sport-agnostic, or tuned to one or two sports first?
5. **Geography:** US-only at launch, or international?
6. **Tech stack:** are you OK with the TypeScript / Next.js / Postgres direction, or do you prefer something else?

## Sources

- [TeamSnap pricing page](https://teamsnap.com/pricing)
- [TeamSnap app reviews (JustUseApp)](https://justuseapp.com/en/app/393048976/teamsnap/reviews)
- [TeamSnap reviews on G2](https://www.g2.com/products/TeamSnap/reviews)
- [TeamSnap on Capterra](https://www.capterra.com/p/123208/teamsnap/)
- [Best TeamSnap alternatives (BenchApp blog)](https://stg.benchapp.com/blog/best-teamsnap-alternatives)

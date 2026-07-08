# Google OAuth Setup for Punctual

Complete configuration for Google Calendar API access, so any user can connect
their Google account inside the app. Order matters — domain verification first,
because the Branding page rejects unverified authorized domains.

## Step 1 — Verify the domain in Search Console (~2 min)

1. Go to [search.google.com/search-console](https://search.google.com/search-console) —
   **use the same Google account** as for the Cloud Console project.
2. **Add property** → choose the **Domain** type (left box, not URL prefix) → enter `punctual-app.com`.
3. It gives you a TXT record and offers a **Cloudflare integration** — click it, authorize, done.
   - Manual alternative: copy the TXT value → Cloudflare dashboard → punctual-app.com → DNS →
     Add record → type `TXT`, name `@`, paste value → back in Search Console click **Verify**.

## Step 2 — Cloud project & Calendar API (~2 min)

1. [console.cloud.google.com](https://console.cloud.google.com) → create a project (name: `Punctual`).
2. **APIs & Services → Library** → search "Google Calendar API" → **Enable**.
   (Without this, the calendar scopes won't appear in Step 4.)

## Step 3 — Google Auth Platform → Branding

☰ Menu → **Google Auth Platform** (search in the top bar if hidden). If it says
"not configured yet," click **Get Started**: app name / support email / audience
**External** / contact email → agree → **Create**. Then open **Branding** and fill:

| Field | Value |
|---|---|
| App name | `Punctual` |
| User support email | your address (from the dropdown) |
| App logo | optional — fine to add, verification is needed anyway |
| Application home page | `https://punctual-app.com` |
| Application privacy policy link | `https://punctual-app.com/privacy/` |
| Authorized domains | `punctual-app.com` |

Save. The authorized domain is accepted because of Step 1.

## Step 4 — Data Access (scopes)

**Data Access** page → **Add or remove scopes** → filter for Calendar →
check **`.../auth/calendar.events.readonly`** ("View events on all your calendars") →
Update → Save.

- The scope is flagged *sensitive* — expected.
- Do **not** take the broad `.../auth/calendar` scope; it makes review harder.

## Step 5 — Create the OAuth client

**Clients** page → **Create client**:

- **Application type**:
  - `iOS` if the app uses the GoogleSignIn SDK → enter the **Bundle ID** from Xcode
    (e.g. `com.punctualapp.punctual`), leave **App Store ID** and **Team ID** blank
    (they're optional — no App Store publication needed).
  - `Desktop app` if the app opens the system browser with a loopback redirect —
    that type needs only a name.
- Create, and put the client ID into the app.

To test before verification: **Audience** page → add your own email under **Test users**.

## Step 6 — Publish + submit for verification

1. **Audience** page → Publishing status → **Publish app** (Testing → In production).
2. A **"Prepare for verification"** banner / **Verification Center** appears
   (triggered by the sensitive scope). Go through it:
   - Confirm links and domain (all green from Steps 1–3).
   - **Scope justification** — e.g.:
     > Punctual is a macOS menu bar app that shows the user's upcoming calendar
     > events, a countdown to the next meeting, and a full-screen reminder when a
     > meeting starts, with a one-click join link. Read-only access to calendar
     > events is the minimum scope required to display event titles, times, and
     > detect meeting URLs. The app runs entirely on-device; no data is
     > transmitted or stored elsewhere.
   - **Demo video** — screen-record the real flow: launch Punctual → click
     "Connect Google Calendar" → Google sign-in → **pause on the consent screen so
     the requested scope is clearly visible, UI in English** → finish → show the
     menu bar countdown/event list populated with the data. Upload **unlisted**
     to YouTube, paste the link.
3. Submit.

## What happens next

- The app keeps working during review; new users see an "unverified app" warning
  (max 100 grants) until approval.
- Google replies by email, typically within a few days to ~2 weeks, sometimes
  with fix requests. The common bounces are already handled:
  - privacy policy hosted on the same domain and linked from the homepage ✓
  - [Limited Use disclosure](https://developers.google.com/terms/api-services-user-data-policy)
    present on the privacy page ✓
  - homepage on an owned, verified domain (github.io would be rejected as
    third-party hosting — that's why punctual-app.com exists) ✓
- After approval: no warning, no user cap, refresh tokens stop expiring after 7 days.

## Site facts (for reference)

- Homepage: <https://punctual-app.com> (Cloudflare Pages, auto-deploys from `main`)
- Privacy policy: <https://punctual-app.com/privacy/>
- Domain registrar + DNS: Cloudflare

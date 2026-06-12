# Privacy Policy

**Last updated:** 2026-06-12
**Effective:** 2026-06-12
**Applies to:** BreezyDrive, an iOS application published by the BreezyDrive maintainers ("we", "us", "our").
**Public URL (production):** https://github.com/eddy2195/BreezyDriveDocs/blob/main/privacy-policy.md

We wrote this policy in plain language. If anything reads as ambiguous, the
plain-language interpretation governs — not the most expansive one a lawyer
could imagine.

---

## 1. Summary

BreezyDrive helps you pick the most comfortable time to drive between two
places. To do that the app needs to know where you are going, what the
weather will look like along the route, and where the sun will be. Almost
every byte of data the app produces stays on your device. We do not
operate a user-account backend, we do not have a "your data" dashboard,
and we cannot identify you.

The categories we do collect are listed in §3. The legal disclosures
required by the European Union's General Data Protection Regulation
(GDPR), and in particular **GDPR Article 13**, are in §10.

---

## 2. Data controller

For the purposes of GDPR Article 4(7), the data controller is the
publisher of BreezyDrive listed in the App Store metadata. Until a
separate registered legal entity is incorporated, you can reach the
controller at **breezydriveapp@gmail.com** (also linked from
Settings → About → "Send feedback") or at the email address printed on
the App Store listing ("Developer Support"). We will respond to any
GDPR rights request within 30 days, as required by Article 12(3).

---

## 3. What we collect

| Category | Stored where | Linked to your identity? | Used for tracking? | Purpose |
|---|---|---|---|---|
| Your location (one-shot, while in use, ~100 m accuracy goal) | RAM only; not written to disk | No | No | Suggest the origin field on the trip-input sheet. |
| Trip metadata (origin, destination, waypoints, scan window) | On-device SwiftData store | No | No | Render and refine your trip; let you re-open a saved trip. |
| Recent searches (last 20 destinations, with their resolved coordinates) | On-device SwiftData store | No | No | Speed up subsequent typing. |
| Saved trips (only when you save one) | On-device SwiftData store | No | No | One-tap re-plan. |
| Weather and route caches (forecast payloads keyed by coordinate, route polylines) | On-device SwiftData store, with expiry timestamps; stale rows are pruned automatically at launch | No | No | Avoid re-downloading the same forecast or re-computing the same route. |
| Plan-count timestamps (the time you submitted a plan — deliberately nothing else: no origin, no destination) | On-device SwiftData store | No | No | Count free-tier plans in the rolling week. |
| Aggregated, anonymous product-interaction events | Sent to TelemetryDeck while Settings → Privacy → "Share anonymous analytics" is on (it is on by default; turn it off any time) | No | No | Iterate on the score engine and paywall copy. |
| Purchase metadata (StoreKit transaction IDs, plan identifier) | Sent to Apple and RevenueCat | No | No | Enforce Pro entitlement; restore purchases. |
| Crash / hang / disk diagnostics (derived from Apple's MetricKit) | Coarse counts only (error domain/code, hang duration buckets, disk-write megabyte buckets) sent through the same TelemetryDeck stream, honoring the same analytics toggle. Stack traces and raw MetricKit payloads are never sent. | No | No | Find the bug that crashed your app. |

Everything above is the complete list. We do not collect: your name,
your email address, your phone number, your Apple ID, your contacts,
your photos, your microphone, your address book, your IDFA, your
browser history, your other apps, your device's hardware identifier
(only the salted hash described in §5 is derived from the vendor
identifier, on-device), or any social-network identity.

A note on IP addresses: we have no server, so *we* never see your IP
address. The third-party services in §5 are contacted directly from
your device, so — as with any internet request — they technically
receive your IP address as connection metadata. TelemetryDeck states
that it does not retain IP addresses; the others handle them under
their own policies linked in §5.

---

## 4. What we do not do

- **No cloud sync in v1.** Trip data, recent searches, saved trips,
  caches, and user preferences live in a SwiftData store inside the
  app's sandbox. They do not leave your device. There is no CloudKit
  or iCloud integration. Uninstalling the app deletes them permanently.
- **No advertising.** We do not run ads in the app. We do not sell ad
  inventory. We do not share your data with advertising networks.
- **No third-party tracking.** We do not request App Tracking
  Transparency permission because we do nothing that would require it
  (no IDFA, no fingerprinting, no cross-app or cross-site behaviour
  joining). The privacy manifest (`PrivacyInfo.xcprivacy`) shipped
  inside every build declares `NSPrivacyTracking = false` and an empty
  tracking-domain list — Apple's build tooling and App Review verify
  it against the app's actual behaviour.
- **No accounts.** There is no sign-up, no login, no password. There is
  also therefore no account-recovery flow, no email list, no
  marketing subscription.
- **No notifications in v1.** We do not request the notification
  permission and do not deliver push notifications. (This is on the
  v1.1 roadmap.)

---

## 5. Third parties

The third parties that receive data, what they receive, and where to
read their own policies:

| Party | What they receive | Why | Their privacy posture |
|---|---|---|---|
| **MET Norway** (the Norwegian Meteorological Institute, api.met.no) | The latitude/longitude points along your planned route, truncated to 4 decimal places. Requests also carry a User-Agent header identifying the app (name, version, and a developer contact address) as MET's terms of service require — this identifies *the app*, not *you*. No timestamps, no addresses, no identifiers. | To return the forecast we score. MET's API is queried directly from your device; responses are cached on-device per MET's HTTP expiry headers. | Weather data is provided by MET Norway / Yr under the [CC BY 4.0 / NLOD licence](https://api.met.no/doc/License); see also their [Terms of Service](https://api.met.no/doc/TermsOfService). Attribution is shown in Settings → About. |
| **Apple MapKit / CoreLocation geocoding** | Your origin and destination strings (and any waypoints) while autocompleting (`MKLocalSearchCompleter`); geocoding lookups for the chosen addresses (`CLGeocoder`); route requests for the chosen pair (`MKDirections`). | To geocode addresses and compute the driving route. | Governed by Apple's MapKit terms. Apple does not, per its own statements, link MapKit requests to your Apple ID for advertising. |
| **Apple App Store / StoreKit 2** | Purchase events for the Pro tier. | To process payments, manage your subscription, and restore purchases. | Governed by the Apple Media Services Terms and Conditions. |
| **RevenueCat** | An anonymous, per-install user identifier generated by the RevenueCat SDK (we set no custom user ID and no attributes); the App Store purchase metadata for entitlement evaluation. | To resolve whether you are entitled to the Pro tier. | See <https://www.revenuecat.com/privacy>. RevenueCat receives no PII from us. |
| **TelemetryDeck** (while the analytics toggle is on) | A fixed catalogue of aggregated, anonymous product-interaction events — app opened, onboarding completed, plan created, paywall shown, purchase started/completed/failed, restore initiated/completed, view toggles, and crash/hang/disk-write diagnostic counts. Payloads are coarse facts and bucketed integers only (e.g., route distance rounded to 50-mile buckets). No addresses; no routes; no coordinates; no names. The SDK's identifier is a salted hash computed on-device from the vendor identifier; the raw identifier never leaves your device. (This appears as "Device ID" on our App Store privacy label, marked not linked to you and not used for tracking.) | To inform iteration on the scoring engine and paywall. | See <https://telemetrydeck.com/privacy>. Hosted in the EU (Germany); IP addresses are not retained. |

Sun position is computed entirely on-device (a Swift port of the Meeus
astronomical algorithms); no astronomy service is contacted.

We do not work with any other third-party data processor. Adding any
new third-party SDK that touches user data would require revising this
policy, the privacy manifest, and our App Store privacy disclosures.

---

## 6. Permissions

| Permission | When requested | What we do with it | What happens if you deny |
|---|---|---|---|
| Location, when-in-use (`NSLocationWhenInUseUsageDescription`) | Onboarding step 3 of 4, after a rationale screen describing why. You can tap "Maybe later" and skip it. | One-shot fetch (no continuous tracking, ~100 m accuracy goal) to auto-fill the origin field on the trip-input sheet. The fetched coordinate stays in RAM and is not persisted. If you then plan a trip from that origin, the origin point — like any origin, typed or suggested — is sent to the weather and routing providers in §5 as part of planning. | The app stays fully usable; you type the origin manually. |

We do not currently request any other system permission.

---

## 7. Data retention

| Class of data | Retention |
|---|---|
| Location coordinate from the system | In RAM during the planning task only. Not persisted. |
| Trip metadata + recent searches + saved trips | Stored on-device until you uninstall the app. (Recent searches are additionally trimmed to the most recent 20.) |
| Weather and route caches | Stored on-device with expiry timestamps; expired rows are pruned automatically at app launch. Removed entirely on uninstall. |
| Plan-count timestamps | Stored on-device; only timestamps, no trip content. Removed on uninstall. |
| StoreKit transaction history | Held by Apple and RevenueCat for as long as their own terms require. We have no way to delete it. |
| Anonymous telemetry | Held by TelemetryDeck per their retention policy; we do not extract or back it up. |

There is no server-side database for us to "delete a user from". The
on-device store is yours: uninstalling the app deletes all of it.
Development builds additionally expose Settings → Debug → "Flush
caches" and "Reset all data"; a user-facing "Reset all data" item is
planned for v1.1.

---

## 8. Your rights and how to exercise them

Under GDPR (and equivalent regimes in the UK, California, and several
US states), you have a number of rights. Our short answer is: because
we do not hold identifiable personal data about you, most rights are
satisfied by uninstalling the app. The longer answer:

- **Right to access (GDPR Art. 15).** You can see every byte of data
  the app holds about you by inspecting the app's sandbox container on
  your device — there is no other repository.
- **Right to rectification (Art. 16).** Trip data is editable from the
  trip-input sheet.
- **Right to erasure (Art. 17).** Uninstall the app to delete the
  on-device store permanently. Cached forecasts and routes also expire
  and are pruned automatically. (A user-facing "Reset all data"
  setting lands with v1.1; development builds already include it.)
- **Right to restrict processing (Art. 18) and right to object
  (Art. 21).** Toggle Settings → Privacy → "Share anonymous
  analytics" off to stop the telemetry stream. The toggle is on by
  default and takes effect immediately; events are dropped on-device
  while it is off.
- **Right to data portability (Art. 20).** Because the data we hold is
  not "personal data" linkable to you, portability is a no-op.
- **Right not to be subject to automated decision-making (Art. 22).**
  The scoring engine is fully on-device and is not used for any
  decision that produces a legal or similarly significant effect on
  you.

To exercise any of these rights against us (e.g., to demand that we
stop processing telemetry events we already received from your
install), email breezydriveapp@gmail.com.

If you believe we are mishandling your data, you have the right to
lodge a complaint with the supervisory authority in your EU member
state.

---

## 9. Children

BreezyDrive is not directed at children under 13 (or under 16 in EU
jurisdictions that adopt the higher age threshold). We do not knowingly
collect personal data from children. If you believe a child has used
the app and we hold data about them, please contact us so we can
delete it — though, as noted, we are unable to identify the child
from any data we hold.

---

## 10. GDPR Article 13 disclosures (one-page version)

This section is the formal Article 13 notice required when collecting
personal data directly from a data subject. Even though most of what
we collect is non-personal, we surface every Art. 13(1) element here
to leave no ambiguity:

- **Identity of the controller and contact.** §2 above.
- **Contact details of the Data Protection Officer.** None designated;
  not required given the volume and category of data.
- **Purposes of processing and legal basis.** Purposes are listed in
  §3. Legal basis is Article 6(1)(b) (performance of the
  subscription contract, for purchase and entitlement data) and
  Article 6(1)(f) legitimate interest for the anonymous,
  aggregate telemetry stream (which you can switch off at any time —
  the Settings toggle is the Art. 21 objection mechanism) and for the
  one-shot location fetch that powers the core feature.
- **Recipients.** Listed in §5.
- **Transfers outside the EEA.** MET Norway processes weather requests
  in Norway (EEA). TelemetryDeck is hosted in Germany. Apple services
  may process data in the United States and other jurisdictions; Apple
  uses Standard Contractual Clauses. RevenueCat is US-based and
  operates under SCCs.
- **Retention.** §7.
- **Your rights.** §8.
- **Right to object at any time.** The Settings → Privacy toggle is
  the objection mechanism for telemetry; switching it off does not
  affect the lawfulness of prior processing.
- **Right to lodge a complaint.** Final paragraph of §8.
- **Whether providing the data is statutory or contractual.** None of
  it is statutory; all of it is voluntary in the sense that you can
  uninstall the app at any moment.
- **Automated decision-making.** None within Art. 22.

---

## 11. Changes to this policy

When we update the policy we will (a) bump the "Last updated" date at
the top, (b) commit the change visibly in this file's git history
(public), and (c) for material changes, surface an in-app notice
before the change takes effect.

---

## 12. Contact

For privacy questions or to exercise any of the rights above, email
breezydriveapp@gmail.com or the Developer Support address shown on our
App Store listing. We will respond within 30 days.

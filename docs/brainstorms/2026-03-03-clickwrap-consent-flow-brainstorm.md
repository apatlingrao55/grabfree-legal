---
title: Clickwrap Consent Flow for GrabFree App
type: feat
date: 2026-03-03
status: complete
related:
  - docs/brainstorms/2026-03-03-legal-docs-hardening-brainstorm.md
  - docs/plans/2026-03-03-feat-harden-legal-docs-statutory-carveouts-aup-plan.md
---

# Clickwrap Consent Flow for GrabFree App

## What We're Building

A legally defensible clickwrap consent mechanism for the GrabFree mobile app that:

1. **Captures affirmative consent** at registration via a required checkbox linking to ToS, Privacy Policy, and AUP
2. **Persists consent** to the database with timestamp and document version
3. **Handles re-consent** when legal documents are materially updated, via a blocking modal on app open
4. **Persists age confirmation** to the database (currently client-side only)

## Why This Approach

The current app has only passive browsewrap text ("By signing up, you agree to...") with no consent recorded in the database. Courts in NZ/AU increasingly reject browsewrap as insufficient for enforceable agreement. A clickwrap checkbox is the minimum legally defensible standard, and recording the consent version + timestamp creates an audit trail if consent is ever disputed.

**Chosen approach: Checkbox on registration + blocking modal for re-consent (Approach A)**

This was selected over:
- **Approach B (Separate consent screen)** — adds friction, overkill for a free app
- **Approach C (Progressive consent)** — unnecessarily complex, consent should be atomic

## Key Decisions

### 1. Consent UX: Required Checkbox on Registration Form
- Replace passive browsewrap text with a required checkbox
- Label: "I agree to the Terms of Use, Privacy Policy, and Acceptable Use Policy" with each as a tappable link
- Checkbox must be checked before the Register button is enabled
- Links open documents in an in-app browser or external browser

### 2. Re-consent: Blocking Modal on App Open
- When `user.legal_consent_version < LEGAL_VERSION`, show a full-screen blocking modal
- Modal displays: "We've updated our legal documents. Please review and accept to continue."
- Links to updated documents
- "I Agree" button to accept; no dismiss/skip option
- On accept: update `legal_consent_at` and `legal_consent_version` in DB
- User cannot use the app until they accept

### 3. Database Schema: Single Timestamp + Version
- Add three columns to the `users` table:
  - `legal_consent_at TIMESTAMPTZ` — when consent was last given
  - `legal_consent_version VARCHAR` — which version of docs they accepted (e.g., `'2026-03-03'`)
  - `age_confirmed_at TIMESTAMPTZ` — when age was confirmed (currently client-side only, not persisted)
- Simple, minimal — no separate consent_logs table needed for a free app

### 4. Age Gate: Persist to Database
- The existing client-side age checkbox ("I confirm that I am 16 years of age or older") should be persisted
- On registration: set `age_confirmed_at = NOW()` when the age checkbox is checked
- Age confirmation is one-time (not re-asked on legal doc updates)

### 5. Versioning: Simple Config String
- Single `LEGAL_VERSION` constant in app config (e.g., `'2026-03-03'`)
- Bumped manually when legal documents are materially updated
- Backend compares `user.legal_consent_version` against current `LEGAL_VERSION`
- No need for a versions table or per-document versioning — all docs update together

### 6. Target Markets
- NZ + AU only (no GDPR/EU requirements)
- Consent mechanism satisfies NZ Privacy Act 2020 and AU Privacy Act 1988 requirements

## Implementation Touchpoints

### Mobile App (`/Users/JFSA/Documents/git-projects/grabfree-app`)

| File | Change |
|------|--------|
| `mobile/components/AuthForm.tsx` | Replace browsewrap text with required checkbox; add age checkbox persistence; send consent data to API |
| `mobile/hooks/useAuth.tsx` | Update `register()` to pass consent fields; add re-consent check in auth flow |
| `mobile/services/api.ts` | Update `authApi.register()` to include `legal_consent_version`, `age_confirmed`; add `authApi.updateConsent()` |
| `mobile/app/(auth)/register.tsx` | No change (thin wrapper) |
| New: re-consent modal component | Blocking modal shown when `legal_consent_version` is stale |
| `src/routes/auth.js` | Accept + validate consent fields on `POST /api/auth/register`; add `POST /api/auth/consent` endpoint |
| `src/models/User.js` | Update `User.create()` to insert consent columns; add `User.updateConsent()` |
| `scripts/init-db.sql` | Add `legal_consent_at`, `legal_consent_version`, `age_confirmed_at` columns |
| New: DB migration script | ALTER TABLE for existing users |
| Config | Add `LEGAL_VERSION = '2026-03-03'` constant |

### Registration Flow (Updated)

```
1. User fills in email, username, password
2. User checks age confirmation checkbox ✓
3. User checks legal consent checkbox ✓ (links to ToS, PP, AUP)
4. User taps Register
5. Frontend sends: { email, password, username, legal_consent_version, age_confirmed: true }
6. Backend validates all fields including consent
7. Backend inserts user with legal_consent_at=NOW(), legal_consent_version='2026-03-03', age_confirmed_at=NOW()
8. OTP verification flow continues as normal
```

### Re-consent Flow

```
1. User opens app → JWT decoded → user record fetched
2. Backend (or frontend) checks: user.legal_consent_version < LEGAL_VERSION?
3. If stale → frontend shows blocking modal
4. User reviews links, taps "I Agree"
5. Frontend calls POST /api/auth/consent { legal_consent_version: '2026-03-03' }
6. Backend updates user.legal_consent_at=NOW(), user.legal_consent_version='2026-03-03'
7. Modal dismissed, app usable
```

## Open Questions

_None — all questions resolved during brainstorming._

## Resolved Questions

| Question | Decision | Rationale |
|----------|----------|-----------|
| Where to place consent UX? | On registration form (checkbox) | Minimal friction, legally sufficient |
| How to handle re-consent? | Blocking modal on app open | Cannot be dismissed, ensures fresh consent |
| DB design? | Single timestamp + version on users table | Simple, sufficient for free app |
| Persist age gate? | Yes, add `age_confirmed_at` column | Currently client-side only, no audit trail |
| Versioning strategy? | Simple string in app config | No need for per-document versions |
| GDPR needed? | No | Not releasing to EU |

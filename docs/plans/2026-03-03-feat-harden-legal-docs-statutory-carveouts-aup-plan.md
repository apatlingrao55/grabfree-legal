---
title: "feat: Harden legal documents with statutory carve-outs, takedown processes, and AUP"
type: feat
status: completed
date: 2026-03-03
---

# Harden Legal Documents with Statutory Carve-outs, Takedown Processes, and AUP

## Overview

Surgical hardening of GrabFree's legal document stack to close liability gaps identified by indemnity/liability research. This covers rewriting weak warranty/liability clauses, adding formal content takedown processes, strengthening dispute resolution, creating a standalone Acceptable Use Policy, and fixing branding inconsistencies.

**Target markets:** NZ + AU only.
**Risk posture:** Maximum protection — explicit CGA (NZ), ACL (AU) carve-outs, plus catch-all for other mandatory law.
**Source brainstorm:** `docs/brainstorms/2026-03-03-legal-docs-hardening-brainstorm.md`
**Source research:** `~/Downloads/app-developer-indemnity-research.md`

## Problem Statement / Motivation

The current legal documents have several clauses that NZ/AU courts are likely to strike down:

1. **ToS S.13.1 attempts to exclude liability for personal injury/death** — void in NZ, AU, and UK. Risks entire limitation clause being invalidated.
2. **No statutory carve-outs** — disclaimers use blanket "as is" language without acknowledging CGA (NZ) or ACL (AU) mandatory guarantees. Courts can strike the whole clause.
3. **No content takedown process** — app has UGC (listings, photos, messages, board posts) but no formal IP/copyright or HDCA takedown mechanism. Loses safe harbour protection.
4. **Weak dispute resolution** — "good-faith negotiation then courts" with no timeframe, mediation, or AU consumer law acknowledgement.
5. **No monitoring obligation disclaimer** — community moderation features could imply a duty to monitor UGC.
6. **Branding inconsistencies** — index.html and SUPPORT.md still reference "Giveaway App" after rebrand.

## Proposed Solution

**Approach: Surgical clause replacement** in existing documents + one new document (AUP). No structural reorganization. Changes follow existing formatting conventions (decimal section numbering, bold+caps for disclaimers, markdown tables for data, relative links between docs).

---

## Technical Approach

### Section Numbering Strategy

New sections will be inserted into the ToS, which shifts subsequent section numbers. The plan uses a **renumber-on-insert** approach:

**Current ToS structure (24 sections):**
S.1-11 (unchanged) → S.12 Disclaimers (rewrite) → S.13 Limitation (rewrite) → S.14 Indemnification (minor edit) → S.15 Assumption of Risk (unchanged) → S.16 Third-Party Services (unchanged) → S.17 Modifications (unchanged) → **NEW S.18 Content Takedown** → **NEW S.19 No Monitoring Obligation** → S.20→S.18 Geographic Scope → S.21→S.19 Governing Law → S.22→S.20 Dispute Resolution (rewrite) → S.23→S.21 Severability → S.24→S.22 Entire Agreement (update to include AUP) → S.25→S.23 No Waiver → **NEW S.26→S.24 Survival** → S.27→S.25 Contact

Wait — inserting sections in the middle will break the numbering flow. Simpler approach: **append new sections at the end** to minimize renumbering disruption:

**Revised strategy — append new sections:**
- S.1-16: Unchanged (except S.5.2, S.12, S.13, S.14 content edits)
- S.17: Modifications (unchanged)
- S.18: Geographic Scope (was S.18, renumbered from S.20)

Actually, the cleanest approach: **keep existing section numbers, insert new sections after S.16 (Third-Party Services), renumber S.17+ accordingly**:

**Final ToS section plan (27 sections):**

| # | Section | Status |
|---|---------|--------|
| 1 | Acceptance of Terms | Unchanged |
| 2 | Description of Service | Unchanged |
| 3 | Eligibility | Unchanged |
| 4 | Account Registration | Unchanged |
| 5 | User Conduct | **Edit** — S.5.2 becomes stub referencing AUP |
| 6 | Item Listings | Unchanged |
| 7 | Messaging | Unchanged |
| 8 | Community Features | Unchanged |
| 9 | Push Notifications | Unchanged |
| 10 | Account Suspension and Termination | Unchanged |
| 11 | Intellectual Property | **Edit** — add user content warranty to S.11.2 |
| 12 | DISCLAIMERS AND NO WARRANTIES | **Rewrite** — add CGA/ACL resupply carve-out |
| 13 | LIMITATION OF LIABILITY | **Rewrite** — mandatory carve-outs, fix death/injury |
| 14 | Indemnification | **Edit** — add warranty breach to indemnifiable events |
| 15 | Assumption of Risk | Unchanged |
| 16 | Third-Party Services | Unchanged |
| 17 | Content Takedown and IP Claims | **NEW** |
| 18 | Harmful Digital Communications | **NEW** |
| 19 | No Monitoring Obligation | **NEW** |
| 20 | Modifications to Terms | Was S.17 |
| 21 | Geographic Scope | Was S.18 |
| 22 | Governing Law | Was S.19 |
| 23 | Dispute Resolution | Was S.20, **rewrite** |
| 24 | Severability | Was S.21 |
| 25 | Entire Agreement | Was S.22, **edit** to include AUP |
| 26 | No Waiver | Was S.23 |
| 27 | Survival | **NEW** |
| 28 | Contact Us | Was S.24 |

### Files Changed

| File | Action | Scope |
|------|--------|-------|
| `TERMS_OF_USE.md` | Edit | 10 sections modified/added |
| `EULA.md` | Edit | 4 sections modified |
| `PRIVACY_POLICY.md` | Edit | 3 sections modified/added |
| `AUP.md` | **Create** | New standalone document |
| `SUPPORT.md` | Edit | Branding fix, date update, section refs, AUP link |
| `index.html` | Edit | Branding fix, AUP card, footer date |

---

## Implementation Phases

### Phase 1: ToS — Rewrite Liability/Warranty Sections (Highest Risk)

These are the clauses most likely to be struck down by courts. Fix first.

#### 1.1 Rewrite S.12 Disclaimers — Add CGA/ACL Resupply Carve-out

**File:** `TERMS_OF_USE.md` S.12

**Add to end of S.12.1 (after existing warranty exclusion list):**

```
TO THE EXTENT THAT APPLICABLE LAW (INCLUDING THE CONSUMER GUARANTEES ACT 1993
(NZ) OR AUSTRALIAN CONSUMER LAW) IMPLIES GUARANTEES OR CONDITIONS THAT CANNOT
BE EXCLUDED, GRABFREE'S LIABILITY FOR BREACH OF THOSE GUARANTEES IS LIMITED,
TO THE FULLEST EXTENT PERMITTED BY LAW, TO RESUPPLYING THE SERVICES OR PAYING
THE COST OF HAVING THE SERVICES RESUPPLIED.
```

**Rationale:** CGA s.43(2) and ACL equivalent allow limiting the remedy to resupply for services. This language is specifically designed to use this escape valve. For a free app, "resupplying the services" is a meaningful but minimal remedy.

- [x] Add resupply carve-out after existing warranty disclaimer list in S.12.1
- [x] Add standalone paragraph after S.12.7: "NOTHING IN THESE TERMS LIMITS OR EXCLUDES ANY GUARANTEE, RIGHT, OR REMEDY WHICH CANNOT BE LIMITED OR EXCLUDED UNDER THE CONSUMER GUARANTEES ACT 1993, THE FAIR TRADING ACT 1986, THE AUSTRALIAN CONSUMER LAW, OR OTHER APPLICABLE MANDATORY LAW."

#### 1.2 Rewrite S.13 Limitation of Liability — Fix Death/Injury, Add Mandatory Carve-outs

**File:** `TERMS_OF_USE.md` S.13

**Critical fix — remove "PERSONAL INJURY OR PROPERTY DAMAGE" from S.13.1 exclusion list.** This is void in NZ/AU/UK and risks the entire clause.

**Add new S.13.6 Mandatory Exceptions after S.13.5:**

```
### 13.6 Mandatory Exceptions

THE LIMITATIONS OF LIABILITY IN THIS SECTION DO NOT APPLY TO LIABILITY FOR:

(a) DEATH OR PERSONAL INJURY CAUSED BY GRABFREE'S NEGLIGENCE;
(b) FRAUD OR FRAUDULENT MISREPRESENTATION BY GRABFREE;
(c) ANY LIABILITY THAT CANNOT BE EXCLUDED OR LIMITED UNDER APPLICABLE LAW,
    INCLUDING THE CONSUMER GUARANTEES ACT 1993 (NEW ZEALAND), THE FAIR
    TRADING ACT 1986 (NEW ZEALAND), OR THE AUSTRALIAN CONSUMER LAW.

FOR USERS IN AUSTRALIA: NOTHING IN THESE TERMS LIMITS ANY RIGHT YOU MAY
HAVE UNDER THE AUSTRALIAN CONSUMER LAW (SCHEDULE 2 OF THE COMPETITION AND
CONSUMER ACT 2010).
```

- [x] Remove "PERSONAL INJURY OR PROPERTY DAMAGE" from S.13.1 bullet list
- [x] Add S.13.6 Mandatory Exceptions with death/injury, fraud, and statutory carve-outs
- [x] Add explicit AU Consumer Law acknowledgement in S.13.6

#### 1.3 Edit S.14 Indemnification — Add Warranty Breach

**File:** `TERMS_OF_USE.md` S.14

**Add to indemnification list:**
```
- Your breach of any representation or warranty in these Terms (including the
  content ownership warranty in Section 11.2)
```

- [x] Add warranty breach to S.14 indemnification event list

#### 1.4 Edit S.11.2 User Content — Add Ownership Warranty

**File:** `TERMS_OF_USE.md` S.11.2

**Add after the licence grant paragraph:**
```
You represent and warrant that: (a) you own or have the necessary rights,
licences, and permissions to the content you post; (b) your content does not
infringe any third-party intellectual property, privacy, or other rights; and
(c) your content complies with these Terms and our Acceptable Use Policy.
```

- [x] Add content ownership warranty to S.11.2

### Phase 2: ToS — New Sections (Takedown, HDCA, Monitoring, Survival)

#### 2.1 New S.17: Content Takedown and IP Claims

**File:** `TERMS_OF_USE.md`

**SpecFlow finding:** IP/copyright takedown and HDCA complaints are legally distinct regimes with different response timelines (5 business days vs 48 hours). Implement as separate sections.

This section covers IP/copyright claims only.

**Structure:**

```markdown
## 17. Content Takedown and Intellectual Property Claims

### 17.1 Reporting IP Infringement

If you believe content on the App infringes your intellectual property rights,
you may submit a takedown notice to: **takedown@[domain]**

Your notice must include:

(a) A description of the intellectual property right you claim is infringed;
(b) A description of the content you claim is infringing, with sufficient
    detail to locate it (listing title, username, URL, or screenshot);
(c) Your contact information (name, email address, postal address);
(d) A statement that you have a good-faith belief that the use of the
    content is not authorised by the IP owner, its agent, or the law;
(e) A statement, under penalty of perjury, that the information in your
    notice is accurate and that you are the IP owner or authorised to act
    on their behalf.

### 17.2 Response Timeline

We will acknowledge receipt of a complete takedown notice within **5 business
days**. Incomplete notices will be returned with a request for the missing
information. The 5-day timeline begins when a complete notice is received.

If we determine the notice is valid, the content will be removed or disabled
and the content owner will be notified via email with: the nature of the claim
(without disclosing the claimant's identity beyond what is required), and
instructions for submitting a counter-notice.

### 17.3 Counter-Notice

If your content has been removed and you believe it was removed in error, you
may submit a counter-notice to **takedown@[domain]** within **14 business
days** of receiving the removal notification.

Your counter-notice must include:

(a) Identification of the content that was removed;
(b) A statement under penalty of perjury that you have a good-faith belief
    the content was removed in error;
(c) Your name, email address, and consent to the jurisdiction of the courts
    of New Zealand;
(d) Your contact information.

We will review the counter-notice within **5 business days**. If upheld,
the content will be restored and the original claimant notified.

### 17.4 Repeat Infringer Policy

Users who are the subject of **three or more upheld takedown notices**
(notices that were not successfully counter-noticed) will have their account
permanently terminated. A successful counter-notice does not count as a strike.

Upon termination under this policy:
- All of the user's active listings will be removed
- Users who have claimed items from those listings will be notified
- The terminated user's data will be handled per our Privacy Policy

### 17.5 Bad-Faith Notices

Submitting a knowingly false or misleading takedown notice may result in
liability to you under applicable law, including the NZ Fair Trading Act 1986
and the tort of injurious falsehood. We reserve the right to disregard
notices we reasonably believe are submitted in bad faith.
```

- [x] Create S.17 Content Takedown and IP Claims
- [x] Include: notice requirements, 5-business-day timeline, counter-notice (14 days), repeat infringer (3 upheld strikes), bad-faith notice warning
- [x] Use dedicated takedown email address

#### 2.2 New S.18: Harmful Digital Communications

**File:** `TERMS_OF_USE.md`

**SpecFlow finding:** The NZ HDCA provides safe harbour for "online content hosts" (s.23-24) only if the host responds within 48 hours. This is a separate regime from IP takedown.

```markdown
## 18. Harmful Digital Communications

### 18.1 Complaints Process

If you believe content on the App constitutes a harmful digital communication
under the Harmful Digital Communications Act 2015 (NZ), including
harassment, intimate images shared without consent, or content that causes
serious emotional distress, you may submit a complaint to:
**takedown@[domain]**

### 18.2 Response Timeline

We will acknowledge and assess complaints about harmful digital
communications within **48 hours** of receipt. Where we determine content
is likely to cause serious harm, we will remove or disable access to the
content promptly.

### 18.3 Relationship to Other Processes

This process is separate from the IP takedown process in Section 17.
Complaints involving both harmful communications and IP infringement will
be assessed under whichever process provides the faster response.
```

- [x] Create S.18 Harmful Digital Communications with 48-hour response timeline
- [x] Separate from IP takedown to satisfy HDCA safe harbour requirements

#### 2.3 New S.19: No Monitoring Obligation

**File:** `TERMS_OF_USE.md`

```markdown
## 19. No Monitoring Obligation

GrabFree has no obligation to monitor, review, or pre-screen user content,
including item listings, photos, messages, community board posts, or ratings.
The provision of community moderation features (content flagging, community
voting, and administrative review as described in Section 8) does not create
any duty on GrabFree to actively monitor content or guarantee the
effectiveness of moderation.
```

- [x] Create S.19 No Monitoring Obligation
- [x] Explicitly reference community moderation features to distinguish from platform monitoring

#### 2.4 New S.27: Survival Clause

**File:** `TERMS_OF_USE.md` (final section before Contact)

```markdown
## 27. Survival

The following sections survive termination of these Terms for any reason:

- Section 11: Intellectual Property
- Section 12: Disclaimers and No Warranties
- Section 13: Limitation of Liability
- Section 14: Indemnification
- Section 15: Assumption of Risk
- Section 17: Content Takedown and IP Claims
- Section 22: Governing Law
- Section 23: Dispute Resolution
- Section 24: Severability
```

- [x] Create standalone S.27 Survival clause with explicit section list

#### 2.5 Edit S.5.2 — Reference AUP

**File:** `TERMS_OF_USE.md` S.5.2

Replace the existing S.5.2 prohibited conduct bullet list with a stub:

```markdown
### 5.2 Prohibited Conduct

Your use of the App is subject to our [Acceptable Use Policy](AUP.md), which
defines prohibited content, prohibited conduct, and enforcement actions. By
using the App, you agree to comply with the Acceptable Use Policy. Violation
of the Acceptable Use Policy constitutes a violation of these Terms.
```

- [x] Replace S.5.2 bullet list with AUP reference stub
- [x] Keep S.5.1 Acceptable Use as-is

### Phase 3: ToS — Rewrite Dispute Resolution

#### 3.1 Rewrite S.23 (was S.20): Dispute Resolution

**File:** `TERMS_OF_USE.md`

**SpecFlow findings incorporated:**
- AU users need explicit right to access state tribunals for ACL claims
- Mediation costs should be addressed
- 12-month limitation starts from date user became aware of the issue
- Internal appeals path for moderation actions (separate from formal disputes)

```markdown
## 23. Dispute Resolution

### 23.1 Internal Appeals for Moderation Actions

If your account is suspended, banned, or your content is removed for
reasons other than an IP takedown (Section 17), you may request an internal
review by emailing us within 14 days of the action. We will review your
appeal and respond within 10 business days.

### 23.2 Informal Resolution

Before initiating any formal proceedings, you agree to contact us at
**dev@aiconsult.co.nz** and allow **30 days** for a good-faith attempt to
resolve the dispute informally. Both parties must engage in good faith,
which means: responding to communications within 5 business days, providing
relevant information when reasonably requested, and considering reasonable
settlement proposals.

### 23.3 Mediation

If the dispute is not resolved informally within 30 days, either party may
refer the dispute to mediation administered by the New Zealand Dispute
Resolution Centre (NZDRC) or another mutually agreed mediator, before
proceeding to litigation. Each party shall bear their own costs of
mediation unless the mediator directs otherwise.

### 23.4 Courts

If mediation does not resolve the dispute, either party may commence
proceedings in the courts of New Zealand.

### 23.5 Time Limitation

Any claim arising from or related to these Terms or your use of the App
must be commenced within **12 months** from the date you became aware (or
ought reasonably to have become aware) of the event giving rise to the
claim. This limitation does not apply where a longer limitation period is
required by applicable mandatory law.

### 23.6 For Users in Australia

Nothing in this Section 23 limits or restricts your right to:

(a) lodge a complaint with the Australian Competition and Consumer
    Commission (ACCC) or a state/territory consumer protection agency;
(b) bring proceedings before your state or territory civil and
    administrative tribunal (including VCAT, NCAT, or QCAT) for claims
    under the Australian Consumer Law; or
(c) exercise any other statutory right that cannot be excluded or limited.

The mediation requirement in Section 23.3 does not apply to claims brought
under the Australian Consumer Law before an Australian tribunal.
```

- [x] Rewrite dispute resolution with 30-day informal + mediation + courts structure
- [x] Add internal appeals path for moderation (14-day window, 10-day response)
- [x] Define "good faith" with specific behavioural requirements
- [x] Address mediation costs (each party bears own)
- [x] Set 12-month limitation from "date of awareness" (NZ Limitation Act s.11 alignment)
- [x] Add AU carve-out preserving access to state tribunals and ACCC
- [x] Exempt AU ACL claims from mandatory NZ mediation

### Phase 4: EULA — Mirror Changes

**File:** `EULA.md`

#### 4.1 EULA S.8 Disclaimers — Add CGA/ACL Resupply Carve-out

Mirror the resupply carve-out language from ToS S.12.1 and add the statutory catch-all paragraph.

- [x] Add resupply carve-out to EULA S.8.1
- [x] Add statutory catch-all paragraph after S.8.3

#### 4.2 EULA S.9 Limitation of Liability — Add Mandatory Carve-outs

Mirror ToS S.13 changes: remove any personal injury/death exclusion, add S.9.2 Mandatory Exceptions with death/injury, fraud, and CGA/ACL carve-outs. Add AU Consumer Law acknowledgement.

- [x] Add mandatory exceptions subsection to EULA S.9
- [x] Add AU Consumer Law acknowledgement

#### 4.3 EULA S.13.2 Dispute Resolution — Mirror ToS

Replace with same structure as ToS S.23: 30-day informal + mediation + courts + 12-month limitation + AU carve-out. Reference ToS for full details rather than duplicating:

```markdown
### 13.2 Dispute Resolution

Disputes arising from this Agreement are subject to the dispute resolution
process set out in Section 23 of our [Terms of Use](TERMS_OF_USE.md),
which is incorporated by reference.
```

- [x] Replace EULA dispute resolution with reference to ToS S.23

#### 4.4 EULA S.7.3 Survival — Synchronize

Update to include Governing Law and Dispute Resolution:

```
Sections 3 (Intellectual Property), 8 (Disclaimers), 9 (Limitation of
Liability), 10 (Indemnification), 12 (Export Compliance), and 13
(Governing Law and Dispute Resolution) shall survive termination.
```

- [x] Update EULA S.7.3 survival list to include all surviving sections
- [x] Add reference to AUP: "Your obligations under the Acceptable Use Policy survive to the extent they relate to content posted during the term."

#### 4.5 EULA — Add AUP Reference

Add to EULA S.2 (License Restrictions) a reference to the AUP:

```
Your use of the App is also subject to our [Acceptable Use Policy](AUP.md).
```

- [x] Add AUP reference to EULA S.2

### Phase 5: Privacy Policy — AU Acknowledgement and SDK Disclosure

**File:** `PRIVACY_POLICY.md`

#### 5.1 Strengthen S.5.1 — SDK Data Disclosure

Expand the third-party services table to clarify what data each SDK collects independently (not just what GrabFree shares with them):

```markdown
| Service | Provider | Data Shared by Us | Data Collected by SDK | Purpose |
|---------|----------|-------------------|----------------------|---------|
| Email delivery | Resend | Email address, OTP codes | Email metadata (delivery status, bounces) | Sending verification emails |
| Image storage | Cloudflare R2 | Item photographs | Request metadata (IP address, access logs) | Storing and serving item images |
| Push notifications | Expo | Push token, notification content | Device type, OS version, delivery status | Delivering push notifications |
```

Add note:
```
Under the NZ Privacy Act 2020 and the Australian Privacy Act 1988 (where
applicable), we are responsible for personal information collected by
third-party SDKs and services integrated into the App, even where that
collection is initiated by the third-party service. Our use of these
services is governed by data processing agreements where available.
```

- [x] Expand S.5.1 table with "Data Collected by SDK" column
- [x] Add responsibility statement for SDK data collection

#### 5.2 New Section: Australian Users

Add after S.16 (Governing Law), before S.17 (Contact):

```markdown
## 17. For Users in Australia

If you are located in Australia, the Australian Privacy Principles (APPs)
under the Privacy Act 1988 (Cth) may apply to our collection and handling
of your personal information. Nothing in this Privacy Policy is intended to
limit or exclude any right you have under the APPs.

You may also lodge a privacy complaint with the Office of the Australian
Information Commissioner (OAIC) at oaic.gov.au.
```

**Note:** The AU Privacy Act small business exemption (s.6C, turnover under $3M AUD) may apply to GrabFree. This language uses "may apply" deliberately to acknowledge the obligation without creating unnecessary commitments. A lawyer review can firm this up.

- [x] Add "For Users in Australia" section with APP acknowledgement
- [x] Use "may apply" language pending small business exemption assessment

#### 5.3 Edit S.16 Governing Law — Add AU Acknowledgement

Add:
```
Nothing in this Privacy Policy limits rights under the Australian Privacy
Act 1988 (Cth) where applicable.
```

- [x] Add AU Privacy Act acknowledgement to governing law section

#### 5.4 New: Takedown Notice Data Disclosure

Add to S.1 (Information We Collect) or S.5 (Data Sharing):

```markdown
### Content Takedown Notices

If you submit or are the subject of a content takedown notice (see Terms
of Use, Section 17), we collect and retain:

- Claimant contact information and takedown notice content
- Records of notices, counter-notices, and outcomes
- Repeat infringer tracking data

This data is retained for the duration of any related dispute and for a
minimum of 24 months after resolution. If a counter-notice is submitted,
limited information about the claim (nature of the claim, not claimant
identity) is shared with the content owner to enable them to respond.
Claimant identity is not disclosed to the accused user unless required
by law or with the claimant's consent.
```

- [x] Add takedown notice data disclosure to Privacy Policy

### Phase 6: Create AUP.md

**File:** `AUP.md` (new)

**Structure following repo conventions:** H1 title, bold app name + date header, opt-in statement, numbered H2 sections.

**Sections:**

1. **Introduction** — What this policy covers, relationship to ToS
2. **Prohibited Content** — Expanded from ToS S.5.2:
   - Illegal, hazardous, or recalled items (cite NZ FTA product safety standards and AU ACL Schedule 2)
   - Items with legal transfer restrictions (firearms per NZ Arms Act 1983, prescription medications, regulated plant material, alcohol, CITES-regulated products)
   - Counterfeit or trademark-infringing goods
   - Intellectual property-infringing content (copyrighted images, text)
   - Defamatory content
   - Harassment, threats, hate speech
   - Non-consensual intimate images (criminal offense under HDCA s.22A)
   - Doxxing (posting others' personal information without consent)
   - Child sexual abuse material (CSAM)
   - Fraud, scam, or deceptive listings
   - Spam, advertisements, promotional content
   - Malware, malicious content
3. **Prohibited Conduct** — Expanded from ToS S.5.2:
   - Selling items or requesting payment
   - Creating fake or misleading listings
   - Multiple accounts to circumvent bans
   - Accessing other users' accounts
   - Reverse-engineering or interfering with infrastructure
   - Circumventing rate limits or security measures
   - Impersonating other users, GrabFree staff, or administrators
   - Automated access (bots, scrapers)
   - Any unlawful purpose
   - Content must comply with laws of both NZ and AU (higher standard applies)
4. **Enforcement** — Graduated response:
   - Content removal (at sole discretion, no liability)
   - Warning
   - Temporary suspension
   - Permanent ban
   - Relationship to IP takedown strikes (separate tracking; an AUP violation for IP-infringing content may also trigger a takedown strike under ToS S.17)
5. **Reporting** — How to report violations, available report categories, what happens after a report
6. **Right to Remove Content** — We may remove content at sole discretion with no liability
7. **User Responsibility** — Users are solely responsible for content they post
8. **Modifications** — We may update this AUP; continued use = acceptance
9. **Contact** — dev@aiconsult.co.nz

- [x] Create AUP.md following repo formatting conventions
- [x] Include specific NZ/AU statutory references for prohibited items
- [x] Address NZ-legal-but-AU-illegal content (higher standard applies)
- [x] Define enforcement escalation path
- [x] Clarify relationship between AUP enforcement and IP takedown strikes
- [x] Include GrabFree staff impersonation in prohibited conduct

### Phase 7: Branding and Cross-Reference Fixes

#### 7.1 Fix index.html

**File:** `index.html`

- [x] Update `<title>` from "Giveaway App" to "GrabFree"
- [x] Update `<h1>` from "Giveaway App" to "GrabFree"
- [x] Update card descriptions that say "Giveaway App" to "GrabFree"
- [x] Add AUP card to the cards grid:
  ```html
  <a class="card" href="AUP.md">
    <h2>Acceptable Use Policy</h2>
    <p>Prohibited content, prohibited conduct, and enforcement actions.</p>
  </a>
  ```
- [x] Update footer date to "March 2026" (or current date when published)

#### 7.2 Fix SUPPORT.md

**File:** `SUPPORT.md`

- [x] Replace all "Giveaway" / "Giveaway App" references with "GrabFree"
- [x] Update "Last Updated" date to match other documents
- [x] Update any section number references to ToS (sections have been renumbered)
- [x] Add AUP to "Related Documents" section (S.6)
- [x] Add FAQ entry for content takedown process

#### 7.3 Update ToS S.25 Entire Agreement

**File:** `TERMS_OF_USE.md` S.25 (was S.22)

Update to include AUP:
```
These Terms, together with the Privacy Policy and Acceptable Use Policy,
constitute the entire agreement between you and GrabFree regarding your
use of the App.
```

- [x] Add AUP reference to Entire Agreement clause

#### 7.4 Update EULA S.15 Entire Agreement

**File:** `EULA.md` S.15

Update to include AUP:
```
This Agreement, together with our Terms of Use, Privacy Policy, and
Acceptable Use Policy, constitutes the entire agreement...
```

- [x] Add AUP reference to EULA Entire Agreement clause

### Phase 8: Final Cross-Document Consistency Check

- [x] Verify all ToS section numbers are correct after renumbering
- [x] Verify EULA references to ToS section numbers are updated
- [x] Verify SUPPORT.md section references match final ToS numbering
- [x] Verify all inter-document links work (`[Name](FILE.md)` format)
- [x] Verify consistent use of "GrabFree" (not "Grabfree" or "Giveaway")
- [x] Verify contact email is consistent (dev@aiconsult.co.nz for legal, support-approjects@pm.me for support)
- [x] Verify all defined terms are capitalized consistently (App, Terms, User, Privacy Policy, Acceptable Use Policy)
- [x] Verify closing opt-in statements are updated in all documents
- [x] Update "Last Updated" date across all documents

---

## Acceptance Criteria

### Functional Requirements

- [x] ToS S.12 disclaimers include CGA/ACL resupply carve-out
- [x] ToS S.13 does NOT exclude death/personal injury liability
- [x] ToS S.13 includes mandatory exceptions for death/injury from negligence, fraud, and statutory rights
- [x] ToS S.13 includes explicit AU Consumer Law acknowledgement
- [x] ToS has content takedown process (S.17) with: notice requirements, 5-day timeline, counter-notice (14 days), repeat infringer (3 upheld strikes), bad-faith warning
- [x] ToS has HDCA complaints process (S.18) with 48-hour response timeline
- [x] ToS has no monitoring obligation clause (S.19)
- [x] ToS has standalone survival clause (S.27)
- [x] ToS S.11.2 includes user content ownership warranty
- [x] ToS S.14 indemnification includes warranty breach
- [x] ToS S.23 dispute resolution: 30-day informal + mediation + courts + 12-month limitation + AU tribunal carve-out
- [x] EULA mirrors all ToS warranty/liability/dispute changes
- [x] Privacy Policy includes AU Privacy Act acknowledgement
- [x] Privacy Policy SDK table includes "Data Collected by SDK" column
- [x] Privacy Policy includes takedown notice data disclosure
- [x] AUP.md exists with prohibited content, prohibited conduct, enforcement, reporting
- [x] AUP includes specific NZ/AU statutory references for restricted items
- [x] index.html says "GrabFree" (not "Giveaway App")
- [x] index.html has AUP card
- [x] SUPPORT.md says "GrabFree" throughout
- [x] All cross-document links work
- [x] All section number references are correct

### Quality Gates

- [x] No blanket liability exclusions that would be struck down by NZ/AU courts
- [x] All mandatory law carve-outs use "to the fullest/maximum extent permitted by applicable law" umbrella
- [x] All sections follow existing formatting conventions (bold+caps for disclaimers, decimal numbering, markdown tables)
- [x] Consistent defined terms across all documents

---

## Dependencies & Risks

**Dependencies:**
- Dedicated takedown email address needs to be set up before publishing (e.g., takedown@aiconsult.co.nz or legal@aiconsult.co.nz)
- If the app doesn't currently have a re-consent mechanism, the AUP will only bind new users via ToS incorporation and existing users via the "modifications" clause

**Risks:**
- **AU mediation enforceability** — The AU tribunal carve-out mitigates this, but a lawyer review is recommended
- **AU small business exemption** — "May apply" language is used as a hedge; a lawyer can firm this up
- **CGA applicability to free apps** — NZ courts haven't definitively ruled on whether free apps are "in trade" under CGA. The CGA carve-outs are included as belt-and-suspenders
- **FTA Part 2A unfair contract terms** — The $50 NZD cap and 12-month limitation could theoretically be challenged as unfair terms under FTA Part 2A (effective March 2023). The proportionality argument (free service, reasonable cap) is strong but untested for apps

**Out of scope:**
- GDPR compliance (not releasing to EU)
- US DMCA agent registration (not releasing to US)
- UK Online Safety Act compliance (not releasing to UK)
- Professional legal review (strongly recommended as a follow-up)
- In-app consent flow changes (technical implementation, not legal docs)

---

## References & Research

### Internal References

- Brainstorm: `docs/brainstorms/2026-03-03-legal-docs-hardening-brainstorm.md`
- Source research: `~/Downloads/app-developer-indemnity-research.md`
- Current ToS: `TERMS_OF_USE.md` (383 lines, 24 sections)
- Current EULA: `EULA.md` (262 lines, 17 sections)
- Current Privacy Policy: `PRIVACY_POLICY.md` (428 lines, 17 sections)
- Current Support: `SUPPORT.md` (176 lines, 7 sections)
- Landing page: `index.html`

### Statutory References

- NZ Consumer Guarantees Act 1993 (as amended Jan 2026): legislation.govt.nz
- NZ Fair Trading Act 1986 (including Part 2A, effective Mar 2023): legislation.govt.nz
- NZ Privacy Act 2020: privacy.org.nz
- NZ Contract and Commercial Law Act 2017 (CCLA): legislation.govt.nz
- NZ Harmful Digital Communications Act 2015 (s.22A, s.23-24): legislation.govt.nz
- NZ Limitation Act 2010 (s.11 discoverability): legislation.govt.nz
- NZ Arms Act 1983: legislation.govt.nz
- Australian Consumer Law (ACL) Schedule 2, Competition and Consumer Act 2010: accc.gov.au
- Australian Privacy Act 1988 (Cth) including s.6C small business exemption: oaic.gov.au

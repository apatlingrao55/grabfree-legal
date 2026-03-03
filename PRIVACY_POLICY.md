# Privacy Policy

**GrabFree App**
**Last Updated: March 3, 2026**

This Privacy Policy describes how GrabFree ("we", "us", "our") collects, uses, stores, and protects your personal information when you use the GrabFree mobile application ("App").

**BY CREATING AN ACCOUNT OR USING THE APP, YOU EXPRESSLY AND VOLUNTARILY OPT IN TO THIS PRIVACY POLICY AND CONSENT TO THE COLLECTION, USE, STORAGE, AND PROCESSING OF YOUR INFORMATION AS DESCRIBED HEREIN. YOUR USE OF THE APP IS ENTIRELY VOLUNTARY. IF YOU DO NOT CONSENT TO THIS POLICY, DO NOT CREATE AN ACCOUNT AND DO NOT USE THE APP.**

---

## 1. Information We Collect

### 1.1 Information You Provide Directly

**Account Information:**

| Data | Purpose | Required |
|------|---------|----------|
| Email address | Account identification, verification, password resets | Yes |
| Username | Public display name (3-30 characters) | Yes |
| Password | Account authentication (stored as bcrypt hash, never in plain text) | Yes |

**Profile Information:**

| Data | Purpose | Required |
|------|---------|----------|
| Primary geographic area | Determines your community board and local item listings | Yes (during onboarding) |
| Additional area preferences | Access to multiple community boards | No |

**Item Listings:**

| Data | Purpose | Required |
|------|---------|----------|
| Item title | Listing display (5-200 characters) | Yes |
| Item description | Listing details | No |
| Item category | Categorization and filtering | Yes |
| Photos (up to 3) | Visual representation of items | No |
| Location text | Approximate pickup location | No |
| Geographic coordinates (latitude/longitude) | Map display and proximity features | No |
| Availability date | Scheduling when item becomes available | No |

**Communications:**

| Data | Purpose | Required |
|------|---------|----------|
| Private messages | Communication between users (up to 1,000 characters) | Voluntary |
| Board posts | Community discussion (up to 2,000 characters) | Voluntary |
| Board replies | Replies to community posts (up to 1,000 characters) | Voluntary |

**Ratings and Reviews:**

| Data | Purpose | Required |
|------|---------|----------|
| Scores (reliability, communication, accuracy, timeliness) | User reputation system (1-5 scale each) | Per transaction |
| Written comments | Feedback details (up to 500 characters) | No |

**Moderation Actions:**

| Data | Purpose | Required |
|------|---------|----------|
| Content flags | Reporting violations (reason + optional details) | Voluntary |
| Community votes (block/admit/vouch) | Community self-regulation (with reason) | Voluntary |
| Rating disputes | Contesting unfair ratings | Voluntary |

### 1.2 Information Collected Automatically

**Device and Usage Data:**

| Data | Purpose | Retention |
|------|---------|-----------|
| Push notification token | Delivering mobile notifications | Until logout or token refresh |
| Last active timestamp | Account activity tracking | Indefinite |
| API request metrics | Performance monitoring and rate limiting | 90 days |
| Daily activity counts | Aggregate usage statistics | 90 days |
| Sponsor interaction events (campaign ID, placement, event type) | Measuring sponsor campaign performance | 365 days |

**Event Logging:**

| Data | Purpose | Retention |
|------|---------|-----------|
| Registration events | Account creation tracking | 365 days |
| Login events | Security monitoring | 365 days |
| Item lifecycle events | Service analytics | 365 days |

### 1.3 Information We Do NOT Collect

- We do not collect payment or financial information
- We do not access your device contacts, calendar, or files beyond photos you explicitly select for listings
- We do not track your real-time location (area selection is manual)
- We do not sell your personal information to advertisers. Our sponsor system records anonymized impression and click events (campaign ID, placement, timestamp) to measure sponsor performance. These events are not linked to your personal profile or shared with third parties.

---

## 2. How We Use Your Information

### 2.1 Core Service Delivery

- **Account management**: Registration, authentication, email verification, password management
- **Item listings**: Displaying your listings to other users in your area
- **Messaging**: Facilitating private conversations between listing owners and interested users
- **Notifications**: Alerting you to new messages, item claims, board replies, and new items in your area
- **Community features**: Operating discussion boards, ratings, and moderation systems
- **Area-based content**: Showing relevant items and community boards based on your selected area

### 2.2 Safety and Moderation

- **Email quarantine**: Reviewing potentially suspicious email addresses during registration through peer review
- **Community voting**: Enabling established users to flag concerning behavior
- **Content moderation**: Reviewing flagged posts and reported items
- **Account enforcement**: Suspending accounts that violate our Terms of Use
- **Rate limiting**: Preventing abuse of the platform (5-100 requests per minute depending on endpoint)

### 2.3 Service Improvement

- **Aggregate analytics**: Understanding usage patterns through anonymized daily statistics (new users, active users, items created, messages sent)
- **System health monitoring**: Database size, performance metrics, error rates
- **API performance**: Response times and error tracking by endpoint

---

## 3. How We Store Your Information

### 3.1 Data Storage

| Data Type | Storage Provider | Location |
|-----------|-----------------|----------|
| Account data, messages, listings, ratings | PostgreSQL database | Cloud-hosted server |
| Item photographs | Cloudflare R2 | Cloudflare global network |
| Real-time session data (typing indicators, online status) | In-memory (server RAM) | Not persisted; lost on restart |

### 3.2 Photo Processing and Storage

When you upload photos for item listings:

1. Photos are resized to a maximum of 1200x1200 pixels
2. Compressed and converted to WebP format (quality: 80%)
3. Thumbnails are generated at 400x400 pixels (quality: 70%)
4. Auto-rotated based on EXIF orientation data
5. Original uploads are not retained after processing
6. Stored in Cloudflare R2 cloud storage
7. Permanently deleted when the associated listing is removed

### 3.3 Security Measures

- **Password hashing**: All passwords are hashed using bcrypt with a salt factor of 10 (12 for administrator accounts). We never store or have access to plain-text passwords.
- **JWT authentication**: Secure token-based authentication for all API requests
- **Rate limiting**: Protection against brute-force attacks on authentication endpoints (5 attempts per minute)
- **Parameterized queries**: All database queries use parameterized inputs to prevent SQL injection
- **Input validation**: All user inputs are validated and sanitized before processing
- **Admin 2FA**: Administrator accounts are protected with time-based one-time passwords (TOTP) and backup codes
- **Admin audit logging**: All administrator actions are logged with timestamps and IP addresses
- **Real-time transport security**: WebSocket connections require JWT authentication

---

## 4. Data Retention

### 4.1 Retention Schedule

| Data Type | Retention Period | Deletion Method |
|-----------|-----------------|-----------------|
| Account data | Until account deletion | Cascade delete on account removal |
| Item listings | 14 days after expiry or 14 days after collection | Automatic scheduled deletion |
| Item photos | Until listing is removed | Automatic deletion from Cloudflare R2 |
| Private messages | Indefinite (while account exists) | Deleted with account |
| Community board posts | Indefinite (can be hidden by moderators) | Deleted with account |
| Ratings and reviews | Indefinite (can be hidden via disputes) | Anonymized on account deletion |
| Email verification codes | 30 minutes (auto-expire) | Overwritten on new request |
| API request metrics | 90 days | Automatic cleanup |
| User activity records | 90 days | Automatic cleanup |
| Event logs | 365 days | Automatic cleanup |
| System health snapshots | 365 days | Automatic cleanup |
| Admin audit logs | Indefinite | Retained for accountability |

### 4.2 Account Deletion

When you delete your account:

- Your user record and associated personal data are removed from the database
- Related records (listings, messages, conversations, area preferences, notification preferences, community votes, board posts) are cascade-deleted
- Photos associated with your listings are deleted from cloud storage
- Ratings you have given may be anonymized rather than deleted to preserve the integrity of other users' reputation scores
- Admin audit logs referencing your account are retained with anonymized references

---

## 5. Data Sharing and Third Parties

### 5.1 Third-Party Service Providers

We use the following third-party services to operate the App:

| Service | Provider | Data Shared by Us | Data Collected by SDK | Purpose |
|---------|----------|-------------------|----------------------|---------|
| Email delivery | Resend | Email address, OTP codes | Email metadata (delivery status, bounces) | Sending verification emails and password reset codes |
| Image storage | Cloudflare R2 | Item photographs | Request metadata (IP address, access logs) | Storing and serving item images |
| Push notifications | Expo (expo.dev) | Push token, notification content | Device type, OS version, delivery status | Delivering mobile push notifications |

These providers process data on our behalf and are bound by their respective privacy policies and data processing agreements.

Under the NZ Privacy Act 2020 and the Australian Privacy Act 1988 (where applicable), we are responsible for personal information collected by third-party SDKs and services integrated into the App, even where that collection is initiated by the third-party service. Our use of these services is governed by data processing agreements where available.

### 5.2 What We Do NOT Share

- We do not sell your personal information to third parties
- We do not share your personal information (email, username, messages) with sponsors or advertisers. Sponsors receive only aggregate, anonymized performance metrics (total impressions, clicks, and click-through rates) for their campaigns.
- We do not provide your email address to other users (your username is your public identifier)
- We do not share your precise location coordinates with other users (only the area-level location and any location text you voluntarily include in listings)

### 5.3 Information Visible to Other Users

The following information is visible to other App users:

- **Public profile**: Username, account creation date, email verification status, vetted status, primary area
- **Item listings**: Title, description, category, photos, location text, area, availability date
- **Aggregate ratings**: Average scores across all categories and number of ratings received
- **Board posts and replies**: Content, username, and timestamp
- **Messages**: Visible only to the other participant in a conversation

The following is **never** visible to other users:

- Your email address
- Your password
- Your precise GPS coordinates (only area-level and voluntary location text)
- Your push notification token
- Your activity metrics or login history
- Your notification preferences

### 5.4 Administrator Access

Our administrators and moderators may access:

- User email addresses and account details (for moderation and support purposes)
- Item listings and photos (for content review)
- Reports and flags submitted by users
- Aggregate user statistics
- Community vote records (for moderation oversight)

All administrator actions are logged in an audit trail that records the administrator, action taken, target, details, and IP address.

### 5.5 Legal Requirements

We may disclose your information if required to do so by law or in response to valid requests by public authorities (e.g., court orders, subpoenas).

### 5.6 Content Takedown Notices

If you submit or are the subject of a content takedown notice (see [Terms of Use](TERMS_OF_USE.md), Section 17), we collect and retain:

- Claimant contact information and takedown notice content
- Records of notices, counter-notices, and outcomes
- Repeat infringer tracking data

This data is retained for the duration of any related dispute and for a minimum of 24 months after resolution. If a counter-notice is submitted, limited information about the claim (nature of the claim, not claimant identity) is shared with the content owner to enable them to respond. Claimant identity is not disclosed to the accused user unless required by law or with the claimant's consent.

---

## 6. Push Notifications

### 6.1 Token Collection

When you enable push notifications, your device's push notification token (Expo Push Token) is stored on our servers. This token is:

- Collected when you register for notifications
- Cleared when you log out
- Automatically removed if the token becomes invalid (e.g., app uninstalled)

### 6.2 Notification Types

| Notification | Default | Can Disable |
|-------------|---------|-------------|
| New messages | Enabled | Yes |
| Item status changes (claimed, collected) | Enabled | Yes |
| New items in your area | Enabled | Yes |
| Community board activity | Enabled | Yes |

### 6.3 Quiet Hours

You may configure quiet hours (start time, end time, and timezone) during which notifications are deferred and delivered after the quiet period ends. The default timezone is Pacific/Auckland (New Zealand).

### 6.4 Batch Processing

Area-wide notifications (e.g., new items posted) are sent in batches of up to 100 recipients and respect individual notification preferences.

---

## 7. Cookies and Tracking

The mobile App does not use cookies. Authentication is handled via JWT tokens stored securely on your device using platform-native secure storage (iOS Keychain / Android Keystore via Expo SecureStore). The web-based administration interface uses httpOnly session cookies for administrator authentication only; these are not used for tracking.

---

## 8. Data Security

### 8.1 Technical Safeguards

- Passwords are hashed with bcrypt and never stored or transmitted in plain text
- All API communication uses HTTPS encryption
- Real-time messaging uses authenticated WebSocket connections
- Rate limiting protects against automated attacks
- Database queries use parameterized inputs to prevent injection attacks
- Administrator accounts require two-factor authentication (TOTP)
- Verification codes expire after 30 minutes and are single-use
- Mobile tokens are stored in platform-native secure storage (not plain AsyncStorage)

### 8.2 Organizational Safeguards

- Administrator actions are fully audited with IP address logging
- Administrator roles are tiered (moderator, admin, super_admin) with appropriate access levels
- Database backups are maintained for disaster recovery

### 8.3 No Guarantee of Security

**WHILE WE IMPLEMENT REASONABLE SECURITY MEASURES, NO METHOD OF ELECTRONIC TRANSMISSION OR STORAGE IS 100% SECURE. WE CANNOT AND DO NOT GUARANTEE THE ABSOLUTE SECURITY OF YOUR INFORMATION.** You acknowledge that you provide your information at your own risk. We are not responsible for circumvention of any security measures contained in the App.

### 8.4 Breach Notification

In the event of a data breach that affects your personal information, we will notify affected users via email and/or in-app notification as soon as practicable, and no later than required by applicable law.

---

## 9. Your Rights

### 9.1 Access

You may access your personal information through the App at any time. Your profile, listings, messages, ratings, and notification preferences are all accessible within the App.

### 9.2 Correction

You may update your username and area preferences through the App. To update your email address, please contact us.

### 9.3 Deletion

You may delete your account at any time, which will remove your personal data as described in Section 4.2. To request account deletion, use the account deletion feature within the App or contact us directly.

### 9.4 Data Portability

You may request a copy of your personal data in a structured, commonly used format by contacting us.

### 9.5 Notification Preferences

You have full control over which notifications you receive and may configure quiet hours. You may also disable push notifications entirely through your device settings.

### 9.6 Objection

You may object to specific data processing activities by contacting us. Note that some data processing is necessary for the core operation of the App, and objecting to essential processing may require account termination.

---

## 10. Children's Privacy

The App is not intended for children under 16 years of age. We do not knowingly collect personal information from children under 16. If we discover that a child under 16 has provided us with personal information, we will promptly delete that information.

---

## 11. Email Quarantine and Peer Review

### 11.1 How It Works

Our system may automatically quarantine email addresses that match certain patterns during registration. Quarantined email addresses are reviewed by established community members (peer reviewers) who vote to approve or reject the email. This process:

- Uses only the **normalized email address** (not the full registration details)
- Requires multiple peer reviews before a decision is made
- May result in the email being approved, remaining quarantined, or being permanently blocked
- Is designed to prevent abuse while minimizing false positives

### 11.2 Your Rights

If your email is quarantined, you will be informed. You may contact us to request a manual review of your quarantine status.

---

## 12. Community Voting and Reputation

### 12.1 Voting System

Established users (verified email, vetted status, 30+ days account age, 3+ completed transactions) may cast community votes on other users. These votes:

- Are stored with the voter's identity and reason
- May affect your ability to use certain App features if block votes reach threshold levels
- Are visible to administrators for moderation oversight
- Are not publicly visible to other users

### 12.2 Ratings

Transaction ratings are publicly visible in aggregate (average scores and count). Individual rating details (scores, comments, rater identity) are visible on your profile. You may dispute ratings through the in-app dispute process.

---

## 13. International Data Transfers

Your data may be processed and stored in locations outside your country of residence, including through our use of cloud services (Cloudflare R2 for image storage, cloud-hosted databases). By using the App, you consent to the transfer of your information to these locations.

---

## 14. Changes to This Policy

We may update this Privacy Policy from time to time. We will notify you of material changes by:

- Posting the updated policy in the App
- Updating the "Last Updated" date at the top of this document
- Sending an email notification for significant changes

Your continued use of the App after changes are posted constitutes your acceptance of the revised policy.

---

## 15. Disclaimer of Liability for Data Processing

**TO THE FULLEST EXTENT PERMITTED BY APPLICABLE LAW, GRABFREE SHALL NOT BE LIABLE FOR ANY LOSS, DAMAGE, OR HARM ARISING FROM:**

- The collection, use, or storage of your information as described in this policy
- Any unauthorized access to or disclosure of your information despite our reasonable security measures
- Any inaccuracy in information provided by other users (ratings, reviews, messages, listings)
- Data loss due to service interruptions, technical failures, or discontinuation of the App
- Actions of third-party service providers (Resend, Cloudflare, Expo)
- Your decision to share personal information through messages, listings, or community posts

**You acknowledge that this is a free service and that you voluntarily provide your information. The limitations described in this section and in our [Terms of Use](TERMS_OF_USE.md) represent a fair allocation of risk for a free service.**

---

## 16. Governing Law

This Privacy Policy is governed by the laws of New Zealand, including the Privacy Act 2020. Nothing in this policy is intended to exclude or limit rights that cannot be excluded under the Privacy Act 2020 or the Australian Privacy Act 1988 (Cth) where applicable.

---

## 17. For Users in Australia

If you are located in Australia, the Australian Privacy Principles (APPs) under the Privacy Act 1988 (Cth) may apply to our collection and handling of your personal information. Nothing in this Privacy Policy is intended to limit or exclude any right you have under the APPs.

You may also lodge a privacy complaint with the Office of the Australian Information Commissioner (OAIC) at oaic.gov.au.

---

## 18. Contact Us

If you have any questions, concerns, or requests regarding this Privacy Policy or your personal data, please contact us at:

**Email**: dev@aiconsult.co.nz

For data access, correction, or deletion requests, please include your registered email address and a description of your request.

---

**BY CREATING AN ACCOUNT OR USING THE GRABFREE APP, YOU ACKNOWLEDGE THAT YOU HAVE READ AND UNDERSTOOD THIS PRIVACY POLICY AND VOLUNTARILY CONSENT TO THE COLLECTION, USE, STORAGE, AND PROCESSING OF YOUR INFORMATION AS DESCRIBED HEREIN. YOU UNDERSTAND THAT THIS IS A FREE SERVICE PROVIDED WITH NO GUARANTEES REGARDING DATA SECURITY OR AVAILABILITY.**

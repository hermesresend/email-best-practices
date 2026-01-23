---
name: email-best-practices
description: Use when building email features, emails going to spam, high bounce rates, setting up SPF/DKIM/DMARC authentication, implementing email capture, ensuring compliance (CAN-SPAM, GDPR, CASL), or deciding transactional vs marketing.
---

# Email Best Practices

Guidance for building deliverable, compliant, user-friendly emails—from authentication setup to legal compliance.

## When to Use

**Use when:** Building any email feature, setting up email infrastructure, or ensuring compliance.

**Don't use for:**
- SMS/push notification best practices
- In-app messaging

## Quick Reference

| Scenario | Primary Skill | Secondary Skills |
|----------|--------------|------------------|
| Setting up email authentication | [Deliverability](./deliverability.md) | - |
| Building password reset flow | [Transactional Emails](./transactional-emails.md) | [Catalog](./transactional-email-catalog.md) |
| Implementing OTP/2FA emails | [Transactional Emails](./transactional-emails.md) | [Catalog](./transactional-email-catalog.md) |
| Building newsletter signup | [Email Capture](./email-capture.md) | [Compliance](./compliance.md), [Marketing Emails](./marketing-emails.md) |
| Sending order confirmations | [Transactional Emails](./transactional-emails.md) | [Catalog](./transactional-email-catalog.md) |
| Ensuring GDPR compliance | [Compliance](./compliance.md) | [Email Capture](./email-capture.md) |
| Troubleshooting delivery issues | [Deliverability](./deliverability.md) | - |
| Deciding transactional vs marketing | [Email Types](./email-types.md) | [Compliance](./compliance.md) |
| Planning emails for new app | [Catalog](./transactional-email-catalog.md) | [Email Types](./email-types.md) |
| Implementing email verification | [Email Capture](./email-capture.md) | [Transactional Emails](./transactional-emails.md) |
| Building marketing campaigns | [Marketing Emails](./marketing-emails.md) | [Compliance](./compliance.md) |

## Common Mistakes

| Mistake | Why it's a problem | Fix |
|---------|-------------------|-----|
| Skipping email authentication (SPF/DKIM/DMARC) | Gmail/Yahoo reject unauthenticated emails | Set up authentication before sending any emails |
| Sending marketing emails without explicit opt-in | Violates GDPR/CASL, damages sender reputation | Use double opt-in for all marketing emails |
| Using same domain/IP for transactional and marketing | Marketing reputation issues affect critical transactional emails | Separate infrastructure for each type |
| No unsubscribe link in marketing emails | CAN-SPAM violation ($53k+ per email fine) | Include prominent unsubscribe in every marketing email |
| Treating "welcome email" as transactional | Often contains promotional content, requires opt-in | Review content; if promotional, treat as marketing |
| Ignoring bounce handling | Damages sender reputation, wastes resources | Remove hard bounces immediately, handle soft bounces with retry logic |

## Examples

**Password reset flow:** [Transactional Emails](./transactional-emails.md) → [Deliverability](./deliverability.md) for authentication

**Newsletter signup:** [Email Capture](./email-capture.md) → [Compliance](./compliance.md) → [Marketing Emails](./marketing-emails.md)

**New app email setup:** [Catalog](./transactional-email-catalog.md) → [Deliverability](./deliverability.md) → [Email Types](./email-types.md)

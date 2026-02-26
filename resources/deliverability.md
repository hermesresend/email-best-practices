# Email Deliverability

Maximizing the chances that your emails are delivered successfully to the recipients.

## Email Authentication

**Required by Gmail/Yahoo/Microsoft** - unauthenticated emails will be rejected or spam-filtered.

### SPF (Sender Policy Framework)

Specifies which servers can send email for your domain.

```
v=spf1 include:amazonses.com ~all
```

- Add TXT record to DNS
- Use `~all` (soft fail)

### DKIM (DomainKeys Identified Mail)

Cryptographic signature proving email authenticity.

- Your email service will provide you with a TXT record

### DMARC

Policy for handling SPF/DKIM failures + reporting.

```
v=DMARC1; p=none; rua=mailto:dmarc@yourdomain.com
```

**Rollout:** `p=none` (monitor) → `p=quarantine; pct=25` → `p=reject`

Learn more: https://resend.com/blog/dmarc-policy-modes 

### Verify Your Setup

Check DNS records directly:

```bash
# SPF record
dig TXT yourdomain.com +short

# DKIM record (replace 'resend' with your selector)
dig TXT resend._domainkey.yourdomain.com +short

# DMARC record
dig TXT _dmarc.yourdomain.com +short
```

**Expected output:** Each command should return your configured record. No output = record missing.

## Sender Reputation

### IP Warming

New IP/domain? Gradually increase volume:

| Week | Daily Volume |
|------|-------------|
| 1 | 50-100 |
| 2 | 200-500 |
| 3 | 1,000-2,000 |
| 4 | 5,000-10,000 |

Start with engaged users. Send consistently. Don't rush.

Learn more: https://resend.com/docs/knowledge-base/warming-up

### Maintaining Reputation

**Do:** Send to engaged users, keep bounce <4%, complaints <0.1%, remove inactive subscribers.

**Don't:** Send to purchased lists, ignore bounces/complaints, send inconsistent volumes

## Bounce Handling

| Type | Cause | Action |
|------|-------|--------|
| Hard bounce | Permanent failure to deliver | Remove immediately |
| Soft bounce | Transient failure to deliver | Retry: 1h → 4h → 24h, remove after 3-5 failures |

**Targets:** <1% good, 1-3% acceptable, 3-4% concerning, >4% critical

## Complaint Handling

**Targets:** <0.01% excellent, 0.01-0.05% good, >0.05% critical

**Reduce complaints:**
- Only send to opted-in users
- Make unsubscribe easy and immediate
- Use clear sender names and "From" addresses

**Feedback loops:** Set up with Gmail (Postmaster Tools), Yahoo, Microsoft SNDS. Remove complainers immediately.

## Infrastructure

**Dedicated sending domain:** Use different subdomains for different sending purposes (e.g., `t.yourdomain.com` for transactional emails and `m.yourdomain.com` for marketing emails). 

**DNS TTL:** Low (300s) during setup, high (3600s+) after stable.

## Troubleshooting

**Emails going to spam?** Check in order:
1. Authentication (SPF, DKIM, DMARC)
2. List-Unsubscribe header — required by Gmail/Yahoo since Feb 2024 (see [Compliance](./compliance.md))
3. Sender reputation (blacklists, complaint rates)
4. Content
5. Sending patterns (sudden volume spikes)

**Diagnostic tools:**
- [Google Postmaster Tools](https://postmaster.google.com) - Domain reputation and spam rates
- [mail-tester.com](https://www.mail-tester.com) - Send a test email, get deliverability score
- [MXToolbox](https://mxtoolbox.com/blacklists.aspx) - Check blacklist status

## Resend Deliverability Insights

Resend provides built-in deliverability checks on every sent email. In the dashboard, click an email → "Insights" to see pass/fail checks:

- **Link URLs match sending domain** — mismatched URLs trigger spam filters
- **DMARC record is valid** — required by Gmail and Yahoo since 2024
- **Unsubscribe header present** — required for bulk senders
- **Text version included** — improves deliverability over HTML-only
- **Email size** — keep under 100KB for best results

### Resend Suppressions

When you send to a recipient that previously hard-bounced or marked your email as spam, Resend proactively blocks the delivery (suppression).

- Suppression list is **per region** — a bounce on any domain in your region suppresses the address across all domains in that region
- Gmail/Google Workspace doesn't return complaint events
- You can view and remove addresses from the suppression list in the dashboard

### "Delivered but Not Received"

If Resend shows `delivered` but the recipient doesn't see the email:

1. **Check spam/junk folder** — most common cause
2. **Check suppression list** — may be suppressed from a previous bounce
3. **Verify recipient address** — typos are surprisingly common
4. **Check email client filters/rules** — auto-archiving or deleting
5. **Apple Private Relay** — Apple hides real addresses; the relay address must be valid
6. **Check Resend dashboard** for detailed status info

### Domain Setup

For domain verification (SPF/DKIM/MX), DMARC progressive rollout, and BIMI setup, use the `domain-setup` skill from `resend/resend-skills`.

### Error Troubleshooting

For API errors (403, 422, 429, 500), CORS issues, and delivery debugging, use the `error-troubleshooting` skill from `resend/resend-skills`.

## Related

- [List Management](./list-management.md) - Handle bounces and complaints to protect reputation
- [Sending Reliability](./sending-reliability.md) - Retry logic and error handling
- [Resend Domain Setup](https://resend.com/docs/dashboard/domains/introduction) - Domain verification docs
- [Resend Deliverability Insights](https://resend.com/docs/dashboard/emails/deliverability-insights) - Dashboard feature docs

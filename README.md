# Email Best Practices Agent Skill

Best practices for building and sending emails that are deliverable, compliant, and user-friendly. This agent skill provides comprehensive guidance on email deliverability, compliance, email types, and responsible email capture.

## Overview

This skill covers everything you need to know about implementing email features correctly, from setting up email authentication to ensuring legal compliance. It's designed to help AI agents provide accurate, actionable guidance when building email features.

## Skills Included

### Deliverability
Email authentication (SPF, DKIM, DMARC, BIMI), sender reputation, bounce handling, monitoring, and infrastructure best practices.

### Email Types
Understanding the difference between transactional and marketing emails, when to use each, and a comprehensive catalog of transactional email types.

### Transactional Emails
Best practices for transactional emails including subject lines, content structure, mobile-first design, and timing considerations.

### Marketing Emails
Guidelines for marketing emails including opt-in requirements, unsubscribe mechanisms, content design, segmentation, and A/B testing.

### Compliance
Legal requirements for email sending including CAN-SPAM, GDPR, CASL, and other regional requirements.

### Email Capture
Guidance on email validation techniques, verification APIs, double opt-in vs single opt-in, form design, and error handling.

## Usage

This skill follows the [Agent Skills format](https://github.com/vercel-labs/agent-skills). The main entry point is `SKILL.md`, which provides an overview and guides agents to the appropriate detailed guidance based on the task.

**When to use this skill:**
- Building email features (transactional or marketing)
- Setting up email infrastructure and authentication
- Implementing email capture and verification
- Ensuring legal compliance for email sending
- Optimizing email deliverability
- Choosing between transactional and marketing emails
- Designing email templates and content

## File Structure

```
email-skills/
├── SKILL.md                    # Main skill file with frontmatter
├── deliverability.md           # Authentication, monitoring, infrastructure
├── email-types.md              # Transactional vs marketing, email catalog
├── transactional-emails.md     # Best practices for transactional emails
├── marketing-emails.md         # Best practices for marketing emails
├── compliance.md               # Legal compliance (CAN-SPAM, GDPR, etc.)
├── email-capture.md            # Validation, verification, opt-in
└── README.md                   # This file
```

## Installation

This skill can be used with agent systems that support the Agent Skills format. Refer to your agent system's documentation for installation instructions.

## License

MIT

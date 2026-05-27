# Email Accessibility

Emails must be readable by screen readers, dark-mode clients, translation tools, and AI agents — not just sighted readers on a default inbox. The rules below are mechanical. Apply them every time.

## Rules

### Set `lang` and `dir` on `<body>`

Always include both. Without them, screen readers guess pronunciation and translators misfire.

```html
<body lang="en" dir="ltr">
  ...
</body>
```

- `lang`: a [BCP 47 language tag](https://developer.mozilla.org/en-US/docs/Glossary/BCP_47_language_tag) (`en`, `pt-BR`, `ja`, `ar`)
- `dir`: `ltr`, `rtl`, or `auto` as a last resort

For multi-locale templates, pass the locale through — do not hardcode `en`.

### Mark layout tables as presentational

Any `<table>` used for layout must have `role="presentation"`. Otherwise screen readers announce "table, row 1 of N" for every layout row and the email becomes unusable.

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0">
  <tr>
    <td>...</td>
  </tr>
</table>
```

Only leave a `<table>` without `role="presentation"` when the data is genuinely tabular (line items, comparison rows). Tabular data should also use `<th scope="col">` for column headers.

### Use a single `<h1>` and nest headings in order

Every email has exactly one `<h1>` that names the email. Subheadings nest in order — `<h1>` → `<h2>` → `<h3>`. Never skip levels. Never fake a heading with bold `<p>`.

```html
<h1>Order confirmation</h1>
  <h2>Items</h2>
  <h2>Shipping</h2>
    <h3>Address</h3>
    <h3>Tracking</h3>
```

Headings are how assistive tech and AI clients navigate and summarize the email. They are not a styling choice.

### Write descriptive link text

Link text must describe the destination. Never use "click here," "learn more," "read more," or bare URLs.

```html
<!-- Wrong -->
<a href="...">click here</a>
<a href="...">https://resend.com/blog/...</a>

<!-- Right -->
<a href="...">Read the 2026 accessibility report</a>
```

### Write meaningful alt text — and use `alt=""` for decorative images

Two distinct rules, both mandatory.

**Meaningful images** (product shots, charts, screenshots, anything carrying information): describe the purpose and key details in context.

```html
<!-- Wrong: redundant, vague -->
<img src="..." alt="image">
<img src="..." alt="photo of a bike">

<!-- Right: purpose + key details -->
<img src="..." alt="Red bicycle leaning against a brick wall on a rainy street">
```

**Decorative images** (spacers, dividers, background flourishes, pure branding ornaments): use an empty `alt=""`. This tells screen readers to skip them cleanly. Never omit the `alt` attribute entirely.

```html
<img src="divider.png" alt="" role="presentation">
```

If an image conveys no information that isn't already in the surrounding text, it is decorative.

### Include a `<title>` tag

Many clients and assistive technologies read `<title>` before anything else. Treat it like the subject line, not the brand name.

```html
<head>
  <title>Your weekly product updates from Resend</title>
</head>
```

### Hit 4.5:1 color contrast, then check dark mode

- Body text and links: **4.5:1** minimum against the background (WCAG AA)
- Large text (≥18pt, or ≥14pt bold): **3:1** minimum
- Never rely on color alone to convey meaning (error states, status badges) — pair it with text or an icon

Verify with the [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) or browser devtools.

**Dark mode.** Outlook, Apple Mail, and others force dark mode and derive dark colors from your light ones. Healthy starting contrast keeps the auto-inverted version readable. Always preview in dark mode before shipping.

## Authoring checklist

Run this on every template:

- [ ] Exactly one `<h1>`, with `<h2>`/`<h3>` nested in order — no skipped levels
- [ ] Every meaningful image has descriptive `alt`; every decorative image has `alt=""`
- [ ] Every link describes its destination — no "click here," "learn more," or bare URLs
- [ ] Body text passes 4.5:1 contrast and stays readable in dark mode
- [ ] `<title>` set on `<head>`, specific to this email (not the brand name)
- [ ] `<body>` has both `lang` and `dir` set to the email's actual locale
- [ ] Layout `<table>` elements have `role="presentation"`
- [ ] Plain-text alternative is sent alongside the HTML version

## Testing

- **Screen reader pass.** macOS VoiceOver (`Cmd+F5`) or NVDA on Windows. Listen top to bottom; if anything is confusing, fix the markup.
- **Contrast.** [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/).
- **Dark mode.** Send a test to Outlook (Windows/web), Apple Mail with dark appearance, Gmail iOS and Android.
- **Automated checks.** [emailmarkup.org/checker](https://emailmarkup.org/en/checker/) flags most of the above.

## Related

- [Transactional Emails](./transactional-emails.md) — content patterns for password resets, OTPs, receipts
- [Marketing Emails](./marketing-emails.md) — newsletter and campaign best practices
- [Compliance](./compliance.md) — legal requirements that overlap with accessibility (e.g., clear unsubscribe text)

## Tooling

When generating templates with React Email, the latest version handles several of the structural rules: `<Html>` sets `lang`/`dir`, `<Img>` defaults to `alt=""`, `<Markdown>` tables render `role="presentation"`, and `<Preview>` emits a `<title>`. Upgrade with `npm install react-email@latest`. The content rules — heading hierarchy, descriptive alt and link text, contrast — still have to be applied by hand.

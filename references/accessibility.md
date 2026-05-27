# Email Accessibility

Making emails readable for everyone — assistive tech, dark-mode clients, translation tools, AI agents, and people with low vision.

The Email Markup Consortium's [2026 accessibility report](https://emailmarkup.org/en/reports/accessibility/2026/) analyzed 376,348 emails. Only 8 passed every check (0.002%). Most failures are mechanical and easy to fix.

## Why it matters

- **Assistive tech.** Screen readers, braille displays, magnifiers, RTL languages. Structure is navigation, not decoration.
- **Every reader.** Phone in sunlight, client-forced dark mode, translated content.
- **Agents and AI.** Modern clients summarize and extract content. Accessible email is machine-readable email.

## The six fixes

### 1. Set `lang` and `dir` on `<body>`

Missing on **~96%** of emails. Without them, screen readers guess pronunciation and translators stumble.

```html
<body lang="en" dir="ltr">
  ...
</body>
```

- `lang`: [BCP 47 language tag](https://developer.mozilla.org/en-US/docs/Glossary/BCP_47_language_tag) (`en`, `pt-BR`, `ja`)
- `dir`: `ltr`, `rtl`, or `auto` as a last resort

### 2. Mark layout tables as presentational

**84%** of emails get this wrong. Without `role="presentation"`, screen readers announce "table, row 1 of 6" for every layout row.

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0">
  <tr>
    <td>...</td>
  </tr>
</table>
```

Only use a plain `<table>` when the data is actually tabular (orders, line items, etc.) — those should be left as real tables with proper headers.

### 3. Use a semantic heading outline

**74%** of emails have no `<h1>`. Headings give screen readers, agents, and clients a map of your content.

```html
<h1>Order confirmation</h1>
  <h2>Items</h2>
  <h2>Shipping</h2>
    <h3>Address</h3>
    <h3>Tracking</h3>
```

- One `<h1>` per email
- Nest in order — never skip levels (no `<h1>` → `<h3>`)
- Don't fake headings with bold `<p>` tags

### 4. Write descriptive link and alt text

**Links** should describe their destination.

```html
<!-- Bad -->
<a href="...">click here</a>

<!-- Good -->
<a href="...">read the full report</a>
```

**Images** need alt text that conveys purpose and important details.

```html
<!-- Bad -->
<img src="..." alt="photo">
<img src="..." alt="image of a bike">

<!-- Good -->
<img src="..." alt="A red bicycle leaning against a brick wall on a rainy street">
```

**Decorative images** (spacers, divider lines, background flourishes) should use an empty `alt=""` so screen readers skip them cleanly. Never omit the attribute entirely.

```html
<img src="divider.png" alt="" role="presentation">
```

### 5. Include a `<title>` tag

Missing on **45%** of emails. Many clients and assistive technologies read it first.

```html
<head>
  <title>Weekly product updates from Resend</title>
</head>
```

Describe the specific email content — treat it like the subject line, not the brand.

### 6. Hit 4.5:1 color contrast

Over half of emails fail WCAG AA contrast. Use the [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/).

- Body text and links: **4.5:1** minimum
- Large text (≥18pt or ≥14pt bold): **3:1** minimum
- Don't rely on color alone to convey meaning (errors, status badges)

**Dark mode.** Some clients (Outlook, Apple Mail) force dark mode and derive dark colors from your light ones. Healthy starting contrast makes the auto-inverted version more likely to stay accessible. Preview in dark mode before shipping.

## Content checklist

Defaults handle structure, but content choices are on you:

- [ ] One `<h1>` that names the email, then nested headings in order
- [ ] Every meaningful image has descriptive `alt`; decorative images have `alt=""`
- [ ] Every link says where it goes — no "click here", "learn more", or bare URLs
- [ ] Body text passes 4.5:1 contrast; preview in dark mode
- [ ] `<title>` set on `<head>`, specific to the email
- [ ] `lang` and `dir` set on `<body>` to match the content
- [ ] Plain text alternative provided alongside the HTML version

## Testing

- **Screen reader pass.** macOS VoiceOver (`Cmd+F5`) or NVDA on Windows. Listen to your email read top to bottom.
- **Contrast.** [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) or browser devtools.
- **Dark mode.** Send a test to Outlook (Windows/web), Apple Mail with dark appearance, Gmail iOS/Android.
- **HTML validation.** [emailmarkup.org/checker](https://emailmarkup.org/en/checker/) flags many of these issues automatically.

## Related

- [Transactional Emails](./transactional-emails.md) — content patterns for password resets, OTPs, receipts
- [Marketing Emails](./marketing-emails.md) — newsletter and campaign best practices
- [Compliance](./compliance.md) — legal requirements that overlap with accessibility (e.g., clear unsubscribe text)

## Tooling

If you're building templates with React Email, the latest version ships accessibility defaults (`<Html>` sets `lang`/`dir`, `<Img>` defaults `alt=""`, `<Markdown>` tables use `role="presentation"`, `<Preview>` emits a `<title>`). Upgrade with `npm install react-email@latest` and the structural fixes happen for free — content choices (headings, alt text, link copy, contrast) are still yours.

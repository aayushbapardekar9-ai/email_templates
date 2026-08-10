# Responsive HTML Email Templates

A set of production-style marketing email templates — promo, newsletter, and welcome/onboarding — built with the constraints that real email clients impose, not standard web development practices.

**Live demo:** https://<your-username>.github.io/email-templates/

## Why email HTML is different

Email clients don't render HTML the way browsers do. Outlook desktop (2007–2021) uses Microsoft Word's rendering engine, which ignores modern CSS layout entirely — no flexbox, no grid. Gmail strips `<style>` blocks in certain contexts. There's no universal support for external stylesheets. So these templates are built the way email developers actually build for production:

- **Table-based layout** — nested `<table>` elements instead of `<div>`s, since tables are the one layout method every major client supports
- **Inline CSS** on anything that must render correctly, since `<style>` blocks aren't reliably honored everywhere
- **MSO conditional comments** (`<!--[if mso]>`) to serve Outlook-specific fixes, including a VML fallback so buttons render as real buttons instead of plain text links
- **A single `<style>` block** reserved only for what inline CSS can't do: mobile media queries and Outlook conditionals
- **Hidden preheader text** controlling the preview snippet shown next to the subject line in the inbox
- **600px max-width container**, the standard safe width, with a media query that stacks multi-column sections and widens buttons on mobile screens

## Templates

| Template | Description |
|---|---|
| [`promo.html`](promo.html) | Sale announcement with hero image, CTA button, and a 3-column feature row |
| [`newsletter.html`](newsletter.html) | Multi-article layout with image + text blocks and a link list |
| [`welcome.html`](welcome.html) | Onboarding email with numbered setup steps and a CTA |

## Tech

- Pure HTML with inline CSS — no build step, no framework
- Tested for rendering across Gmail (web + mobile), and Outlook, using real test sends
- Hosted on GitHub Pages

## Folder structure

```
email-templates/
├── index.html          # Landing page linking all three templates
├── promo.html
├── newsletter.html
├── welcome.html
└── Images/              # Image assets referenced by the templates
```

## Testing approach

Each template was tested by sending the actual HTML source as a real email (via SMTP, not a browser copy-paste) to verify rendering matched the source exactly — checking button appearance, image loading, mobile stacking, and preheader text — across Gmail desktop, Gmail mobile, and Outlook.

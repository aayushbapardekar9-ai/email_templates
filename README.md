# Responsive HTML Email Templates

Three production-style templates, built the way real email marketing teams build them: table-based layout, inline CSS, Outlook conditional comments, and a mobile media query for stacking on small screens.

- `promo.html` — Sale/promo email with product image, CTA button, 3-column feature row
- `newsletter.html` — Multi-article newsletter with image+text blocks and a link list
- `welcome.html` — Onboarding email with numbered steps and a CTA

## Why these are built this way (worth knowing for the interview)

- **Tables, not divs.** Outlook desktop (2007–2021) renders emails with Word's HTML engine, which ignores modern CSS layout (flexbox/grid) entirely. Nested `<table>` elements are the only layout method that survives every major client.
- **Inline CSS for anything that must render.** Gmail strips `<style>` blocks in some contexts (webmail, older Android). Only put things in `<style>` that are "nice to have" (media queries, hover states) — never put load-bearing styles there.
- **`<style>` block still included** for two things inline CSS can't do: `@media` queries (mobile stacking) and MSO conditional comments (Outlook-specific fixes).
- **Bulletproof buttons**: real `<a>` tags for Gmail/Apple Mail/mobile, wrapped in `<!--[if mso]>` VML fallback so Outlook desktop renders a proper button instead of a plain blue link.
- **Preheader text**: the hidden `<div>` right after `<body>` controls the preview snippet next to the subject line in the inbox — most templates people find online skip this.
- **`width="600"` + `max-width:600px`**: 600px is the safe universal width; the `width` attribute is the fallback for clients that ignore CSS, `max-width` is what actually makes it fluid on mobile.

## Step 1 — Preview locally

No server needed. Just open the files directly:

```bash
open promo.html        # Mac
start promo.html        # Windows
xdg-open promo.html     # Linux
```

Resize your browser window down to ~375px wide to see the mobile media query kick in (columns stack, button goes full-width).

## Step 2 — Test in real email clients

Browser preview only gets you ~80% of the way — Outlook and Gmail have quirks a browser won't show you. Two free ways to catch the rest:

### Option A: Send to yourself (fastest, free)
1. In Gmail, compose a new email.
2. Click the `<>` "Insert code snippet" isn't native to Gmail compose — instead, use your email client's **"View source" / "Show original"** trick in reverse: install a browser extension like **"HTML Editor for Gmail"** or simply:
   - Open the `.html` file in Chrome
   - Select All (Ctrl/Cmd+A) → Copy
   - Paste directly into the Gmail compose window (Gmail preserves most inline HTML/CSS when pasted this way)
3. Send it to your own Gmail address, then open it on both **desktop Gmail** and the **Gmail mobile app** — check that the button, images, and stacking look right.
4. Forward the same email to an Outlook.com or Yahoo address if you have one, for a second data point.

### Option B: Litmus / Email on Acid (free trial, most thorough)
1. Sign up for a free trial at [litmus.com](https://litmus.com) or [emailonacid.com](https://www.emailonacid.com).
2. Paste your HTML into their editor (or send a test email to the unique address they give you).
3. You'll get back screenshots of your email rendered in ~40+ real clients: Outlook 2016/2019/365, Gmail (web/iOS/Android), Apple Mail, Yahoo, etc.
4. This is genuinely what agencies use before every campaign send — worth mentioning by name in an interview.

### What to specifically check
- Does the button render as a button in **Outlook desktop** (not just a blue underlined link)?
- Do images have `alt` text and not break layout if they fail to load (many corporate inboxes block images by default)?
- Does the 3-column row / image+text row **stack vertically** on a phone screen?
- Is the preheader text (the snippet next to the subject line) showing the right preview copy, not the "view in browser" link?

## Step 3 — Host on GitHub Pages (live shareable link)

This gives you a real URL to put on your resume/portfolio instead of just attaching files.

```bash
# 1. Create a new repo on github.com, e.g. "email-templates"
#    (do this on github.com — "New repository" — public, no README)

# 2. From this folder:
cd email-templates
git init
git add .
git commit -m "Add responsive email templates: promo, newsletter, welcome"
git branch -M main
git remote add origin https://github.com/<your-username>/email-templates.git
git push -u origin main

# 3. Turn on Pages
#    GitHub repo → Settings → Pages → Source: "Deploy from a branch"
#    Branch: main, folder: / (root) → Save
```

Your templates will be live within ~1 minute at:

```
https://<your-username>.github.io/email-templates/promo.html
https://<your-username>.github.io/email-templates/newsletter.html
https://<your-username>.github.io/email-templates/welcome.html
```

Add an `index.html` (optional) that just links to all three, so the root URL isn't a 404 — happy to generate that for you if you want it.

## Step 4 — What to say about this in an interview

- "I built these as raw HTML with inline CSS specifically because email clients don't support external stylesheets or modern CSS layout — Outlook still renders with Word's engine, so tables are the only reliable layout method."
- "I used bulletproof buttons with VML fallback for Outlook, and tested rendering across clients using [Litmus / send-to-self method]."
- "I made them mobile-responsive with a media query that stacks multi-column sections and widens the CTA button on small screens."
- Have the GitHub Pages link ready to pull up live on your phone during the call — that's a stronger signal than a PDF or screenshot.

## Notes on the placeholder images

The templates currently use `placehold.co` placeholder images so they render immediately with no setup. Before sending real campaigns, replace the `src="https://placehold.co/..."` URLs with your own hosted images (GitHub Pages can host these too — drop them in an `/images` folder in the repo and reference them with a relative or full GitHub Pages URL).

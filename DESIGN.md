# TradeREA Email Design System

Light-first onboarding drip templates aligned with the TradeREA homepage (white canvas, teal accents, soft grey panels).

| Template | HTML | Plain text | ESP |
|----------|------|------------|-----|
| Welcome | `welcome.html` | `welcome.txt` | Brevo merge tags |
| Finish signup | `finish.html` | `finish.txt` | Static |
| KYC reminder | `KYC.html` | `KYC.txt` | Static |
| KYC complete | `KYC-complete.html` | `KYC-complete.txt` | Brevo merge tags |
| Referral | `referral.html` | `referral.txt` | Static |

`finishh.html` is retired — use `finish.html`.

---

## Design rationale

Inspired by traderea.com: airy SaaS layout, generous whitespace, teal primary actions, no navy full-bleed bands.

### Palette (light-first)

| Token | Value | Role |
|-------|-------|------|
| Page bg | `#f5f8fb` | Soft outer wash |
| Card | `#ffffff` | Main email body |
| Soft panel | `#f0f7fa` | Steps, reward badge, muted bands |
| Heading | `#1a2332` | Bold headlines |
| Body | `#5b6472` | Secondary copy |
| Primary CTA | `#0891b2` | Pill button (site Sign Up) |
| Accent | `#06b6d4` | Highlights / step numbers |
| Border | `#e2e8f0` | Hairline dividers |
| Footer bg | `#f1f5f9` | Light grey footer |
| Footer text | `#64748b` | Muted links |

Near-black (`#1a2332`) and soft page wash (`#f5f8fb`) avoid pure `#000` / `#FFF` forced-invert issues. Body contrast targets WCAG AA (≥4.5:1).

### Type scale

| Role | Size | Line height |
|------|------|-------------|
| Hero H1 | 28px | 36px |
| Section H2 | 20px | 28px |
| Body | 16px | 26px (≥1.5) |
| Small / footer | 14px / 12px | 21px / 20px |

Font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif` (MSO forces Arial).

### Spacing

8px rhythm: **16 / 24 / 32 / 40 / 48**. Content max-width **600px**. Single-column; intro + badge stacks on mobile via `.stack`.

### CTA

One primary action. Teal pill buttons (`border-radius: 999px`), ≥44px tall, table-based with Outlook `v:roundrect` (high `arcsize`). Secondary actions are text links only.

### Imagery

**Header banner (required on all HTML emails):**

`https://traderea-public-assets.s3.ca-central-1.amazonaws.com/gallery/beaaed29-ba79-44bc-813b-20311c594fec.jpg`

- Full content width **600px**, edge-to-edge inside the card (no side padding on the header row so blue rules span the full width)
- `display:block`, `width="600"`, height auto, linked to `https://traderea.com/`
- Alt text: `TradeREA — Liquidity for Real Estate Assets`

**Footer:** text-only — no logo image.

No navy logo swap, no CSS background-image reliance. Copy never depends on images (H1 + body carry meaning when images are blocked).

### Footer

Light grey (`#f1f5f9`), reason for email, support/socials, unsubscribe. No logo, no legal company name or street address.

### Dark mode (defensive only)

Design intent is light. Keep `color-scheme: light dark` meta plus light `prefers-color-scheme` / `[data-ogsc]` tweaks so forced-dark clients do not invert badly. Do **not** introduce a second navy theme.

---

## Brevo merge tags

### Welcome (`welcome.html`)

| Tag | Use |
|-----|-----|
| `{{ contact.FIRSTNAME }}` | Greeting |
| `{{ unsubscribe }}` | Unsubscribe URL |

Welcome CTA href is hardcoded: `https://traderea.com/login`.

### KYC complete (`KYC-complete.html`)

| Tag | Use |
|-----|-----|
| `{{ contact.FIRSTNAME }}` | Congratulations line |
| `{{ unsubscribe }}` | Unsubscribe URL |

Dashboard CTA → `https://traderea.com/dashboard`; Settings → `https://traderea.com/settings`. Reward copy uses **5 TRU tokens**.

Finish, KYC reminder, and Referral: static (no Brevo tags). Referral Invite now → `https://traderea.com/rewards`; finish/KYC reminder → `https://traderea.com/signup`; `#unsubscribe` placeholder.

---

## Known client quirks and fallbacks

| Client / quirk | Fallback |
|----------------|----------|
| Outlook Windows | MSO 96 DPI, VML pill buttons, Arial force |
| Outlook.com dark | Minimal `[data-ogsc]` / `[data-ogsb]` text/bg tweaks |
| Apple Mail dark | Subtle `@media (prefers-color-scheme: dark)` |
| Gmail / Outlook mobile invert | Soft greys; solid teal CTA; no navy bands |
| Gmail clipping | HTML well under 100KB |
| Web fonts blocked | System stack only |
| Image blocking | Alt text; readable without images |
| Narrow mobile | Fluid 600→100%, stacked columns, 16px body |

---

## Testing checklist

### Litmus or Email on Acid
- [ ] Gmail (web + iOS/Android)
- [ ] Apple Mail macOS + iPhone (light + dark)
- [ ] Outlook 2016 / 2019 / 365 Windows
- [ ] Outlook.com + new Outlook
- [ ] Yahoo, Samsung Mail, Proton Mail
- [ ] Images off / plain-text multipart

### Manual spot checks
- [ ] Reads as a **light** email (white card, light footer — no navy bands)
- [ ] Teal pill CTA ≥44px; Outlook button clickable
- [ ] Preheader correct; welcome Brevo tags resolve
- [ ] Finish + KYC CTA → `https://traderea.com/signup`
- [ ] Footer unsubscribe present
- [ ] Contrast AA on body text

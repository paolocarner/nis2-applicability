# NIS2 Applicability Self-Assessment Tool v1.0

## Purpose

Allows EU-based companies to self-assess whether NIS2 (Directive 2022/2555) likely applies to them, based on: country of headquarters, sector, employee count, and annual revenue.

---

## ⚠️ Important Limitations

1. NIS2 has been transposed differently across EU member states. This tool uses best-available knowledge of transposition status as of mid-2025. Always verify against your national competent authority.
2. Some countries (e.g. Netherlands) had not fully transposed NIS2 as of the tool knowledge date. Status may have changed.
3. "Criticality override" entities may be in scope regardless of size.
4. **This tool is NOT legal advice.** Output must be reviewed by qualified legal/compliance professionals before acting on it.
5. **Email sending is NOT implemented** in this static file. The `sendEmail()` function is a stub — wire it to your backend (SendGrid, Mailchimp, etc.) before deploying. Collecting emails without a working privacy notice and GDPR-compliant consent mechanism is a compliance risk.

---

## Architecture

- Pure HTML/CSS/JS — no dependencies, no build step
- Step-by-step wizard with progress bar
- Logic engine: `countryData[]` + `sectorData[]` + threshold rules
- Result engine: 4 outcome states (`in-scope`, `possibly`, `exempt`, `check-law`)
- Email stub: clearly marked for backend integration

---

## Security Notes

- All user input is escaped before rendering via `escapeHtml()`
- No `eval()` or `innerHTML` with raw user strings
- Email input validated client-side (format only, not deliverability)
- No `localStorage` / `sessionStorage` used
- No external scripts loaded (only Google Fonts CSS)
- XSS mitigated throughout

---

## Known Weaknesses / TODO

- Transposition data will go stale — needs a maintenance workflow
- "Criticality override" (Art. 2(2)) for critical entities regardless of size is flagged but not deeply modelled per-country
- Does not model NIS2 Art. 2(2)(a)–(f) special cases (critical sole providers, national security implications, etc.) — needs human review
- Revenue threshold uses EU definition (net turnover, not EBITDA)
- Multi-country operations: tool only asks for HQ country — cross-border entities need separate advice

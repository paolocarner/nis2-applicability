# NIS2 Readiness Assessment Tool

A free, client-side web tool that helps EU organisations determine whether NIS2 applies to them and assess their current security posture against the 10 Article 21(2) requirements.

Built and maintained by [BARE Consult](https://www.bare-consult.com).

**Live tool:** https://paolocarner.github.io/nis2-applicability

---

## What it does

The tool runs a 15-question wizard in three phases:

**Phase 1 — Scope (5 questions)**
Determines whether NIS2 applies based on country, sector, employee count, and turnover. Uses the EU SME definition (medium: ≥50 employees and ≥€10M turnover; large: ≥250 employees or ≥€50M turnover). Covers all 27 EU member states with current transposition status as of mid-2025.

Four possible scope verdicts:
- **Likely In Scope** — meets sector and size thresholds in a country where NIS2 is law
- **Possibly In Scope** — size uncertain, Art. 2(2) override may apply, or law is imminent
- **Likely Exempt** — below size thresholds or sector not listed
- **Law Status Unclear** — country transposition delayed or in draft

**Phase 2 — Security Posture (10 questions)**
One question per NIS2 Article 21(2) measure. Three-option answers: Not started / Partial / Implemented. Exempt and check-law users can skip this phase — it is optional for them.

Measures covered:
1. Risk management framework — Art. 21(2)(a)
2. Incident response and reporting — Art. 21(2)(b), Art. 23
3. Business continuity and backup — Art. 21(2)(c)
4. Supply chain security — Art. 21(2)(d)
5. Network and systems security — Art. 21(2)(e)
6. Security effectiveness assessment — Art. 21(2)(f)
7. Training and cyber hygiene — Art. 21(2)(g), Art. 20(2)
8. Cryptography and encryption — Art. 21(2)(h)
9. Access control and HR security — Art. 21(2)(i)
10. Governance and secure communications — Art. 20, Art. 21(2)(j)

**Phase 3 — Contact**
Email address and consent. Unlocks the result page.

**Result page**
- Animated readiness score gauge (0–100) with band label: Critical / Developing / Progressing / Advanced
- NIS2 scope verdict card and country transposition status card
- Article 21(2) gap assessment — horizontal bar chart, colour-coded per control
- Personalised recommendations sorted by priority (highest gap first), each mapped to its Article reference
- Link to the free [NIS2 SME Toolkit](https://github.com/paolocarner/nis2-sme-toolkit)
- €1,500 fixed-fee assessment CTA with Calendly booking link (shown for in-scope and possibly users only)

---

## Architecture

Single HTML file — no framework, no build step, no dependencies.

| Concern | Approach |
|---|---|
| Fonts | IBM Plex Serif + Sans + Mono via Google Fonts |
| Lead capture | Formspree (JSON POST, silent — customer never sees status) |
| Customer email | None — result page directs to info@bare-consult.com |
| Booking | Calendly embed link |
| Storage | None — no localStorage, no cookies, no tracking |
| Deployment | GitHub Pages |

---

## Data and limitations

**Country transposition status** is accurate as of mid-2025. It will go stale — review quarterly and update the `COUNTRIES` array in the JS.

**Scope logic** covers the standard size-threshold rules and the Art. 2(2) critical-entity override for six sectors (banking, financial market infrastructure, drinking water, digital infrastructure, ICT management, public administration). It does not model:
- Art. 2(2)(a)-(f) special cases in full
- Multi-country group structures (only HQ country is assessed)
- National variations introduced during transposition

**Posture score** is entirely self-reported. It is a screening indicator, not a certified compliance determination. The disclaimer on the result page makes this explicit.

**Revenue threshold** uses net turnover per the EU SME definition — not EBITDA, gross revenue, or other measures.

---

## Deployment

The tool is a single `index.html` file deployed via GitHub Pages from the root of the repository.

To deploy your own copy:
1. Fork the repository
2. Enable GitHub Pages (Settings → Pages → Deploy from branch → main → / (root))
3. Update the Formspree endpoint in `submitToFormspree()` with your own form ID
4. Update the Calendly URL and contact email if needed

---

## Lead capture setup (Formspree)

The tool submits assessment data to Formspree on result page load. To activate:
1. Create a free account at [formspree.io](https://formspree.io)
2. Create a new form and copy the form ID
3. Replace `mlgpjnjz` in the `submitToFormspree()` function with your form ID
4. Submit a test from the live GitHub Pages URL to confirm the endpoint (Formspree requires confirmation from a real domain — `file://` submissions will fail with a CORS error, which is expected)

Fields submitted: email, country, sector, employees, turnover, scope verdict, readiness score, posture by measure, country law status, consent flag, timestamp.

---

## Security notes

- All user input is escaped via `escapeHtml()` before DOM insertion — no raw user data is injected as HTML
- No `eval()` or `innerHTML` with user-controlled content
- All interactive elements wired via `addEventListener` after injection, never inline `onclick`
- External links opened with `noopener,noreferrer`
- No localStorage, sessionStorage, or cookies

---

## Related

- [NIS2 SME Toolkit](https://github.com/paolocarner/nis2-sme-toolkit) — free templates, checklists, and guidance for organisations starting their NIS2 compliance journey
- [BARE Consult](https://www.bare-consult.com) — NIS2 and DORA compliance advisory

---

## Disclaimer

This tool is a self-reported screening aid only. It is not legal advice and does not constitute a certified compliance determination. NIS2 transposition status is indicative and may have changed since the data was last updated. Always engage qualified legal and compliance professionals before acting on these results.

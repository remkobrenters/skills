---
name: company-intelligence
description: European company intelligence and due diligence report. Trigger this skill when the user wants to research a company, vet a prospect or supplier, do due diligence on a potential partner, asks 'who is this company', 'check this business', 'look up this client', 'research this company', 'vet this supplier', or pastes a company name with a request to investigate. Also trigger for 'can you find info about X', 'what do we know about company Y', 'background check on this business', or any request to assemble a structured profile of a European company. Works for Netherlands, Belgium, Germany, UK, France, Spain, Italy, Portugal, Switzerland, Austria, Luxembourg, Denmark, Sweden, Norway, Finland, Ireland, and pan-European companies.
---

# Company Intelligence Skill

Builds a structured, layered intelligence profile of a European company from public sources. Output is a four-layer HTML report that separates verified facts, external estimates, derived signals, and analyst interpretation — each clearly labelled so any reader can calibrate their own trust level.

---

## Four-layer information architecture

Every data point must be assigned to exactly one layer. Never mix layers within a single statement.

| Layer | Label | Meaning |
|-------|-------|---------|
| L1 | `VERIFIED` | Directly from an official register, primary source filing, or the company's own legal documentation. Traceable. |
| L2 | `ESTIMATE` | From third-party aggregators (ZoomInfo, RocketReach, Tracxn, etc.) or range inferences from public signals. Not independently verified. Always state source + year. |
| L3 | `SIGNAL` | Derived from combining multiple sources or applying reasoning. Always state the basis: "Derived from: {source} × {logic}". |
| L4 | `ANALYST VIEW` | Interpretive block, explicitly marked. Uses hedged language only. Reader draws own conclusions. |

**Layer rules (enforce strictly):**
- L1 facts never contain editorial language ("strong", "credible", "stable", "rare", "genuine moat")
- L2 estimates always state the source name and year
- L3 signals always state: "Derived from: [X] × [logic]"
- L4 blocks use only: "suggests", "may indicate", "limited evidence of", "consistent with", "no public evidence of"
- The Overall Assessment section is always L4 — never presented as conclusion

**Banned phrases in L1/L2/L3:**
credible · stable · strong · solid · healthy · well-established · exceptional · genuine moat · notable · impressive · obvious · good sign · positive signal · clean · rare for this size

**Replace editorial conclusions with context flags:**
- ~~"no legal issues"~~ → "No cases found in {sources searched} on {date}"
- ~~"stable business"~~ → "Operating since {year}. No insolvency filings found in {source}."
- ~~"not a direct competitor"~~ → "Primary focus: {X}. Limited public evidence of {Y} capability."
- ~~"credible track record"~~ → "Client references visible: {list}"

---

## Workflow

### Step 1: Parse input

Extract from the user's message:
- **Company name** (required)
- **Country** (optional — infer from name or context if not provided)
- **Registration number** (optional)
- **Website or LinkedIn URL** (optional)
- **Additional context**

If country is ambiguous, default to NL for Webparking's typical prospect base and note the assumption.

---

### Step 2: Run research phase

Run searches **in parallel**. Gather before synthesising.

#### 2a. Registration and legal identity

**🇳🇱 Netherlands — KvK**
- Fetch: `https://www.kvk.nl/zoeken/?q={company+name}`
- Provides: CoC number, address, legal form, SBI codes, founding date
- Financial filings: `https://www.kvk.nl/jaarrekening/`

**🇧🇪 Belgium — KBO**
- Fetch: `https://kbopub.economie.fgov.be/kbopub/zoekwoordenform.html?lang=nl&naam={company+name}`
- Provides: registration number (BE 0xxx), VAT, legal form, NACE codes, address, status
- Revenue band and employee count sometimes in KBO filings

**🇩🇪 Germany — Handelsregister + Unternehmensregister**
- Fetch: `https://www.unternehmensregister.de/ureg/`
- Also: `https://www.northdata.de/`
- Provides: HRB/HRA number, legal form, directors, Jahresabschluss for GmbH/AG above threshold

**🇬🇧 UK — Companies House**
- Fetch: `https://find-and-update.company-information.service.gov.uk/search?q={company+name}`
- API (free): `https://api.company-information.service.gov.uk/search/companies?q={company+name}`
- Best financial data in Europe: full P&L in annual filings

**🇫🇷 France — SIRENE + Pappers**
- Fetch: `https://annuaire-entreprises.data.gouv.fr/rechercher?terme={company+name}`
- Fetch: `https://www.pappers.fr/recherche?q={company+name}` (revenue often available)

**🇪🇸 Spain — Registro Mercantil + BORME**
- Fetch: `https://www.boe.es/diario_borme/` and `https://einforma.com/`

**🇮🇹 Italy — Registro delle Imprese**
- Fetch: `https://www.atoka.io/` (free basic tier)

**🇵🇹 Portugal — Racius**
- Fetch: `https://www.racius.com/empresas/?q={company+name}`

**🇨🇭 Switzerland — Zefix**
- Fetch: `https://www.zefix.ch/en/search/entity/list/firm?name={company+name}`

**🇦🇹 Austria — WKO**
- Fetch: `https://firmen.wko.at/suche.aspx?suchbegriff={company+name}`

**🇱🇺 Luxembourg — LBR**
- Fetch: `https://www.lbr.lu/mjrcs/jsp/search.jsp`

**🇩🇰 Denmark — CVR**
- Fetch: `https://cvr.dk/` — includes financials for most companies (L1 quality)

**🇸🇪 Sweden — Allabolag**
- Fetch: `https://www.allabolag.se/what/{company+name}` — full income statements (L1 quality)

**🇳🇴 Norway — Brreg + Proff**
- Fetch: `https://www.brreg.no/finn-foretak/?q={company+name}` and `https://www.proff.no/`

**🇫🇮 Finland — YTJ + Finder**
- Fetch: `https://ytj.fi/` and `https://www.finder.fi/`

**🇮🇪 Ireland — CRO**
- Fetch: `https://www.cro.ie/` and `https://www.vision-net.ie/`

**🌍 OpenCorporates (universal fallback)**
- Fetch: `https://opencorporates.com/companies?q={company+name}&jurisdiction_code={cc}`

---

#### 2b. Financial signals

Assign every figure to a layer:
- **L1**: figures directly from official filings (KBO, Companies House, CVR, allabolag.se, pappers.fr, etc.) — state filing year
- **L2**: third-party estimates — state source name + year (ZoomInfo, RocketReach, Tracxn, Growjo, etc.)
- **L3**: derived estimates — state the formula explicitly ("Derived from: {X} FTE × {sector avg revenue-per-FTE} = ~{Y}")

Country financial data quality:
- 🇬🇧 UK / 🇩🇰 DK / 🇸🇪 SE / 🇳🇴 NO: full financials publicly accessible → L1 possible
- 🇧🇪 BE: revenue band in KBO filings → L1 (band), pappers/companyweb → L2
- 🇫🇷 FR: pappers.fr revenue → L2 (aggregated)
- 🇳🇱 NL: abbreviated filings common for SMEs → revenue usually L2 or L3
- 🇩🇪 DE / 🇨🇭 CH / 🇦🇹 AT / 🇪🇸 ES / 🇵🇹 PT: limited public financial data → mostly L2/L3

---

#### 2c. Market position and products

- Fetch company website homepage and /about page
- Search: `"{company name}" products OR services OR sector`
- Extract product names, target segments, geographic reach, pricing model if visible
- Label all positioning language L3 — it is your interpretation of their self-description
- Do not call a market position "strong" or "clear" — describe what is visible

---

#### 2d. Competitive landscape

- Search: `"{company name}" competitors OR alternatives`
- Search: `{product category} {country} top companies`
- Label each competitor: **Direct** (same product/segment) or **Adjacent** (related space, different core)
- Always add a source note explaining how competitors were identified
- Never present competitor identification as L1

---

#### 2e. Online presence

- Search LinkedIn company page: follower count, headcount range, specialties, last post date
- Check website, event participation, tech stack signals from job ads or Wappalyzer
- LinkedIn employee count and follower count are L2 (platform estimates, not payroll data)

---

#### 2f. News and press

- Search: `"{company name}" news` (past 12 months)
- Search: `"{company name}" acquisition OR funding OR insolvency OR lawsuit`
- Each item: state source + date
- Separate confirmed announcements (L1) from press speculation or single-source claims (L2)

---

#### 2g. Legal and regulatory

**🇳🇱**: `"{company name}" site:uitspraken.rechtspraak.nl`
**🇧🇪**: `"{company name}" site:juridat.be` + `"{company name}" rechtbank`
**🇩🇪**: site:insolvenzbekanntmachungen.de + `"{company name}" insolvenz`
**🇬🇧**: `"{company name}" court judgment` + bailii.org
**🇫🇷**: `"{company name}" tribunal` + bodacc.fr
**🇩🇰/🇸🇪/🇳🇴/🇫🇮**: `"{company name}" konkurs OR domstol`
**🌍 GDPR**: search enforcementtracker.com
**🌍 General**: `"{company name}" fine OR boete OR regulatory penalty`

Reporting rule: state exactly what was searched and what was returned. Do not use "no issues found" framing. Write: "Search of {source} on {date}: no results returned for this entity."

---

#### 2h. Logo / brand image

- image_search: `"{company name}" logo`

---

#### 2i. Trust signals

- Search: `"{company name}" ISO 27001 OR certified OR keurmerk OR award`
- Each item: L1 if certification body or official register confirms it; L2 if claimed on company website only (unverified)

---

### Step 3: Assign layers before writing

Before generating HTML, audit every data point:
- Does it contain an adjective that describes quality? → L4 only
- Is a single source the only basis? → L2 or L3, not L1
- Did you combine sources using your own logic? → L3, state the basis
- Is it a negative ("not found")? → State search performed, not a trust signal

---

### Step 4: Generate the HTML report

Read `references/report-template.md` BEFORE writing any HTML. Follow it exactly.

Key rules:
- Every data row must show its layer badge (L1/L2/L3)
- Context flags in the main body — no editorial conclusions
- L4 Analyst View blocks are visually distinct and clearly labelled
- Financial section: one row per figure, source + year + layer
- Legal section: one row per source searched + result returned
- Remove any row where content is "not found" for a generic filler search
- Competitors section: always include source note explaining how they were identified

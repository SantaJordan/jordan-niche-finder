# PROMPT — Higher-Ed Vendor Relationship Census (Jared / CTOx)

> Paste this whole prompt into a fresh Claude Code session on one of Jordan's Macs.
> It expects the global `crawford` skill (Blueprint-GTM-Skills) to be available —
> invoke it before doing anything else. If Crawford is not available, stop and say so.

---

## Mission

Build a ranked census of the companies that have the MOST relationships (contracts,
repeat engagements, cooperative-purchasing agreements) with US higher-ed institutions —
restricted to vendors that are **non-technical but technology-adjacent**.

This is prospecting for **Jared, a fractional CTO in the CTOx program**. His ideal
prospect: a company that already owns deep relationships and domain expertise across
many colleges and universities, does NOT have software as its core offering, but sells
something (curriculum, UX/user-experience work, assessment content, enrollment
consulting, student-success services…) that could plausibly become its own technology
product. Jared's pitch writes itself from the data: *"You already have relationships
with N institutions. Your category is productizing. I help firms like yours ship their
first product without hiring a full-time CTO."*

**The message is a redescription of the targeting** — so every vendor kept in the final
list must carry, as typed evidence: (1) how many distinct institutions it demonstrably
works with, (2) proof it is not a technology company, and (3) a one-line statement of
what its productizable asset is.

## Deliverable

A ranked vendor list as CSV + a Google Sheet (use the `gws` CLI; auth is already set
up), one row per vendor:

| field | type |
|---|---|
| vendor_name | string |
| domain | string (resolved, never guessed — use the domain-linkedin-finder skill / Crawford Job 2 if needed) |
| category | enum: curriculum_content, ux_design, enrollment_marketing, assessment_testing, student_success_consulting, compliance_training_content, career_services, advancement_fundraising_services, other_services (extend if the data demands it) |
| tech_adjacency | enum: sweet_spot, services_pure, already_tech_EXCLUDE, ambiguous |
| tech_adjacency_rationale | one sentence citing the vendor's own site |
| institutions_contract_evidence | int — distinct institutions with Tier-A evidence |
| institutions_presence_evidence | int — distinct institutions with only Tier-B evidence |
| evidence_samples | 3–5 citations: url + quote + as_of date each |
| productizable_asset | one sentence: what they'd turn into software |
| decision_maker_name / title / linkedin_url | via enrichment APIs only (Blitz/FullEnrich — FullEnrich needs Jordan's explicit OK in-session) |
| why_now | one sentence tying THEIR evidence to Jared's offer |
| confidence | 0–1 |
| as_of | date of freshest supporting evidence |

Rank by `institutions_contract_evidence` (primary), `institutions_presence_evidence`
(tie-break). Target: top 50 fully enriched; keep the longer tail (all classified
vendors) in a second tab, unenriched.

## Ground rules (Crawford doctrine — non-negotiable)

1. **Invoke the `crawford` skill first** and follow its session contract: preflight
   before any spend, catalog first (`catalog.py search` / `dead`), free-first
   escalation ladder, cost ledger on, quota ledger for free tiers.
2. **Never scrape what's already scraped; never pay for what's free.** Registries and
   bulk files are the backbone; the live web is for the residue.
3. **Evidence contract:** a "works with institution X" claim needs a citation with a
   URL, a load-bearing quote, and an honest `as_of` date. Vendor self-reported client
   logos are Tier-B only and never corroborate alone. Aggregators never corroborate.
4. **Typed cells are the product.** Every row schema-valid; unknowns are explicit
   nulls, never guesses. Never guess `{name}.com` for a domain.
5. **House rules:** no LinkedIn browser automation ever (enrichment APIs taking
   LinkedIn URLs as data are fine). No MCP tools for batches >50 — write API scripts.
   Any LLM job ≥$100 estimated → cost guard first. Final dataset ≥1,000 rows →
   `/tam-qa` before ship.
6. **Estimate before you spend.** State expected rows × rung mix and a dollar estimate
   up front; ask Jordan before any paid rung beyond trivial cost.

## Clarify gate — ask Jordan (or Jared) these BEFORE building

1. **Education level:** all Title-IV postsecondary (~6,000 incl. 2-year and
   trade/vocational), or degree-granting 4-year only (~2,700)? Default if no answer:
   all degree-granting 2-year + 4-year.
2. **Vendor size ceiling:** exclude giants (Pearson, Sodexo, Aramark, EAB, Huron)
   that would never hire a fractional CTO? Default: keep them in the census for
   completeness but flag `too_big_for_offer = true` above ~1,000 employees; the
   ranked outreach list is the sub-1,000 slice.
3. **"Already-tech" line:** a curriculum company that sells digital courseware — in
   or out? Default rubric below; confirm it.

## Step 1 — The universe (denominator): NCES IPEDS

Pull the full institution universe from **NCES IPEDS** (in Crawford's catalog:
UNITID, name, address, website, sector/control public/private-nonprofit/for-profit,
enrollment). The Urban Institute Education Data Portal API is the no-download way to
query it. This is the join spine: every relationship claim maps to a UNITID.
Split matters downstream:
- **Private nonprofits** (~1,600) → IRS 990 lane.
- **Publics** (~1,900) → state transparency/checkbook + procurement lane.

## Step 2 — Relationship evidence (numerator), cheapest lane first

### Tier A — contract/payment evidence (counts toward the ranking)

**Lane 1 — IRS Form 990, Part VII Section B (private nonprofits). Do this first; it
is the silver bullet.** Every private nonprofit college's 990 lists its **five
highest-compensated independent contractors** (>$100k), by name, with compensation
and service description. Bulk e-file XML is free (AWS Open Data / ProPublica
Nonprofit Explorer API — both in Crawford's catalog). Match college EINs from the IRS
BMF against IPEDS names, extract every contractor row for the latest 2–3 filing
years, normalize vendor names, count distinct institutions per vendor. One free bulk
source yields a defensible nationwide contractor×institution matrix. (Bias to
disclose honestly: it captures only each school's top-5 contractors — construction
and food service will dominate raw counts; the tech-adjacency filter in Step 3 is
what makes the output useful.)

**Lane 2 — public-university vendor payments.** Query Crawford's Tier-2 gov index
(`gov_index.py search`) for state checkbook / vendor-payment / purchase-order / PO
datasets covering public universities (e.g. "vendor payments", "university
purchase orders", state transparency portals). Harvest the states where a bulk file
exists; log a coverage ledger of states attempted/covered/absent — absence of a
state is absence of evidence, never proof.

**Lane 3 — cooperative purchasing contract catalogs.** E&I Cooperative Services
(higher-ed-specific; public supplier/contract catalog), NASPO ValuePoint, Sourcewell,
OMNIA Partners. A vendor holding an E&I contract has a standing purchasing
relationship with hundreds of member institutions — record it as
`coop_contract = true` with the co-op named; it counts as Tier-A breadth but keep the
count separate from per-institution rows.

**Lane 4 (residue only) — university bid-award pages.** Individual procurement/bid
award postings, only for vendors already surfaced that need corroboration.

### Tier B — presence evidence (tie-break only, never primary)

Higher-ed association conference **exhibitor/sponsor lists**: NACUBO, AACRAO, NACAC,
NASPA, ACUHO-I, UPCEA, CASE, ACE. (Skip EDUCAUSE — its exhibitors are tech companies
by definition; useful only as an EXCLUSION signal: an EDUCAUSE exhibitor is probably
`already_tech`.) Vendor-site client lists/case studies count here too, clearly
flagged self-reported.

## Step 3 — The filter: technology-adjacent, non-technical

Classify every vendor that clears a breadth threshold (≥3 institutions Tier-A, or a
co-op contract). Rubric:

- **already_tech_EXCLUDE** — software/SaaS/IT-services is the core offering: LMS, SIS,
  ERP, IT consulting, managed services, dev shops, edtech platforms. Signals: pricing
  page for software, "platform/login" as primary CTA, EDUCAUSE exhibitor, engineering
  jobs dominating careers page.
- **sweet_spot** (the target) — core offering is content, expertise, or human-delivered
  service with an obvious software-shaped upgrade, AND no serious in-house engineering.
  Examples of the shape: curriculum/course-material developers, UX and user-experience
  research firms serving .edu, enrollment-marketing agencies, assessment/test-prep
  content shops, student-success and retention consultancies, compliance-training
  content providers, career-services providers, advancement/fundraising consultancies.
- **services_pure** — real relationships but no plausible product path (construction,
  food service, janitorial, insurance brokerage, audit). Keep in census, out of
  outreach list.
- **ambiguous** — escalate.

Run classification the Crawford delegation way: build a small **evidence pack** per
vendor (homepage + about + services + careers excerpts), dispatch **blind no-tools
adjudicators** (one vendor per invocation) against the rubric, then a separate
compare pass on disagreements. Never classify from the vendor's name alone.

## Step 4 — Enrich the outreach slice (top ~50 sweet_spot vendors)

Founder/CEO/President via Blitz (primary) — FullEnrich for email/mobile only with
Jordan's explicit OK. Write the `why_now` line from each vendor's own evidence
(their institution count, their category's productization pressure). Short — the
data is the message.

## Step 5 — QA + ship

- ≥1,000 rows → `/tam-qa`.
- Sanity checks: top-10 raw contractor list WILL be construction/food-service —
  verify the filter removed them; spot-check 10 sweet_spot classifications by hand;
  verify institution counts against 3 known vendors.
- Print the coverage ledger (990 years covered, states with/without checkbook data,
  co-ops harvested) and the cost ledger report.
- Ship: CSV in the run dir + Google Sheet via `gws`, share link in the final message.

## Budget posture

Steps 1–3 should be ~$0 (bulk files, free APIs, subscription tokens). The only paid
spend should be Step 4 enrichment (~50 contacts — Clay credits first, they're sunk)
and possibly small search residue. If any step projects >$25, stop and ask.

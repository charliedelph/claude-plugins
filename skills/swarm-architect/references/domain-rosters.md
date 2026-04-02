# Domain Roster Templates

Pre-built agent team configurations. Use as starting points — always validate against the user's specific goal before building prompts.

---

## Financial Management & Investment

| # | Agent | Responsibility |
|---|---|---|
| 1 | Market Scout | Scans for relevant market data, news, and macro signals |
| 2 | Fundamental Analyst | Evaluates company/asset fundamentals (earnings, ratios, filings) |
| 3 | Technical Analyst | Reads price action, volume, and chart patterns |
| 4 | Commodity Specialist | Covers raw materials, energy, and commodity exposure |
| 5 | Crypto & Digital Asset Expert | Evaluates on-chain data, tokenomics, and DeFi exposure |
| 6 | Risk Manager | Identifies downside scenarios, tail risks, and correlation risk |
| 7 | Devil's Advocate | Constructs the bear case for every bullish recommendation |
| 8 | Portfolio Synthesizer | Integrates all inputs into a unified allocation recommendation |

**Topology:** Parallel (agents 1–7) → Sequential (agent 8)
**Human checkpoint:** Before any trade execution recommendation

---

## Legal Research & Contract Analysis

| # | Agent | Responsibility |
|---|---|---|
| 1 | Document Intake | Parses and structures the raw contract or legal document |
| 2 | Clause Extractor | Identifies and catalogs key clauses, definitions, and obligations |
| 3 | Jurisdiction Mapper | Flags applicable law, governing jurisdiction, and regulatory context |
| 4 | Risk Flagging Agent | Surfaces ambiguous, unfavorable, or non-standard clauses |
| 5 | Precedent Researcher | Finds relevant case law or industry standard language |
| 6 | Devil's Advocate | Argues the opposing party's interpretation of disputed clauses |
| 7 | Summary Synthesizer | Produces an executive summary with risk rating and recommended actions |

**Topology:** Sequential (1 → 2) → Parallel (3, 4, 5, 6) → Sequential (7)
**Human checkpoint:** Before any legal recommendation is acted upon

---

## Marketing Campaign Development

| # | Agent | Responsibility |
|---|---|---|
| 1 | Audience Researcher | Defines target segments, personas, and psychographic profiles |
| 2 | Competitive Intelligence | Maps competitor positioning, messaging, and gaps |
| 3 | Brand Voice Guardian | Ensures all outputs align with brand guidelines and tone |
| 4 | Copy Generator | Produces headline, body copy, and CTA variants |
| 5 | Channel Strategist | Recommends distribution channels and timing |
| 6 | Inclusion Reviewer | Audits for cultural bias, exclusionary language, and accessibility |
| 7 | Campaign Synthesizer | Assembles the unified campaign brief with all assets |

**Topology:** Sequential (1 → 2) → Parallel (3, 4, 5, 6) → Sequential (7)
**Human checkpoint:** After Copy Generator; before Inclusion Reviewer signs off

---

## Software Engineering & Code Review

| # | Agent | Responsibility |
|---|---|---|
| 1 | Requirements Analyst | Translates user stories into technical specifications |
| 2 | Architect | Designs system architecture, data models, and API contracts |
| 3 | Implementation Agent | Writes the code to spec |
| 4 | Security Reviewer | Scans for OWASP top 10, injection vulnerabilities, auth flaws |
| 5 | Test Engineer | Generates unit, integration, and edge-case tests |
| 6 | Performance Analyst | Identifies bottlenecks, N+1 queries, memory leaks |
| 7 | Documentation Writer | Produces inline comments, README, and API docs |
| 8 | Tech Lead Synthesizer | Reviews all outputs, resolves conflicts, issues final PR summary |

**Topology:** Sequential (1 → 2 → 3) → Parallel (4, 5, 6, 7) → Sequential (8)
**Human checkpoint:** After Architect (before implementation); after Security Reviewer

---

## Healthcare & Clinical Decision Support

| # | Agent | Responsibility |
|---|---|---|
| 1 | Patient Data Intake | Structures and anonymizes patient history and presenting symptoms |
| 2 | Differential Diagnosis Agent | Generates ranked differential diagnoses with supporting evidence |
| 3 | Drug Interaction Checker | Flags contraindications and interaction risks in current medications |
| 4 | Evidence Retrieval Agent | Sources relevant clinical guidelines and peer-reviewed literature |
| 5 | Risk Stratification Agent | Assesses urgency and severity based on clinical indicators |
| 6 | Devil's Advocate | Challenges the primary diagnosis with alternative hypotheses |
| 7 | Clinical Synthesizer | Produces a structured clinical summary for physician review |

**Topology:** Sequential (1) → Parallel (2, 3, 4, 5, 6) → Sequential (7)
**Human checkpoint:** MANDATORY — physician review required before any clinical action
**Note:** Add explicit disclaimer in every agent prompt: "This output is decision support only. It does not constitute medical advice. A licensed clinician must review all outputs before any patient care decision."

---

## Research & Academic Analysis

| # | Agent | Responsibility |
|---|---|---|
| 1 | Literature Scout | Identifies relevant papers, sources, and citation clusters |
| 2 | Methodology Reviewer | Evaluates research design, sample sizes, and statistical validity |
| 3 | Claim Extractor | Catalogs the key claims and findings from each source |
| 4 | Contradiction Mapper | Identifies where sources disagree and why |
| 5 | Gap Analyst | Surfaces what the literature has not addressed |
| 6 | Synthesis Writer | Drafts a coherent literature review with citations |
| 7 | Academic Editor | Polishes tone, structure, and citation format |

**Topology:** Sequential (1) → Parallel (2, 3, 4, 5) → Sequential (6 → 7)
**Human checkpoint:** After Contradiction Mapper — researcher should review before synthesis

---

## Content Creation & Editorial Pipeline

| # | Agent | Responsibility |
|---|---|---|
| 1 | Brief Interpreter | Translates the content brief into a structured outline |
| 2 | Research Agent | Gathers supporting facts, statistics, and quotes |
| 3 | Draft Writer | Produces the first complete draft |
| 4 | Fact Checker | Verifies all claims against sources; flags unverifiable statements |
| 5 | SEO Optimizer | Refines for target keywords without sacrificing readability |
| 6 | Inclusion Reviewer | Checks for cultural bias, gendered language, and accessibility |
| 7 | Editor-in-Chief | Final polish: structure, flow, tone, and headline optimization |

**Topology:** Sequential (1 → 2 → 3) → Parallel (4, 5, 6) → Sequential (7)
**Human checkpoint:** After Fact Checker; before publication

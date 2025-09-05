# Operating Playbook: How Strategy, Content, Design, Dev & Lead Gen Work Together

**Purpose:** Define the end‑to‑end operating model for the new headless platform (Next.js + Contentful + Marketo + Smartling), with Figma as the **primary** design tool and **code as the single source of truth** (tokens + components + MDX docs).

**Guiding principles**
- Outcomes over output; ship small and often.  
- Code is canonical for tokens/components; Figma reflects the latest published versions.  
- Edge‑first performance; accessibility and SEO are non‑negotiable.  
- Localized by design; experiments are safe and reversible.  
- Data privacy by default (consent, server‑side tagging, PII minimization).

---
## Roles & Team Responsibilities
**Strategy (Web Strategy + PMM + Program Ops)**  
Owns the roadmap, success metrics, prioritization, and portfolio trade‑offs. Facilitates intake and sets experiment hypotheses.

**Content (Stakeholders + SEO + Localization PM)**  
Owns content model fidelity, briefs, drafting, editing, taxonomy, metadata, and localization readiness.

**Design (Brand/UX/Visual/Content Design)**  
Owns Figma exploration, tokens, component specs, accessibility criteria, and design QA. Publishes guidance in Storybook MDX.

**Development (FE/BE/Platform)**  
Owns the codebase, CI/CD, templates, component library, performance/SEO guardrails, DX tooling, and technical QA.

**Lead Generation (Demand Gen + MOPs + SDR Ops)**  
Owns conversion strategy, campaign LPs, Marketo integration, attribution, scoring/routing, and post‑launch optimization.

---
## End‑to‑End Lifecycle (Swimlanes)
```
Intake → Discover/Shape → Design → Build → Localize → QA → Launch → Measure → Iterate
```
**Swimlane highlights**
- **Intake (Strategy)**: triage requests; validate against goals; create brief; attach hypothesized impact.  
- **Discover/Shape (Strategy + Content + Design + Dev)**: align on scope; choose template/module reuse; define Definition of Ready (DoR).  
- **Design (Design)**: Figma exploration; tokens updated; handoff via Storybook references; file tagged with tokens version.  
- **Build (Dev)**: implement/compose modules; PR with preview URL; tests + budgets enforced.  
- **Localize (Content + Smartling)**: send for translation; ensure hreflang/meta; market reviewers sign‑off.  
- **QA (Design + Dev + SEO)**: visual, a11y, perf, SEO, analytics and form checks.  
- **Launch (Strategy + Lead Gen + Dev)**: go/no‑go; feature flags/experiments; staged rollout.  
- **Measure (Lead Gen + Strategy)**: readouts on KPIs; decide iterate/graduate/retire.  
- **Iterate (All)**: backlog updates; document ADRs and learnings.

---
## Definitions of Ready & Done (per stage)
**Definition of Ready (DoR)**  
- Problem statement & target metrics defined  
- Audience/persona & JTBD clarified  
- Content brief or experiment BRD attached  
- Template/module inventory decided (reuse before build)  
- Analytics events & SEO requirements listed  
- Localization markets + rollout plan identified  

**Definition of Done (DoD)**  
- Stories merged with green checks (unit/E2E/visual)  
- Lighthouse CI & Web Vitals budgets pass  
- A11y checks (axe) pass; keyboard flows verified  
- Schema/JSON‑LD present; sitemap & hreflang updated  
- Analytics validated (dev → preview → prod)  
- Docs/MDX updated; changelog and ADR logged  
- Experiment guardrails configured (if applicable)

---
## Core Workflows
### 1) Net‑New Marketing Page (Hero + Modules + Form)
1. **Intake** (Strategy): brief + success metric; select template; prioritize slot.  
2. **Content** drafts in Contentful (modules), adds SEO meta & internal links.  
3. **Design** assembles in Figma using existing modules; proposes token tweaks if needed.  
4. **Dev** composes page with existing modules; creates PR preview; connects Marketo form via server proxy.  
5. **QA**: visual (Chromatic), a11y, perf, SEO checks; analytics events verified in Server GTM.  
6. **Localize** via Smartling; market reviewers approve.  
7. **Launch** behind flag; monitor; iterate.

### 2) Experiment / A/B Test
1. **Hypothesis** (Strategy + Lead Gen): define KPI, minimal detectable effect, sample/segment, and duration.  
2. **Design** variants in Figma using existing modules; no net‑new components unless approved.  
3. **Dev** configures experiment at edge (Vercel/GrowthBook); exposes toggles in config.  
4. **QA** assures no SEO cloaking; consistent analytics; performance parity.  
5. **Run**; **Analyze**; **Decide** (graduate, iterate, or roll back).  
6. **Document** learnings in MDX playbook.

### 3) New Component / Design‑System Change
1. **Request** via design‑system intake (use‑cases, constraints, accessibility needs).  
2. **Design** explores in Figma; updates tokens; links to specs.  
3. **Dev** implements in `design‑system`; adds stories, MDX, tests; Chromatic review.  
4. **Release** `@org/design-tokens` and/or `@org/design-system` (semver); apps bump via Renovate.  
5. **Deprecate** old patterns with migration notes and sunset date.

### 4) Content Optimization / SEO Fix
1. **Content** proposes changes (title/meta/internal links/schema).  
2. **SEO** review; **Dev** only if template change needed.  
3. **Preview** via Contentful Preview API on PR URL; **Publish**; monitor Search Console & CWV.

### 5) Localization Rollout
1. **Scope** markets & content types; verify translatability of modules.  
2. **Send** entries to Smartling; maintain glossary & TM.  
3. **Review** by local owners; **QA** locale-specific assets; **Publish** with hreflang & canonical logic.  
4. **Measure** local CWV, CTR, CVR; backlog follow‑ups.

### 6) Campaign Landing Page (Lead Gen)
1. **Lead Gen** shares campaign brief: offer, audience, UTMs, routing rules.  
2. **Content** selects Landing Page type; **Design** applies variants;  
3. **Dev** connects Marketo form (server proxy, bot defense); event taxonomy set.  
4. **QA**: conversion checks (form, UTMs, attribution), SEO no‑index rules if needed.  
5. **Launch**; **MOPs** verifies scoring/routing; **SDR Ops** confirms alerting; **Analyze** & iterate.

### 7) Incident/Hotfix
- **Pager order:** Dev (on‑call) → Strategy → Design → Content → Lead Gen.  
- Rollback via feature flag; fix‑forward with PR; post‑mortem within 48h; action items logged.

---
## Governance & Quality Gates
- **Performance:** p75 LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1; budgets enforced in CI.  
- **Accessibility:** WCAG 2.2 AA; axe & keyboard flows in E2E; color contrast via token rules.  
- **SEO:** canonical, hreflang, schema coverage; link hygiene CI; sitemaps automated.  
- **Security/Privacy:** CSP/COOP/CORP headers; server‑side tagging; consent mode v2; PII scrubbing.  
- **Observability:** Sentry, OpenTelemetry → Datadog; RUM CWV; form error alerts to MOPs.

---
## Cadence, SLAs, and Rituals
**Cadence**  
- Daily: Dev stand‑up (15m).  
- 2×/week: Cross‑team triage (30m).  
- Weekly: Release planning & quality gate review (45m).  
- Bi‑weekly: Experiment review; design‑system council (45m each).  
- Monthly: Metrics deep‑dive; roadmap reset (60–90m).  

**SLAs (business days)**  
- Intake triage ≤ **2 BD**  
- Design review on PR previews ≤ **2 BD**  
- Dev code review turnaround ≤ **1 BD**  
- Translation cycle (per locale) ≤ **3–5 BD**  
- Post‑launch experiment readout ≤ **5 BD**  

**Rituals**  
- **Design‑system council:** approves new components/tokens; publishes changelog.  
- **WebOps council:** performance/SEO governance; incident reviews.  
- **Content guild:** taxonomy, tone, and internal linking practices.

---
## Artifacts & Tooling Hand‑offs
- **Briefs & BRDs:** stored with issue; include metrics, audience, constraints.  
- **Figma files:** linked to Storybook stories; note `@org/design-tokens` version.  
- **Storybook MDX:** single source for best practices, usage, a11y notes, examples.  
- **Contentful entries:** follow content model & validation; preview links in PRs.  
- **Experiment configs:** versioned in repo; flags documented with owners.  
- **Marketo forms:** defined in CMS module; submitted via server proxy; QA in preview.  
- **Analytics:** event map in repo; Server GTM config reviewed in PRs.

---
## RACI Summary
| Work Item | Strategy | Content | Design | Dev | Lead Gen |
|---|---|---|---|---|---|
| Net‑new page | **A/R** | R | R | R | C |
| Experiment | **A/R** | C | R | R | R |
| Component change | C | C | **A/R** | R | C |
| SEO remediation | A | R | C | R | C |
| Localization rollout | A | R | C | R | C |
| Campaign LP | A | R | R | R | **A/R** |
*A = Approver, R = Responsible, C = Consulted*

---
## Communication & Decision Logs
- **Change log:** design tokens/components; content model; templates.  
- **ADRs:** architecture & design decisions with context and consequences.  
- **Runbooks:** incidents (rollback, cache purge, form outage, translation fail).  
- **Channels:** #web‑triage, #design‑system, #experiments, #seo‑perf.

---
## Templates & Checklists (Appendix)
**Intake template (short)**  
- Goal/KPI:  
- Audience/persona:  
- Primary action:  
- Markets/locales:  
- Deadline/constraints:  
- Measurement plan:  

**Experiment BRD (short)**  
- Hypothesis (change → impact on KPI)  
- Primary KPI & guardrails (e.g., bounce, INP)  
- Segment, traffic split, duration  
- Success threshold & decision rule  

**Localization checklist**  
- [ ] Strings translatable  
- [ ] Assets have locale variants  
- [ ] Hreflang/canonical verified  
- [ ] Market review complete  

**QA checklist**  
- [ ] Axe a11y pass  
- [ ] Lighthouse ≥ 90 (Perf/SEO/A11y/Best)  
- [ ] Analytics fires in preview/prod  
- [ ] Forms submit + route  
- [ ] Web Vitals p75 within budget  

— End of playbook —

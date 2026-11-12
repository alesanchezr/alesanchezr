# Caxton: product decisions and outcomes

[Overview](README.md) · [Technical](TECHNICAL.md) · **Product**

> I turned a marketing website into an AI-operated content operation: a governed team of specialized AI agents now authors every content change on 4geeks.com, applies a median edit 3 minutes after filing, and updated 55% of the blog in 10 weeks, on infrastructure that costs $25/month.

## Metrics at a glance

| Metric | Value |
|---|---|
| Content changes authored by AI agents | 100% of 1,434 closed proposals |
| Changes applied | 1,022 in 8 weeks, ~124 per week |
| Median time from proposal to live (edits) | 3 minutes (92% within 24 hours) |
| Blog posts updated | 180 of 326 (55%) in 10 weeks |
| Organic search clicks | +135% from July to October 2026 (still 31% below October 2025) |
| Platform cost | $25/month |

Sources and metric types are listed under [Results](#results).

---

## 1. Problem and opportunity

4Geeks Academy competes for organic search and AI-answer visibility in two languages against bootcamps, universities and AI-native learning products. Search demand shifts weekly as new AI tools launch, so content goes stale quickly, and refreshing hundreds of pages by hand does not scale for a small team.

The site ran on a free Vercel setup with no way to host AI agents. Off-the-shelf platforms either locked content into a vendor (Wix, Contentful), required database and plugin operations (WordPress), or offered no editor at all (custom Next.js). None supported the operating model I wanted: **AI agents producing most of the work, with humans owning decisions and quality.**

## 2. Users and jobs-to-be-done

| User | Job to be done |
|---|---|
| Marketing and SEO team | "Keep hundreds of pages accurate, current and ranking in two languages without a large writing team." |
| Company leadership | "Grow organic leads without growing headcount or tooling spend." |
| AI agents (as first-class users) | "Find work, make a safe change, know exactly what happened, and hand off to the next role." |
| Engineers | "Add components and features without breaking content or the agents that edit it." |
| External adopters (open source) | "Own my site in Git, keep a visual editor, and let my agents run it." |

## 3. Competitive landscape and positioning

| Option | Strength | Gap for an AI-operated site |
|---|---|---|
| Wix, Squarespace | Fast setup | No custom logic, limited SEO control, no agent interface |
| WordPress | Plugin ecosystem | Database and security operations; agents edit through fragile APIs |
| Contentful, Sanity | Strong content APIs | Content in a vendor cloud; usage-based pricing; no governance for agent writes |
| Custom Next.js + MDX | Full control | No editor for non-engineers |
| **Caxton** | Content in your Git repo, visual editor, **governed MCP interface for agents** | Early-stage ecosystem |

**Positioning:** "The agentic CMS. Own your website in the AI era." The differentiator is not that agents can edit, but that agents edit **under governance**: least-privilege roles, separation of duties, draft-first changes and full auditability.

## 4. Product Decision Records

### PDR-01: Build an agent-first CMS instead of buying one

- **Context.** No available platform combined content ownership, a visual editor and a safe agent interface.
- **Decision.** Build Caxton, designed from day one for AI agents as primary operators.
- **Alternatives considered.** Stay on the free Vercel setup; adopt a headless CMS and bolt on agents.
- **Outcome.** 100% of content changes are now agent-authored, at a platform cost of $25/month.

### PDR-02: Agents propose, they do not publish

- **Context.** Unsupervised agents publishing to a revenue-generating site is an unacceptable brand and compliance risk.
- **Decision.** Every agent change is a proposal backed by a draft. Publishing requires an approval that satisfies separation-of-duties rules enforced in code.
- **Alternatives considered.** Agents with direct publish rights and post-hoc review.
- **Outcome.** 1,022 changes applied with a 4.8% rejection rate; 19% of applied edits went through at least one blocker-and-fix cycle before going live, which is where review caught issues before visitors saw them.

### PDR-03: A team of specialized agent roles instead of one general agent

- **Context.** A single agent with every permission is harder to steer, audit and trust.
- **Decision.** Define eight roles, each with its own tools and permissions: orchestrator, SEO specialist, copy editor, layout editor, translator, media editor, proposal reviewer and publisher.
- **Outcome.** Clear accountability per role. In the first 8 weeks, the SEO specialist contributed 420 applied proposals, the copy editor 168, the translator 84 and the orchestrator 21.

### PDR-04: Business rules live in code, not in prompts

- **Context.** Prompt instructions are ignored or forgotten under load, and every model behaves differently.
- **Decision.** Encode the marketing playbook as server-side rules:
  - every new page must declare its funnel stage and target product;
  - a target keyword can be owned by only one page (prevents keyword cannibalization);
  - outcome figures (hiring rate, salary, tuition) and third-party claims must match an approved evidence source.
- **Outcome.** The same rules apply regardless of model or vendor. Agents from three model families (Anthropic Claude, xAI Grok and DeepSeek) operate under identical governance.

### PDR-05: Vendor-neutral by design

- **Context.** The best model for each job changes every few months.
- **Decision.** Use the open Model Context Protocol, standard OAuth and per-session model identification instead of a single AI vendor's SDK.
- **Outcome.** Applied edits came from Claude (369), Grok/xAI (314) and DeepSeek (15) through Claude, Cursor and Grok clients, with no vendor lock-in.

### PDR-06: Multi-site from a single deployment

- **Context.** 4Geeks operates several web properties for different audiences.
- **Decision.** One deployment serves many domains, each with its own content repository, with optional component and theme inheritance.
- **Outcome.** 4geeks.com, fl.4geeksacademy.com and business.4geeks.com run on one platform and one $25/month server.

### PDR-07: A human feedback loop on agent outcomes

- **Context.** Approval checks correctness before publishing; it does not measure whether the change achieved its goal.
- **Decision.** Staff grade closed proposals as good or bad, describe what should have happened, and capture lessons that feed back into agent conventions.
- **Outcome.** 203 outcomes graded so far: 71% good. The 58 graded bad are the improvement backlog for agent behavior.

### PDR-08: Open-source the platform

- **Context.** Agent-operated websites are a new category, and credibility comes from transparency.
- **Decision.** Release Caxton under the MIT license and publish the MCP response standards it follows.
- **Outcome.** Public repository and public standards documentation. *(Adoption metrics to be tracked after public launch.)*

## 5. Roadmap and sequencing

I sequenced the platform so each stage de-risked the next:

| Phase | Period | Capability delivered | Why in this order |
|---|---|---|---|
| 1. Foundation | Nov 2025 to Jan 2026 | Git-backed content, schema-first components, validation framework | Content ownership and quality gates before any automation |
| 2. Scale | Feb to Apr 2026 | Cloud media, commit queue, multilingual SEO, AI chat layer | Production readiness for the full site |
| 3. Agent access | May 2026 | MCP server, OAuth, RBAC, versioning | Agents could only be trusted once permissions existed |
| 4. Multi-site | Jun to Aug 2026 | Per-site content repos, HTML cache, event pipeline, job worker | Isolation and throughput for agent-scale write volume |
| 5. Governance | Sep to Oct 2026 | Proposals, agent roles, four-eyes review, evidence checks | Turned agent access into a governed operating model |
| 6. Growth loop | Oct to Nov 2026 | First-party analytics, lead outbox, IndexNow, outcome reviews | Measure and improve business results, not just output |

## 6. Adoption and rollout

- **Migration.** 4geeks.com moved from the free Vercel setup to Caxton, then to separate per-site content repositories in June 2026.
- **Agent rollout.** The first governed agent proposals landed on 16 September 2026. Throughput reached a peak of 211 applied edits in the week of 5 October 2026.
- **Steady state.** Since then the agent team applies ~124 proposals per week (8-week average), with 92% of edits live within 24 hours of filing.

## 7. Success metrics

| Level | Metric | Current value |
|---|---|---|
| North star *(proposed)* | Share of content changes authored by agents and accepted without rework | 100% agent-authored; 81% applied without a blocker cycle |
| Leading indicators | Weekly applied proposals; median proposal-to-live time; rejection rate | ~124 / week; 3 minutes; 4.8% |
| Quality | Share of graded outcomes rated good | 71% (145 of 203) |
| Business outcomes | Organic clicks; search impressions; traffic | +135% clicks Jul to Oct 2026; +41% impressions Oct 2026 vs Oct 2025; 308,778 page views in 90 days |

## 8. What I would do differently

*(Draft for review.)*

1. **Start with draft-backed proposals.** The first proposal model required a migration to "Proposals 1.0" (draft as the source of truth) in October 2026. Designing it draft-first would have avoided the cutover.
2. **Instrument before migrating.** Organic clicks fell during the first half of 2026 before recovering. Baseline dashboards and before/after tracking per migration step would have made cause and effect clearer.
3. **Define the outcome-review loop on day one.** Grading agent outcomes started after throughput was already high; earlier grading would have shaped agent conventions sooner.

## Results

| Metric | Value | Type | Source (as of) |
|---|---|---|---|
| Proposals filed | 1,457 (16 Sep to 9 Nov 2026) | Measured | Caxton proposals log (10 Nov 2026) |
| Agent-authored share | 100% of 1,434 closed proposals; 0 from the staff UI | Measured | Proposals log, filtered by actor type (10 Nov 2026) |
| Proposals applied | 1,022: 699 edits, 310 ideas, 13 notes | Measured | Proposals log (10 Nov 2026) |
| Weekly throughput | ~124 applied per week (992 in 8 full weeks, 14 Sep to 8 Nov) | Derived | Proposals weekly KPI history (10 Nov 2026) |
| Peak week | 211 edits applied (week of 5 Oct 2026) | Measured | Proposals weekly KPI history (10 Nov 2026) |
| Proposal-to-live time, edits | Median 3 min; p75 16 min; 92% within 24 h (675 edits) | Measured | Proposals log timestamps (10 Nov 2026) |
| Rejection rate | 4.8% (51 of 1,073 decided) | Derived | Proposals log (10 Nov 2026) |
| Edits with a blocker cycle | 19% (131 of 699) | Measured | Proposals log (10 Nov 2026) |
| Applied proposals by role | SEO specialist 420, copy editor 168, translator 84, orchestrator 21 | Measured | Proposals log, filtered by role (10 Nov 2026) |
| Applied edits by model family | Claude 369, Grok/xAI 314, DeepSeek 15 | Measured | Proposals log, actor model (10 Nov 2026) |
| Graded outcomes | 145 good, 58 bad (71% good) | Measured | Proposals log, staff outcome reviews (10 Nov 2026) |
| Blog posts updated | 180 of 326 (55%) updated 1 Sep to 9 Nov 2026 | Measured | Caxton content index, `updated_at` (10 Nov 2026) |
| Organic clicks, monthly | 3,776 (Jul 2026) to 8,871 (Oct 2026), +135% | Measured | Google Search Console via OpenRush (10 Nov 2026) |
| Search impressions, October | 956,251 (2025) to 1,344,843 (2026), +41% | Measured | Google Search Console via OpenRush (10 Nov 2026) |
| Organic clicks, October year over year | 12,922 (2025) to 8,871 (2026), -31% | Measured | Google Search Console via OpenRush (10 Nov 2026) |
| Site traffic, 90 days | 308,778 page views, 82,363 sessions, 50,210 users | Measured | GA4 BigQuery export via Caxton (as of 9 Nov 2026) |
| Content entries | 883 on 4geeks.com across 14 content types | Measured | Caxton content index (6 Nov 2026) |
| Platform cost | $25/month (previously a free hosting tier that could not run agents) | Measured | Author (Nov 2026) |
| Share of content work by agents | ~90% with human review; ~30% fully autonomous | Estimated | Author (Nov 2026) |

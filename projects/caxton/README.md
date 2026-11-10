# Caxton: the agentic CMS

**Overview** · [Technical architecture](TECHNICAL.md) · [Product decisions](PRODUCT.md)

> I architected and built an agent-native content platform that lets a governed team of AI agents operate a production website. On 4geeks.com, agents now author 100% of content changes and apply a median edit 3 minutes after filing, on a $25/month server.

**Nov 2025 to present** · **Role:** sole architect and main contributor (79% of code) · **License:** MIT · **Repo:** [breatheco-de/caxton-cms](https://github.com/breatheco-de/caxton-cms) · **Live:** [4geeks.com](https://4geeks.com)

## Metrics at a glance

| | |
|---|---|
| **1,022** content changes applied by AI agents in 8 weeks | **100%** of closed proposals authored by agents |
| **3 min** median time from proposal to live | **55%** of the blog updated in 10 weeks |
| **+135%** organic search clicks, July to October 2026 | **$25/month** total platform cost |

## Context

- **Company:** [4Geeks Academy](https://4geeks.com), an AI reskilling company accredited by the Florida Department of Education, operating in the US, Latin America and Europe. I am its co-founder and CTO.
- **Scope:** the platform behind 3 web properties (4geeks.com, fl.4geeksacademy.com, business.4geeks.com), 883 content entries in English and Spanish, and 308,778 page views in the last 90 days.
- **Team:** two core engineers plus three occasional contributors, over 12 months.
- **Starting point:** the site ran on a free hosting tier with no way to run AI agents. No off-the-shelf CMS offered content ownership, a visual editor and a safe interface for agents at the same time.

## My role and scope

I am the sole architect of Caxton and its main contributor. Every technical and architectural decision was mine: storage formats, libraries, queue and caching strategy, performance optimization, logging and observability, and the MCP server architecture. I also set the product direction and the agent operating model.

The code was written with AI assistance under my full supervision: I directed it and reviewed every change. It is the same model Caxton applies to content: AI produces the volume, and a human owns the decisions and the review.

| Contributor | Code lines added | Share | Lines in current codebase | Share |
|---|---|---|---|---|
| **Alejandro Sánchez** | 784,660 | **79%** | 623,243 | **85%** |
| [Alejandro Jiménez Morales](https://www.linkedin.com/in/alejandro-jimenez-morales/) | 149,564 | 15% | 76,659 | 10.5% |
| Replit Agent | 29,931 | 3% | 17,302 | 2.4% |
| Luis Miguel Del Valle | 13,427 | 1% | 6,308 | 0.9% |
| Omar Hidalgo | 14,401 | 1% | 4,738 | 0.6% |

*Measured 6 Nov 2026 over code files only (`.ts`, `.tsx`, `.js`, `.css`, `.sh`), with each person's accounts merged and site content, lockfiles and data files excluded. "Current codebase" is `git blame` over 2,680 files (~732k lines).*

## Headline outcomes

1. **An AI agent team runs the website.** Agents in eight specialized roles filed 1,457 proposals in 8 weeks, and 1,022 went live with a 4.8% rejection rate. [Product, PDR-03](PRODUCT.md#pdr-03-a-team-of-specialized-agent-roles-instead-of-one-general-agent)
2. **Governance enforced in code, not in prompts.** Least-privilege RBAC (40 capabilities, role-scoped MCP connectors), separation of duties that blocks a model from approving its own work, and optimistic concurrency control on every change. [Technical, ADR-04 and ADR-06](TECHNICAL.md#4-architecture-decision-records)
3. **Speed without losing control.** Median proposal-to-live time of 3 minutes; 92% of edits live within 24 hours; 19% of edits corrected through review before publishing. [Product, results](PRODUCT.md#results)
4. **Enterprise patterns at startup cost.** Transactional outbox, event-driven cache invalidation, distributed leases with fencing tokens and atomic releases, all on one $25/month VPS with no managed services. [Technical, ADR-07](TECHNICAL.md#adr-07-embedded-sqlite-a-transactional-outbox-and-a-local-job-queue)
5. **Search growth.** Organic clicks up 135% from July to October 2026 as the agent pipeline came online, with October impressions up 41% year over year and a 97.3/100 on-page SEO score. [Product, results](PRODUCT.md#results)

## Read more

- **[Technical architecture and engineering achievements](TECHNICAL.md):** system context, requirements, 9 Architecture Decision Records, scalability, reliability, security, quality engineering, trade-offs. About 15 minutes.
- **[Product decisions and outcomes](PRODUCT.md):** problem, users, positioning, 8 Product Decision Records, roadmap, rollout, success metrics. About 10 minutes.

## Evidence

- Source code: [github.com/breatheco-de/caxton-cms](https://github.com/breatheco-de/caxton-cms)
- Production: [4geeks.com](https://4geeks.com)
- Article: [How We Run Our Website With AI Agents and One MCP Server](https://4geeks.com/en/blog/ai-tools/run-website-with-ai-agents-mcp-server)
- MCP response standards: [`docs/blog/mcp-standards.md`](https://github.com/breatheco-de/caxton-cms/blob/main/docs/blog/mcp-standards.md)

## Skills and technologies

TypeScript · Node.js · React (streaming SSR) · Vite · Express · Model Context Protocol (MCP) · OAuth 2.1 · RBAC · AI agent orchestration · multi-model agents (Claude, Grok, DeepSeek) · LLM integration (OpenRouter) · SQLite · Zod · Qdrant vector search · GitHub API · Google Cloud Storage · BigQuery · GA4 · Google Search Console · pino · pm2 · Nginx · technical SEO · i18n · A/B testing · system design · product strategy

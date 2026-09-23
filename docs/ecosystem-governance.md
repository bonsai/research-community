# Community Ecosystem Governance

## Purpose

This document defines how `research-community`, `community-based-sales`, `bonsai/MX`, and the `buraiha-p` producer-agent concept work together.

The goal is to build a community-based livelihood system around the proposition:

> **やとわれずに生きていく。AIと仲間と現場で、自分たちの仕事をつくる。**

This is an operating model for small, measurable work experiments. It is not a promise of guaranteed income, passive income, or a shortcut around contracts, taxes, consumer protection, or platform rules.

## Repository hierarchy

```text
research-community  [parent: strategy, research, brand, learning]
├── community-based-sales  [execution: content → email/community → product experiments]
├── MX                  [marketing model: market observation and relationship design]
└── buraiha-p           [producer agent: coordination and experiment facilitation]
```

The hierarchy is **governance and learning ownership**, not a claim that one repository contains or technically controls the others.

## Responsibilities and source of truth

| Repository | Primary responsibility | Source of truth | Must not own |
|---|---|---|---|
| `research-community` | Mission, audience, pillars, research, platform choices, ethical rules, cross-project learning | Strategic hypotheses, research records, brand principles, decision log | Production credentials, private member data, platform access tokens |
| `community-based-sales` | Short-form discovery, email/community nurture, reading/content products, sales experiments | Funnel definition, content manifest, CTA tests, product experiment results | Hidden persuasion, unverified claims, payment/member identity as the only record |
| `MX` | Observe market, segment, hypothesize, position, communicate, measure, learn, update | Market model, customer model, hypotheses, campaigns, measurements | Final legal/commercial approval or private customer data by default |
| `buraiha-p` | Coordinate people, skills, projects, events, and AI-assisted work experiments | Asset maps, experiment plans, matching proposals, retrospectives | Autonomous commitments, unapproved outreach, payment/refund decisions |

## Operating loop

```text
1. Research-community observes culture, work, creator assets, platform conditions, and member needs
2. MX turns observations into market/customer models and explicit hypotheses
3. Community-based-sales turns one approved hypothesis into content, CTA, email, community, or product experiments
4. Buraiha-p helps assemble people and resources for a small real-world test
5. Participants provide consented feedback and outcome data
6. MX measures market response and updates the hypothesis
7. Research-community records the learning, changes the strategy, or stops the experiment
```

Every experiment should have an owner, hypothesis, target audience, offer, channel, cost ceiling, consent requirements, stop condition, measurement plan, and retrospective.

## Governance rules

### Strategy decisions

`research-community` owns the common mission, audience assumptions, content pillars, platform selection principles, and cross-repository decision log. Changes that alter the promise, data policy, or monetization ethics must be recorded here.

### Marketing decisions

`MX` owns the language and artifacts for market understanding: segments, customer signals, cultural signals, positioning, hypotheses, campaign plans, measurements, and learning. It should explain *why* a campaign exists before `community-based-sales` implements it.

### Sales and content decisions

`community-based-sales` owns the implementation of a funnel experiment: short video, landing page, email sequence, community activity, book/product offer, CTA, and conversion measurement. It must link back to the relevant MX hypothesis and the parent strategy decision.

### Producer-agent decisions

`buraiha-p` may suggest matches, agendas, content repurposing, and seven-to-fourteen-day experiments. A human must approve introductions, public claims, contracts, pricing, refunds, member access, personal-data use, and publication.

## Shared artifact contract

Use stable IDs so a decision can be traced across repositories:

```text
RC-YYYYMM-NNN   strategy/research decision
MX-YYYYMM-NNN   market hypothesis or measurement
CBS-YYYYMM-NNN  content/funnel/sales experiment
BHP-YYYYMM-NNN  producer-agent work experiment
```

A cross-repository record should include:

- `id`
- `parent_id` or linked IDs
- `status`: proposed, approved, running, measured, repeated, revised, stopped
- `owner`
- `hypothesis`
- `audience`
- `offer`
- `channel`
- `consent_and_rights`
- `cost_ceiling`
- `success_metrics`
- `stop_condition`
- `result`
- `next_decision`

Do not place API keys, payment credentials, private member records, customer addresses, or private chat transcripts in these repositories.

## Marketing system for the community

The common funnel is:

```text
Instagram / short video / podcast / street and event stories
        ↓ discovery
research-community mission and useful free material
        ↓ consented relationship
email / LINE OA / community discussion
        ↓ participation
7-day work experiment, workshop, event, or reading session
        ↓ proof of value
paid project, cohort, membership, campaign, book, or template
        ↓ learning
MX measurement + community feedback + strategy update
```

The community is not a hidden sales list. State whether a channel is free, paid, sponsored, referral-based, or affiliate-supported; explain data use and withdrawal; respect people who do not buy.

## Metrics

### Parent metrics

- number of consented, useful readers;
- repeat participation after 30 days;
- experiments completed and documented;
- member-reported usefulness;
- correction and response time;
- trust signals: referrals, voluntary contributions, and low complaint rate.

### Sales metrics

- content-to-landing-page click rate;
- opt-in rate and email engagement;
- community participation and retention;
- qualified conversations;
- offer acceptance and repeat work;
- revenue minus direct costs and hours;
- refund, complaint, and unsubscribe rate.

Do not use views, followers, or short-term conversion alone as the definition of success.

## Review cadence

- **Weekly:** `community-based-sales` reviews content and funnel experiments; `buraiha-p` reviews active work experiments.
- **Biweekly:** `MX` updates hypotheses and measurements.
- **Monthly:** `research-community` reviews strategy, platform constraints, rights, trust, and what to repeat or stop.
- **Quarterly:** archive stale assumptions, revise the mission only when evidence warrants it, and reduce tools before adding automation.

## Non-negotiable trust rules

- Never claim guaranteed earnings or “everyone can earn.”
- Clearly label advertising, referral, sponsorship, gifted products, and affiliate compensation.
- Verify current campaign terms, eligibility, region, deadline, and reward conditions from official sources.
- Obtain consent for photos, video, audio, event recordings, AI processing, and reuse.
- Use official APIs, not scraping or browser automation, for production integrations.
- Keep member identity, payment state, consent, and access rights in an independent system.
- Make automation rate-limited, idempotent, stoppable, logged, and human-reviewable.

## First integration milestone

Complete one cross-repository experiment:

1. `research-community` records the mission and audience hypothesis.
2. `MX` creates one market hypothesis and measurement plan.
3. `community-based-sales` produces two short-form pieces, one opt-in asset, and one follow-up sequence as drafts.
4. `buraiha-p` facilitates one seven-day work experiment with five to ten participants.
5. Humans review rights, claims, consent, and CTA before publication.
6. `MX` records results; `research-community` decides repeat, revise, or stop.

The first milestone is complete only when the loop is documented end to end without production API keys or irreversible automation.

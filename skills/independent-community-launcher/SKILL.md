---
name: independent-community-launcher
description: Research, design, and package an independent-work community spanning craft, self-sufficiency, freelance work, AI, creator campaigns, events, podcasts, and social platforms. Use when a user wants to turn existing social accounts and income experiments into a reusable community/online-salon concept, compare platform APIs and bots, create content and podcast plans, or commit the resulting materials to GitHub.
---

# Independent Community Launcher

Turn a creator's existing accounts, skills, income experiments, events, and AI workflow into a coherent community concept and implementation-ready research package. Treat the work as an experiment in building independent work, not as a promise of passive income or guaranteed earnings.

## End-to-end workflow

1. **Collect assets and intent.** Record existing accounts, supplied or publicly verified audience metrics, content types, events, skills, income experiments, preferred/avoided platforms, target participants, geography, and deliverable. Do not invent private information.
2. **Define the proposition.** Prefer “make your own work and livelihood” over a loose list of side hustles. Use pillars such as craft, self-sufficiency, independent work, AI work, influence/events, and media.
3. **Assign channel roles.** Use a hub-and-spoke model: discovery account for people/style/culture, operating account for events/AI/business, and local/travel account for place-based experiments. Do not force identical posts everywhere.
4. **Research feasibility.** For three or more independent platforms, read the workflow-composer skill and use `workflow/run`: one item agent per platform/repository plus one reducer. Prefer official developer docs, terms, pricing, policies, official GitHub, and help pages. Never treat search snippets as sufficient evidence.
5. **Separate actual from planned.** Inspect the repository tree, commits, workflows, code, license, and issues. For platforms, distinguish documented capability from proposed architecture and state when a function is unavailable.
6. **Design architecture.** Keep an independent member database, consent ledger, payment source of truth, and entitlement service. Project access to Discord roles, LINE OA, Slack channels, or a member site. Never make a social platform, OpenChat room, or Spotify playlist the membership authority.
7. **Create the content engine.** Build a podcast plan, recurring series, 30-day test, short-form repurposing plan, CTA, KPI definitions, and event-to-community funnel. Place AI in production and decision experiments; keep real people, work, places, and events as the subject.
8. **Handle monetization carefully.** Treat referral campaigns, delivery work, creator campaigns, events, workshops, and memberships as experiments. Disclose compensation, never guarantee earnings, and verify current conditions, deadlines, regions, fees, and eligibility from official sources.
9. **Package the result.** For GitHub, create a concise README plus focused Markdown files for brand/community, podcast, social/AI strategy, growth/monetization, and platform API research. Use Japanese when appropriate and numbered Markdown references.
10. **Verify and deliver.** Check every intended file, inspect the diff, commit descriptively, and push only when the user explicitly requests a remote update or has authorized that exact repository. Report branch, commit hash, changed files, and unresolved items.

## Platform defaults

- **Discord:** default for automation-heavy core communities with roles, channels, events, bots, and moderation.
- **LINE Official Account + Messaging API:** default for opt-in announcements, reservations, reminders, and CRM. Never conflate this with LINE OpenChat.
- **LINE OpenChat:** optional human-managed/native-feature exchange layer; do not promise external Bot/API control over rooms or posts.
- **Slack:** consider for professional/B2B communities already using Slack; check workspace invitation, SCIM, Marketplace, and history-rate-limit constraints.
- **Instagram:** discovery, comments, short onboarding, and support; keep payment, authorization, and gated content outside Instagram.
- **X:** optional public distribution and listening; use official APIs only with consent, bot labeling, opt-out, and anti-spam controls. Do not force it when the creator dislikes it.
- **Spotify:** music discovery, official links, and playlists; do not design paid gated streaming, synchronized public playback, audio redistribution, or artificial play inflation.

## Required report decisions

For each platform or repository, cover API/Bot status, authentication, events/Webhooks, onboarding, payment and entitlements, roles, content delivery, moderation, analytics, rate limits, review/access requirements, commercial restrictions, consent, privacy, deletion, salon fit, difficulty, recommendation, and at least three authoritative URLs where available. Label third-party sources separately.

## Safety and trust rules

- Never say “everyone can earn,” “guaranteed five万円,” or “effortless passive income.” Describe system-based income as an experiment, not a guarantee.
- Label referral, advertising, sponsorship, free product, and affiliate relationships.
- Obtain consent for photos, video, audio, event recording, AI processing, and reuse.
- Do not feed personal data, unpublished deal information, private community messages, or copyrighted music into AI or third-party systems without permission.
- Use official APIs rather than browser automation or scraping.
- Keep automated actions rate-limited, idempotent, stoppable, logged, and human-reviewable.
- For payments, legal notices, tax, consumer protection, and terms, state uncertainty and recommend expert review when the concrete sales flow warrants it.

## References

- [platform-research-template.md](references/platform-research-template.md) — structured fields and comparison table for platform/API research.
- [community-content-template.md](references/community-content-template.md) — brand, podcast, content, funnel, KPI, and monetization structure.

## GitHub packaging pattern

```text
README.md
docs/brand-and-community.md
docs/podcast-plan.md
docs/social-and-ai-strategy.md
docs/growth-and-monetization.md
research/platform-api-bot-research.md
```

Use small focused commits such as `Add community, podcast, social and monetization concept materials` and `Add platform API and bot research report`. Inspect first and preserve existing project conventions; do not overwrite user files blindly.

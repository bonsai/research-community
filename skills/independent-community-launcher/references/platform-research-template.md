# Platform/API research template

Use this structure for each platform or repository. Keep facts and recommendations separate.

## Item

- Name:
- Official URL:
- As-of date:
- Scope: platform, API, repository, or policy

## Findings

### API and Bot status

State whether an official API/Bot exists. Identify authentication, required account type, scopes, and whether the user must opt in.

### Events and delivery

Describe inbound events, outbound actions, Webhooks, delivery guarantees, signature verification, response deadlines, retries, and idempotency requirements.

### Community capabilities

Cover member onboarding, roles/permissions, payment or entitlement integration, content delivery, moderation, notifications, analytics, and deletion/withdrawal.

### Constraints

Record rate limits, plan/price dependencies, App Review, Business Verification, allowlists, quota modes, platform terms, automation restrictions, commercial use limits, data retention, and user-consent requirements.

### Salon fit

Use one of: high, conditional, low, or unsuitable. Explain the intended role: core community, CRM/announcement, acquisition/support, music discovery, or content link only.

### Implementation difficulty

Use low, medium, or high and name the main sources of complexity.

### Official sources

Provide at least three direct official URLs when available. Prefer developer docs, terms/policies, official pricing, official GitHub, and official help pages. Do not cite a search result snippet as evidence.

## Comparison row

| Target | API/Bot utility | Salon role | Main constraints | Recommendation |
|---|---|---|---|---|
| Name | Summary | Core / CRM / acquisition / auxiliary | Key limits | Adopt / conditional / avoid |

## Reducer checklist

- Do not merge distinct products under one API name. Example: LINE OpenChat is not LINE Messaging API.
- Do not infer a missing API from a related API.
- Mark planned architecture as planned, not implemented.
- Distinguish platform capability from policy permission.
- Prefer an external member/payment source of truth.
- State uncertainty next to the claim.

# Definition of Done (DoD)

A work item is considered **Done** only if all items below are satisfied.

## Engineering
- [ ] Code builds/runs locally
- [ ] Lint/format checks pass
- [ ] Unit tests added/updated where applicable
- [ ] No secrets committed (.env, tokens, credentials)
- [ ] No raw or processed datasets committed (data/raw, data/processed)

## API (when backend changes)
- [ ] OpenAPI docs updated (endpoints reflect current behavior)
- [ ] Input validation added (schemas/models)
- [ ] Error handling is consistent (HTTP status + message)

## Data / Analytics (when ingestion/model changes)
- [ ] Data contract mapping documented
- [ ] Dataset source & version recorded
- [ ] Reproducible run instructions provided

## Docs
- [ ] README updated if behavior/setup changed
- [ ] ADR added if a new major decision is introduced

## Review
- [ ] Changes are small enough to review (or split into PRs)
- [ ] Related issue/feature ID referenced

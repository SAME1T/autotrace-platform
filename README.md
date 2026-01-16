# AutoTrace Platform (Automotive Quality • Traceability • Diagnostics)

AutoTrace; otomotiv üretim hattı ölçümleri, parça/lot/istasyon izlenebilirliği (traceability),
kalite olay yönetimi (NCR/8D/CAPA) ve araç diagnostik/telemetri verilerini (DTC, OBD/CAN)
tek bir platformda birleştiren uçtan uca bir demonstratördür.

## Real-world problem
Üretimdeki ölçüm sapmaları ile sahadaki arıza (DTC) artışlarını ilişkilendirerek:
- Kök neden analizini (RCA) hızlandırmak
- Hatalı lot/istasyon/tedarikçi etkisini daraltmak
- Geri çağırma (recall) maliyetini azaltmak

## Modules
- Traceability: part → lot → station → measurements timeline
- Quality: Defect/NCR + CAPA workflow
- Telemetry: DTC + signals ingestion
- Analytics: manufacturing defect risk & predictive maintenance baselines
- Dashboard: quality overview + trace timeline + telemetry feed

## Tech Stack
FastAPI • PostgreSQL • Alembic • React (Vite) • Docker Compose • GitHub Actions

## Repository Structure
- docs/           -> architecture, data contracts, domain glossary
- services/       -> api, ingestion, analytics
- apps/           -> dashboard
- data/           -> raw (ignored), processed (ignored), samples
- infra/          -> docker, scripts
- tests/          -> automated tests

## Quick start (coming soon)
- docker compose up
- run migrations + seed demo data
- open dashboard

## Roadmap (high level)
- Phase 1: data contracts + DB schema + API skeleton
- Phase 2: ingestion pipelines (Bosch/Scania/NASA/CAN)
- Phase 3: dashboards + analytics baselines

# C4 - Context Diagram (Text)

## System
**AutoTrace Platform** is an internal platform that unifies:
- Manufacturing traceability (part/lot/station/measurements)
- Quality management (defect, NCR/8D, CAPA)
- Vehicle diagnostics & telemetry (DTC, signals)
- Analytics (risk scoring, predictive maintenance baselines)

## People (Actors)
- **Production Engineer**: monitors stations, processes, and traceability timelines
- **Quality Engineer**: manages defects, NCR/8D investigations, CAPA actions
- **Maintenance/Service Analyst**: reviews DTC/telemetry patterns and anomalies
- **Management (Viewer)**: views dashboards and KPIs

## External Systems (Data Sources)
- **Manufacturing Datasets** (public): e.g., Bosch production line measurement dataset
- **Fleet/Failure Datasets** (public): e.g., Scania APS failure dataset
- **Prognostics Datasets** (public): e.g., NASA C-MAPSS RUL datasets
- **CAN/OBD Datasets** (public): e.g., car-hacking / CAN intrusion datasets
- **Optional Real Vehicle OBD Logger** (personal): user-collected, anonymized OBD-II logs

## High-level interactions
- Actors use **Dashboard** to explore KPIs, timelines, and anomaly feeds.
- Dashboard talks to **AutoTrace API**.
- AutoTrace API stores operational data in **PostgreSQL**.
- Ingestion jobs download/prepare datasets and load processed data into the platform.
- Analytics jobs train baseline models and publish risk/anomaly scores to the API.

## Notes
- Raw datasets are never committed to Git.
- Data contracts define the common event formats used across modules.

# API Endpoints v1 (Draft)

This document lists the initial HTTP endpoints for AutoTrace API.
It is aligned with ERD v1 and Data Contracts v1.

Base URL: /api/v1

---

## 1) Health & Meta
- GET  /health
- GET  /version

---

## 2) Traceability (Parts / Lots / Stations / Steps / Measurements)

### Parts
- POST /parts
- GET  /parts?part_serial=&part_number=&lot_id=
- GET  /parts/{part_id}
- GET  /parts/{part_id}/timeline

### Lots
- POST /lots
- GET  /lots?lot_code=&supplier_id=
- GET  /lots/{lot_id}

### Suppliers
- POST /suppliers
- GET  /suppliers
- GET  /suppliers/{supplier_id}

### Stations
- POST /stations
- GET  /stations?station_code=&line_id=
- GET  /stations/{station_id}

### Process Steps
- POST /process-steps
- GET  /process-steps?part_id=&station_id=&from=&to=
- GET  /process-steps/{step_id}

### Measurements
- POST /measurements
- GET  /measurements?process_step_id=&name=&from=&to=

---

## 3) Quality (Defect / NCR / 8D / CAPA)

### Quality Cases
- POST /quality/cases
- GET  /quality/cases?case_type=&status=&severity=&part_id=&station_id=
- GET  /quality/cases/{case_id}
- PATCH /quality/cases/{case_id}                 # summary/description/severity updates
- POST  /quality/cases/{case_id}/status          # status transition record

### Quality Actions
- POST /quality/cases/{case_id}/actions
- GET  /quality/cases/{case_id}/actions
- PATCH /quality/actions/{action_id}             # status/owner/due_date/notes

---

## 4) Telemetry / Diagnostics

### Vehicles
- POST /vehicles
- GET  /vehicles?vehicle_key=
- GET  /vehicles/{vehicle_id}

### Telemetry Points
- POST /telemetry/ingest                          # bulk insert signals
- GET  /telemetry?vehicle_id=&signal_name=&from=&to=

### DTC Events
- POST /dtc/ingest                                # bulk insert dtc codes
- GET  /dtc?vehicle_id=&code=&from=&to=

---

## 5) Analytics (Optional v1)
- GET  /analytics/scores?score_type=&part_id=&vehicle_id=
- POST /analytics/scores                          # store computed score

---

## Notes
- All timestamps are ISO-8601 UTC.
- Validation rules follow Data Contract v1 schemas.
- Auth will be added after core CRUD is stable.

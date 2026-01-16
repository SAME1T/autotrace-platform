# ERD v1 (Database Schema Draft)

This document defines the initial relational schema for AutoTrace.
It is designed to support:
- Traceability timeline (part -> lot -> station/process_step -> measurements)
- Quality cases (defect/NCR/8D/CAPA) with state transitions
- Telemetry signals and DTC events
- Analytics scores linked to parts/vehicles

> Note: IDs are UUID unless stated otherwise.

---

## 1) Master Data

### suppliers
- id (uuid, pk)
- name (text, unique)
- country (text, null)
- created_at (timestamptz)

### lots
- id (uuid, pk)
- supplier_id (uuid, fk -> suppliers.id)
- lot_code (text)
- produced_at (timestamptz, null)
- created_at (timestamptz)

### parts
- id (uuid, pk)
- part_serial (text, unique)
- part_number (text)
- lot_id (uuid, fk -> lots.id, null)
- created_at (timestamptz)

---

## 2) Manufacturing / Traceability

### stations
- id (uuid, pk)
- station_code (text, unique)
- line_id (text, null)
- name (text, null)
- created_at (timestamptz)

### process_steps
- id (uuid, pk)
- part_id (uuid, fk -> parts.id)
- station_id (uuid, fk -> stations.id)
- step_name (text)
- shift_id (text, null)
- operator_id (text, null)
- machine_id (text, null)
- started_at (timestamptz, null)
- finished_at (timestamptz, null)
- created_at (timestamptz)

### measurements
- id (uuid, pk)
- process_step_id (uuid, fk -> process_steps.id)
- name (text)
- value (double precision)
- unit (text, null)
- lower_limit (double precision, null)
- upper_limit (double precision, null)
- is_ok (boolean, null)
- created_at (timestamptz)

Indexes:
- idx_process_steps_part_id
- idx_measurements_process_step_id
- idx_measurements_name

---

## 3) Quality Management (Defect / NCR / 8D / CAPA)

### quality_cases
- id (uuid, pk)
- case_type (text)         # DEFECT | NCR | CAPA | EIGHT_D
- status (text)            # OPEN | IN_PROGRESS | WAITING | CLOSED | REJECTED
- severity (text)          # LOW | MEDIUM | HIGH | CRITICAL
- summary (text)
- description (text, null)
- related_part_id (uuid, fk -> parts.id, null)
- related_station_id (uuid, fk -> stations.id, null)
- created_at (timestamptz)
- updated_at (timestamptz)

### quality_actions
- id (uuid, pk)
- case_id (uuid, fk -> quality_cases.id)
- action_type (text)       # CONTAINMENT | RCA | CORRECTIVE | PREVENTIVE | VERIFICATION
- owner (text, null)
- due_date (date, null)
- status (text)            # OPEN | DONE | CANCELED
- notes (text, null)
- created_at (timestamptz)

### quality_state_changes
- id (uuid, pk)
- case_id (uuid, fk -> quality_cases.id)
- from_status (text)
- to_status (text)
- changed_by (text, null)
- changed_at (timestamptz)

Indexes:
- idx_quality_cases_type_status
- idx_quality_cases_related_part_id
- idx_quality_actions_case_id
- idx_quality_state_changes_case_id

---

## 4) Telemetry / Diagnostics

### vehicles
- id (uuid, pk)
- vehicle_key (text, unique)        # anonymized ID
- vin_hash (text, null)             # optional, never raw VIN
- created_at (timestamptz)

### telemetry_points
- id (uuid, pk)
- vehicle_id (uuid, fk -> vehicles.id)
- signal_name (text)
- signal_value (double precision)
- unit (text, null)
- occurred_at (timestamptz)
- created_at (timestamptz)

### dtc_events
- id (uuid, pk)
- vehicle_id (uuid, fk -> vehicles.id)
- code (text)                       # e.g., P0300
- description (text, null)
- occurred_at (timestamptz)
- created_at (timestamptz)

Indexes:
- idx_telemetry_vehicle_time
- idx_telemetry_signal_name
- idx_dtc_vehicle_time
- idx_dtc_code

---

## 5) Analytics Scores (Optional v1)

### analytics_scores
- id (uuid, pk)
- score_type (text)                 # DEFECT_RISK | FAILURE_RISK | ANOMALY_SCORE | RUL
- part_id (uuid, fk -> parts.id, null)
- vehicle_id (uuid, fk -> vehicles.id, null)
- score_value (double precision)
- model_version (text, null)
- computed_at (timestamptz)

Rule:
- Either part_id or vehicle_id must be set (not both null).

---

## Relationships Summary
- suppliers 1---N lots
- lots 1---N parts
- parts 1---N process_steps
- stations 1---N process_steps
- process_steps 1---N measurements
- quality_cases N---0/1 parts
- quality_cases N---0/1 stations
- quality_cases 1---N quality_actions
- quality_cases 1---N quality_state_changes
- vehicles 1---N telemetry_points
- vehicles 1---N dtc_events
- analytics_scores N---0/1 parts
- analytics_scores N---0/1 vehicles

# Data Retention and Disposal Record Template

Informative Appendix (non-normative)

This appendix provides an illustrative template for documenting data retention, secure deletion, disposal verification, exceptions, and customer-requested deletion events in autonomous penetration testing workflows. It is intended to help platform operators, customers, and reviewers collect evidence for existing APTS requirements. It does not create or modify any APTS requirement.

## Purpose

APTS requires operators to define retention periods by data classification, justify retention, track expiration dates, securely delete data, verify deletion, and provide destruction proof at higher assurance tiers. Those activities are often split across evidence stores, vaults, logs, backups, and ticketing systems.

This appendix provides:

- a compact record structure for retention and disposal events
- an example YAML record
- a JSON-equivalent structure
- review questions for customers and reviewers
- cross-references to the APTS requirements this artifact helps support

## Primary Use Cases

Consider using this record when documenting:

- retention decisions for engagement data sets
- retention exceptions or customer-approved extensions
- routine disposal at the end of a retention period
- customer-initiated deletion requests
- credential or restricted-data purge events
- backup, archive, or derived-artifact deletion tracking
- evidence supporting a data destruction certificate

## Design Principles

A retention and disposal record should:

- identify the data set without exposing unnecessary sensitive content
- link each data class to a retention period and justification
- record expiration dates and reminder events
- distinguish primary storage, backup storage, archives, logs, and derived artifacts
- document the secure deletion method used for each storage medium
- preserve verification evidence that retrieval or recovery was expected to fail
- capture exceptions, legal holds, customer approvals, and deletion-request handling

## Recommended Template Sections

### 1. Record metadata

Use stable identifiers so the record can be correlated with engagement records, evidence manifests, deletion logs, and customer requests.

Recommended fields:

- `record_id`
- `engagement_id`
- `customer_reference`
- `data_set_id`
- `record_status`
- `created_at`
- `last_updated_at`
- `owner`
- `reviewers`

Suggested `record_status` values:

- `active_retention`
- `deletion_scheduled`
- `deletion_completed`
- `exception_active`
- `legal_hold_active`
- `customer_request_in_progress`

### 2. Data set inventory

Describe the data set and related locations without embedding the sensitive data itself.

Recommended fields:

- `data_category`
- `classification`
- `source_system`
- `storage_locations`
- `derived_artifacts`
- `backup_locations`
- `contains_credentials`
- `contains_personal_data`
- `contains_customer_confidential_data`
- `evidence_manifest_reference`

Suggested `data_category` values:

- `finding_report`
- `raw_scan_data`
- `credential_or_secret`
- `audit_log`
- `screenshot_or_recording`
- `tool_output`
- `customer_upload`
- `derived_summary`
- `backup_or_archive`

### 3. Retention basis

Document why the data is retained and when it should expire.

Recommended fields:

- `retention_period`
- `retention_start_at`
- `retention_expires_at`
- `retention_basis`
- `contractual_reference`
- `regulatory_reference`
- `operational_need`
- `customer_approval_reference`
- `reminder_schedule`
- `next_review_at`

### 4. Exceptions and holds

Capture any condition that extends or suspends deletion.

Recommended fields:

- `exception_type`
- `exception_reason`
- `approved_by`
- `approved_at`
- `approval_reference`
- `new_expiration_date`
- `legal_hold_reference`
- `customer_notification_reference`

Suggested `exception_type` values:

- `customer_approved_extension`
- `legal_hold`
- `regulatory_obligation`
- `incident_investigation`
- `billing_or_dispute_hold`
- `none`

### 5. Disposal execution

Record the deletion action by storage medium and location.

Recommended fields:

- `deletion_request_id`
- `deletion_trigger`
- `scheduled_deletion_at`
- `deletion_started_at`
- `deletion_completed_at`
- `storage_medium`
- `deletion_method`
- `responsible_system_or_role`
- `deletion_log_reference`
- `cryptographic_key_reference`

Suggested `deletion_method` values:

- `cryptographic_erasure`
- `provider_secure_delete_api`
- `nist_sp_800_88_clear`
- `nist_sp_800_88_purge`
- `multi_pass_overwrite`
- `manufacturer_secure_erase`
- `physical_destruction`

### 6. Verification and recovery test

Record how deletion was verified and whether recovery attempts failed as expected.

Recommended fields:

- `primary_storage_verified`
- `backup_storage_verified`
- `archive_storage_verified`
- `derived_artifacts_verified`
- `recovery_test_performed`
- `recovery_test_result`
- `verification_method`
- `verified_by`
- `verified_at`
- `verification_evidence_reference`

Suggested `recovery_test_result` values:

- `not_recoverable`
- `partially_recoverable_requires_remediation`
- `recoverable_failed`
- `not_applicable_documented`

### 7. Customer request handling

Use this section for customer-initiated deletion, erasure, or disposal requests.

Recommended fields:

- `customer_request_id`
- `request_received_at`
- `request_type`
- `acknowledged_at`
- `acknowledgement_reference`
- `processing_sla`
- `completed_within_sla`
- `customer_certificate_reference`

Suggested `request_type` values:

- `engagement_data_deletion`
- `credential_purge`
- `data_subject_erasure`
- `contract_end_disposal`
- `evidence_package_removal`

### 8. Evidence checklist

Attach or reference the artifacts customers and reviewers may request.

Recommended evidence:

- data classification record
- retention policy mapping
- retention exception approval, if applicable
- reminder or expiration notification
- deletion job log
- secure deletion method evidence
- backup/archive deletion evidence
- failed recovery test evidence
- destruction certificate, when applicable
- customer acknowledgement or notification, when applicable

## Example YAML Template

```yaml
record_id: rdr-2026-0081
engagement_id: eng-2026-001
customer_reference: customer-acme
record_status: deletion_completed
created_at: 2026-04-01T09:00:00Z
last_updated_at: 2026-07-01T12:45:00Z
owner: data-governance
reviewers:
  - platform-security-01

data_set:
  data_set_id: raw-scan-data-eng-2026-001
  data_category: raw_scan_data
  classification: confidential
  source_system: web-scanner
  storage_locations:
    - object-store://engagements/eng-2026-001/raw-scan-data/
  derived_artifacts:
    - evidence-manifest:epm-2026-001
    - report-draft:report-2026-001-v2
  backup_locations:
    - backup-set-2026-04-week-1
  contains_credentials: false
  contains_personal_data: true
  contains_customer_confidential_data: true
  evidence_manifest_reference: epm-2026-001

retention:
  retention_period: 90_days
  retention_start_at: 2026-04-01T00:00:00Z
  retention_expires_at: 2026-06-30T23:59:59Z
  retention_basis: contractual_engagement_terms
  contractual_reference: msa-2026-acme-section-7
  regulatory_reference: none
  operational_need: support customer validation and retest window
  customer_approval_reference: customer-approval-2026-041
  reminder_schedule:
    - 2026-05-31T09:00:00Z
    - 2026-06-23T09:00:00Z
  next_review_at: 2026-06-23T09:00:00Z

exception_or_hold:
  exception_type: none
  exception_reason: null
  approved_by: null
  approved_at: null
  approval_reference: null
  new_expiration_date: null
  legal_hold_reference: null
  customer_notification_reference: null

disposal:
  deletion_request_id: del-2026-0081
  deletion_trigger: retention_expired
  scheduled_deletion_at: 2026-07-01T10:00:00Z
  deletion_started_at: 2026-07-01T10:03:00Z
  deletion_completed_at: 2026-07-01T10:18:00Z
  locations:
    - storage_location: object-store://engagements/eng-2026-001/raw-scan-data/
      storage_medium: cloud_object_storage
      deletion_method: provider_secure_delete_api
      responsible_system_or_role: retention-worker
      deletion_log_reference: logs/deletion/del-2026-0081-primary.json
    - storage_location: backup-set-2026-04-week-1
      storage_medium: encrypted_backup
      deletion_method: cryptographic_erasure
      responsible_system_or_role: backup-controller
      deletion_log_reference: logs/deletion/del-2026-0081-backup.json
      cryptographic_key_reference: key-destruction-log-2026-0081

verification:
  primary_storage_verified: true
  backup_storage_verified: true
  archive_storage_verified: true
  derived_artifacts_verified: true
  recovery_test_performed: true
  recovery_test_result: not_recoverable
  verification_method: retrieval_attempt_and_key-destruction-check
  verified_by: platform-security-01
  verified_at: 2026-07-01T12:30:00Z
  verification_evidence_reference: evidence/deletion/del-2026-0081-verification.md

customer_request:
  customer_request_id: null
  request_received_at: null
  request_type: null
  acknowledged_at: null
  acknowledgement_reference: null
  processing_sla: null
  completed_within_sla: null
  customer_certificate_reference: destruction-certificate-2026-0081.pdf
```

## JSON-Equivalent Structure

```json
{
  "record_id": "rdr-2026-0081",
  "engagement_id": "eng-2026-001",
  "record_status": "deletion_completed",
  "data_set": {
    "data_set_id": "raw-scan-data-eng-2026-001",
    "data_category": "raw_scan_data",
    "classification": "confidential",
    "contains_personal_data": true,
    "evidence_manifest_reference": "epm-2026-001"
  },
  "retention": {
    "retention_period": "90_days",
    "retention_expires_at": "2026-06-30T23:59:59Z",
    "retention_basis": "contractual_engagement_terms"
  },
  "disposal": {
    "deletion_request_id": "del-2026-0081",
    "deletion_trigger": "retention_expired",
    "deletion_completed_at": "2026-07-01T10:18:00Z"
  },
  "verification": {
    "primary_storage_verified": true,
    "backup_storage_verified": true,
    "recovery_test_performed": true,
    "recovery_test_result": "not_recoverable"
  }
}
```

## Field Mapping to APTS Requirements

| Record area | Primary requirements |
| --- | --- |
| Data classification and storage inventory | `APTS-TP-012`, `APTS-TP-013`, `APTS-AR-015` |
| Retention period, basis, reminders, and exceptions | `APTS-TP-015` |
| Credential and restricted-data handling | `APTS-MR-019`, `APTS-TP-015` |
| Deletion method and disposal execution logs | `APTS-TP-015`, `APTS-TP-016` |
| Recovery testing and verification evidence | `APTS-TP-015`, `APTS-TP-016` |
| Customer-requested deletion handling | `APTS-TP-015`, `APTS-TP-016` |
| Evidence package and downstream handoff references | `APTS-AR-010`, `APTS-RP-005`, `APTS-RP-015` |

## Validation Guidance for Customers and Reviewers

When reviewing a retention and disposal record, consider asking:

1. Does the record identify the data set and storage locations without disclosing unnecessary sensitive content?
2. Is the retention period justified by classification, contract, regulation, or operational need?
3. Are credentials and restricted data subject to shorter retention periods where required?
4. Were expiration reminders and periodic reviews recorded before deletion?
5. If retention was extended, is there an approved exception, legal hold, or customer authorization?
6. Does the deletion method match the storage medium and the operator's documented policy?
7. Were primary storage, backup storage, archives, and derived artifacts all considered?
8. Did recovery or retrieval tests fail as expected, or is any recoverable data tracked for remediation?
9. If the customer initiated deletion, was the request acknowledged and completed within the documented SLA?
10. Is a destruction certificate available when required by the platform's tier or customer agreement?

## Usage Notes

This template is intentionally illustrative. Operators may keep equivalent records in governance platforms, data catalogs, ticketing systems, vaults, deletion pipelines, or customer portals as long as the evidence is complete, reviewable, and available to customers when required by APTS.

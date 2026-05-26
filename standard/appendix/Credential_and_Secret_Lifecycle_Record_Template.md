# Credential and Secret Lifecycle Record Template

Informative Appendix (non-normative)

This appendix provides an illustrative template for documenting the lifecycle of credentials and secrets used, encountered, or generated during autonomous penetration testing. It helps platform operators, customers, and reviewers inspect credential provisioning, discovery, access, rotation, revocation, retention, disposal, and exception handling consistently. It does not create or modify APTS requirements.

## Purpose

APTS-SE-023 requires a complete lifecycle for credentials and secrets used, encountered, or generated during testing. Related requirements cover discovered credential protection, API authentication, sensitive data handling, retention, and destruction proof. A dedicated lifecycle record helps reviewers answer a practical question: for each credential or secret reference, can the platform show where it came from, who or what used it, how it was protected, and when it was rotated or destroyed?

This appendix shows:

- a compact field set for credential and secret lifecycle tracking
- an example YAML record
- a JSON-equivalent structure
- reviewer questions for customers and security reviewers
- cross-references to related APTS requirements and appendices

## Primary Use Cases

Use a credential and secret lifecycle record when the platform needs to document:

- client-provided test credentials provisioned for an engagement
- platform-issued temporary credentials or API tokens
- target-discovered credentials, keys, tokens, certificates, or connection strings
- credential references exposed to an agent while secret values remain in a vault
- authorized tool-time credential use
- rotation, revocation, quarantine, or destruction after use
- exceptions to default retention, access, or rotation policy

## Design Principles

A credential and secret lifecycle record should:

- track secret references, not plaintext secret values
- identify provenance and engagement scope for every credential
- show whether the credential is client-provided, platform-issued, target-discovered, or provider-issued
- document who or what can resolve the secret reference to the underlying value
- preserve access and use evidence without copying secret material into logs or reports
- connect retention and disposal status to the platform's retention policy and destruction evidence
- make emergency actions such as quarantine, immediate rotation, or customer notification auditable

## Recommended Template Sections

### 1. Record metadata

Use stable identifiers so the record can be correlated with vault entries, access logs, audit events, and disposal evidence.

Recommended fields:

- `secret_lifecycle_record_id`
- `engagement_id`
- `secret_reference_id`
- `record_version`
- `status`
- `created_at`
- `last_updated_at`
- `record_owner`

Suggested status values:

- `active`
- `quarantined`
- `rotated`
- `revoked`
- `destroyed`
- `retention_exception_active`

### 2. Provenance and classification

Document where the credential came from and how sensitive it is.

Recommended fields:

- `provenance`
- `secret_type`
- `data_classification`
- `source_system`
- `discovered_by_tool_or_agent`
- `discovery_timestamp`
- `plaintext_never_logged_verification`

Suggested provenance values:

- `client_provided`
- `platform_issued`
- `target_discovered`
- `provider_issued`
- `operator_supplied`

Suggested secret types:

- `password`
- `api_key`
- `oauth_token`
- `session_cookie`
- `ssh_private_key`
- `certificate`
- `database_connection_string`
- `cloud_access_key`
- `basic_auth_header`

### 3. Scope and authorization

Record where the credential can be used and who or what may resolve it.

Recommended fields:

- `authorized_engagement_scope`
- `authorized_targets`
- `authorized_tools`
- `authorized_autonomy_levels`
- `credential_manager_ref`
- `resolution_authority`
- `reuse_policy`
- `cross_engagement_reuse_allowed`
- `subprocess_or_remote_agent_delegation`

### 4. Protection controls

Document the controls protecting the secret value and the secret-free reference exposed to the agent.

Recommended fields:

- `vault_location_ref`
- `encryption_control_ref`
- `access_control_policy_ref`
- `secret_reference_format`
- `redaction_policy_ref`
- `model_context_exposure`
- `log_scrubbing_status`
- `backup_handling`

### 5. Access and usage evidence

Record credential use in an auditable but secret-free way.

Recommended fields:

- `access_event_id`
- `timestamp`
- `actor_type`
- `actor_id`
- `tool_or_connector`
- `target_ref`
- `purpose`
- `approval_ref`
- `result`
- `audit_log_ref`

### 6. Rotation, revocation, and quarantine

Track lifecycle actions that reduce exposure after use or discovery.

Recommended fields:

- `rotation_required`
- `rotation_due_at`
- `rotated_at`
- `revocation_required`
- `revoked_at`
- `quarantine_reason`
- `customer_notification_ref`
- `post_rotation_validation_ref`

### 7. Retention, disposal, and exceptions

Connect credential handling to retention and destruction evidence.

Recommended fields:

- `retention_policy_ref`
- `retention_expires_at`
- `disposal_method`
- `destroyed_at`
- `destruction_evidence_ref`
- `destruction_log_hash`
- `exception_ref`
- `exception_approver_ref`
- `exception_expires_at`

## Example YAML Template

```yaml
secret_lifecycle_record_id: cslr-2026-0042
engagement_id: eng-2026-001
secret_reference_id: credref-7f3a2c
record_version: "1.0"
status: rotated
created_at: "2026-05-10T09:15:00Z"
last_updated_at: "2026-05-12T18:30:00Z"
record_owner: credential-governance-owner

provenance_and_classification:
  provenance: target_discovered
  secret_type: api_key
  data_classification: RESTRICTED
  source_system: app.example.com/.env
  discovered_by_tool_or_agent: web-content-inspection-tool
  discovery_timestamp: "2026-05-10T09:15:00Z"
  plaintext_never_logged_verification:
    verification_method: canary-and-artifact-scan
    verification_result: passed
    evidence_ref: artifact-scan-2026-0510-09

scope_and_authorization:
  authorized_engagement_scope: eng-2026-001
  authorized_targets:
    - app.example.com
  authorized_tools:
    - credential-vault-resolver
    - http-validation-client
  authorized_autonomy_levels:
    - L1 Assisted
    - L2 Supervised
  credential_manager_ref: credential-manager-prod#credref-7f3a2c
  resolution_authority:
    - tool-execution-layer
  reuse_policy: no-autonomous-reuse-without-human-approval
  cross_engagement_reuse_allowed: false
  subprocess_or_remote_agent_delegation:
    allowed: false
    approval_ref: null

protection_controls:
  vault_location_ref: restricted-vault#entry-88421
  encryption_control_ref: vault-policy-2026-restricted
  access_control_policy_ref: vault-acl-eng-2026-001
  secret_reference_format: opaque-reference-only
  redaction_policy_ref: redaction-policy-secrets-v3
  model_context_exposure: reference_only
  log_scrubbing_status: enabled-before-tool-result-persistence
  backup_handling: encrypted-backup-with-source-retention-policy

access_and_usage_evidence:
  - access_event_id: vault-access-2026-771
    timestamp: "2026-05-10T09:18:00Z"
    actor_type: tool_execution_layer
    actor_id: http-validation-client
    tool_or_connector: http-validation-client
    target_ref: app.example.com
    purpose: confirm whether discovered key is active without exposing value to agent context
    approval_ref: human-review-approval-2026-117
    result: active-key-confirmed
    audit_log_ref: audit-log-2026-05-10#event-9912

rotation_revocation_and_quarantine:
  rotation_required: true
  rotation_due_at: "2026-05-10T11:15:00Z"
  rotated_at: "2026-05-10T10:04:00Z"
  revocation_required: true
  revoked_at: "2026-05-10T10:04:00Z"
  quarantine_reason: target-discovered-live-api-key
  customer_notification_ref: notification-2026-0510-credentials
  post_rotation_validation_ref: validation-2026-0510-key-inactive

retention_disposal_and_exceptions:
  retention_policy_ref: retention-policy-restricted-credentials-v2
  retention_expires_at: "2026-08-08T00:00:00Z"
  disposal_method: crypto-shred-vault-entry-and-delete-derived-cache
  destroyed_at: null
  destruction_evidence_ref: pending-until-retention-expiry
  destruction_log_hash: null
  exception_ref: null
  exception_approver_ref: null
  exception_expires_at: null
```

## JSON-Equivalent Structure

```json
{
  "secret_lifecycle_record_id": "cslr-2026-0042",
  "engagement_id": "eng-2026-001",
  "secret_reference_id": "credref-7f3a2c",
  "record_version": "1.0",
  "status": "rotated",
  "provenance_and_classification": {
    "provenance": "target_discovered",
    "secret_type": "api_key",
    "data_classification": "RESTRICTED",
    "source_system": "app.example.com/.env",
    "discovered_by_tool_or_agent": "web-content-inspection-tool",
    "plaintext_never_logged_verification": {
      "verification_method": "canary-and-artifact-scan",
      "verification_result": "passed",
      "evidence_ref": "artifact-scan-2026-0510-09"
    }
  },
  "scope_and_authorization": {
    "authorized_engagement_scope": "eng-2026-001",
    "authorized_targets": ["app.example.com"],
    "authorized_tools": ["credential-vault-resolver", "http-validation-client"],
    "credential_manager_ref": "credential-manager-prod#credref-7f3a2c",
    "reuse_policy": "no-autonomous-reuse-without-human-approval",
    "cross_engagement_reuse_allowed": false
  },
  "protection_controls": {
    "vault_location_ref": "restricted-vault#entry-88421",
    "secret_reference_format": "opaque-reference-only",
    "model_context_exposure": "reference_only",
    "log_scrubbing_status": "enabled-before-tool-result-persistence"
  },
  "rotation_revocation_and_quarantine": {
    "rotation_required": true,
    "rotated_at": "2026-05-10T10:04:00Z",
    "revocation_required": true,
    "revoked_at": "2026-05-10T10:04:00Z",
    "customer_notification_ref": "notification-2026-0510-credentials"
  },
  "retention_disposal_and_exceptions": {
    "retention_policy_ref": "retention-policy-restricted-credentials-v2",
    "retention_expires_at": "2026-08-08T00:00:00Z",
    "disposal_method": "crypto-shred-vault-entry-and-delete-derived-cache",
    "destruction_evidence_ref": "pending-until-retention-expiry"
  }
}
```

## Reviewer Questions

When inspecting a credential and secret lifecycle record, ask:

- does the record identify the credential by opaque reference rather than plaintext value
- is the provenance clear enough to distinguish client-provided, platform-issued, provider-issued, and target-discovered credentials
- is the credential scoped to a single engagement or is any broader use explicitly justified and approved
- can reviewers verify that plaintext secret values did not enter model context, logs, draft reports, crash dumps, or telemetry
- does each credential access event identify the actor, tool, target, purpose, approval basis, and audit-log reference
- were discovered live credentials quarantined, rotated, revoked, and communicated to the customer within the documented process
- does the retention expiry align with the credential data classification and engagement agreement
- is destruction evidence available or scheduled with a clear disposal method and verification reference
- are exceptions time-bounded, approved by an authorized role, and visible in the audit trail

## Relationship to Existing APTS Artifacts

This template complements, but does not replace:

- [Rules of Engagement Template](Rules_of_Engagement_Template.md) for defining authorized credential use before an engagement
- [Scope Change Decision Record Template](Scope_Change_Decision_Record_Template.md) for scope or authorization changes involving credential use
- [Data Retention and Disposal Record Template](Data_Retention_and_Disposal_Record_Template.md) for broader retention and deletion evidence
- [Evidence Package Manifest](Evidence_Package_Manifest.md) for finding evidence provenance and downstream handoff

## Related Requirements

This template can help collect evidence for:

- APTS-SE-023: Credential and Secret Lifecycle Governance
- APTS-MR-019: Discovered Credential Protection
- APTS-TP-003: API Security and Authentication
- APTS-TP-013: Sensitive Data Discovery and Handling
- APTS-TP-015: Data Retention and Secure Deletion
- APTS-TP-016: Data Destruction Proof and Certification
- APTS-AR-015: Evidence Classification and Sensitive Data Handling

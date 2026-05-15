# Model Change and Drift Record Template

Informative Appendix (non-normative)

This appendix provides an illustrative template for documenting model changes, behavioral comparisons, drift detections, human review, and rollback decisions for autonomous penetration testing platforms. It is intended to help platform operators, customers, and reviewers collect evidence for existing APTS requirements. It does not create or modify any APTS requirement.

## Purpose

APTS requires operators to pin model versions, track AI/ML model changes, validate behavior after changes, detect behavioral drift, and re-attest after material foundation model changes. Those activities often involve several teams and artifacts, so customers and reviewers benefit from one compact record that connects the change-management decision to the evidence that supports it.

This appendix provides:

- a practical field set for model changes and drift events
- an example YAML record
- a JSON-equivalent structure
- review questions for customers and reviewers
- cross-references to the APTS requirements this artifact helps support

## Primary Use Cases

Consider using this record when documenting:

- a foundation model provider change
- a model family or version update
- fine-tuning, adapter, system prompt, safety policy, or tool-use wiring changes that affect model-mediated decisions
- a fallback-provider or routing-policy change
- behavioral drift in an externally hosted model where the operator did not initiate a local code or configuration change
- rollback from a changed model configuration to a prior baseline

## Design Principles

A model change and drift record should:

- identify the previous and proposed model configuration unambiguously
- separate routine model inventory updates from material changes requiring re-attestation
- connect behavioral comparisons to the safety-critical decision paths tested
- document drift detection results and affected decision paths
- capture human review before deployment when safety-critical behavior changes
- preserve rollback evidence and supersession relationships
- avoid exposing customer secrets, prompts, credentials, or restricted engagement data unnecessarily

## Recommended Template Sections

### 1. Record metadata

Use stable identifiers so the record can be linked to change tickets, conformance claims, drift alerts, and customer notifications.

Recommended fields:

- `record_id`
- `change_ticket_id`
- `record_type`
- `status`
- `created_at`
- `last_updated_at`
- `owner`
- `reviewers`

Suggested `record_type` values:

- `planned_model_change`
- `emergency_model_change`
- `provider_side_drift`
- `rollback`
- `post_change_review`

### 2. Model configuration

Record both the prior and candidate model configurations. The exact version format may vary by provider, but the record should be specific enough for review and rollback.

Recommended fields:

- `provider`
- `model_family`
- `model_name`
- `model_version`
- `deployment_environment`
- `region_or_endpoint`
- `inference_route`
- `fallback_route`
- `system_policy_reference`
- `tool_policy_reference`
- `behavioral_fingerprint_reference`

### 3. Change summary and materiality

Explain what changed and whether the change is material under the operator's APTS-TP-022 policy.

Recommended fields:

- `change_type`
- `change_summary`
- `reason_for_change`
- `materiality_decision`
- `materiality_basis`
- `affected_domains`
- `requires_re_attestation`
- `customer_notification_required`

Suggested `change_type` values:

- `provider_change`
- `model_family_change`
- `model_version_update`
- `fine_tune_or_adapter_change`
- `system_prompt_or_policy_change`
- `tool_use_or_action_space_change`
- `fallback_provider_change`
- `routing_policy_change`
- `detected_drift_without_operator_change`

### 4. Behavioral comparison

Capture the comparison between the previous model behavior and the candidate or observed model behavior, especially on safety-critical decisions.

Recommended fields:

- `test_set_reference`
- `baseline_run_id`
- `candidate_run_id`
- `scope_decision_delta`
- `escalation_decision_delta`
- `impact_classification_delta`
- `manipulation_resistance_delta`
- `safety_control_delta`
- `summary_result`
- `evidence_references`

### 5. Drift detection record

Use this section when the operator detects model behavior changes that were not introduced by an operator-controlled deployment.

Recommended fields:

- `drift_alert_id`
- `detected_at`
- `detection_source`
- `baseline_reference`
- `observed_deviation`
- `affected_decision_paths`
- `tolerance_threshold`
- `threshold_exceeded`
- `operator_alerted_at`
- `blocked_or_limited_paths`
- `acknowledged_by`
- `acknowledged_at`

### 6. Human review and re-attestation

Document who reviewed the change, what evidence they inspected, and whether re-attestation or customer notification was completed.

Recommended fields:

- `review_required`
- `reviewer_name_or_role`
- `review_started_at`
- `review_completed_at`
- `review_decision`
- `review_notes`
- `re_attestation_scope`
- `conformance_claim_updated`
- `foundation_model_disclosure_updated`
- `customer_notification_status`

Suggested `review_decision` values:

- `approved_for_deployment`
- `approved_with_limits`
- `requires_more_testing`
- `rejected`
- `rollback_required`

### 7. Rollback and supersession

Preserve the operational path back to the prior model configuration and make superseded records traceable.

Recommended fields:

- `rollback_plan_reference`
- `rollback_test_run_id`
- `rollback_test_result`
- `previous_record_id`
- `supersedes_record_id`
- `superseded_by_record_id`
- `rollback_completed_at`

### 8. Evidence checklist

Attach or reference the artifacts customers and reviewers may request.

Recommended evidence:

- model registry entry
- pinned model configuration or lock file
- behavioral fingerprint
- baseline and candidate test run output
- drift alert or scheduled drift test output
- human review record
- re-attestation record, when applicable
- updated conformance claim, when applicable
- updated foundation model disclosure, when applicable
- customer notification artifact, when applicable
- rollback test evidence

## Example YAML Template

```yaml
record_id: mcdr-2026-0042
change_ticket_id: change-2026-117
record_type: planned_model_change
status: approved_for_deployment
created_at: 2026-05-01T09:00:00Z
last_updated_at: 2026-05-01T15:30:00Z
owner: platform-governance
reviewers:
  - safety-reviewer-01
  - customer-assurance-01

previous_model:
  provider: example-ai-provider
  model_family: example-secure-model
  model_name: example-secure-model-4
  model_version: 4.2.1
  deployment_environment: production
  region_or_endpoint: us.example.provider
  inference_route: primary-agent-route
  fallback_route: fallback-agent-route-v1
  system_policy_reference: policy/sp-2026-03
  tool_policy_reference: tools/tp-2026-03
  behavioral_fingerprint_reference: bf-2026-03-15

candidate_model:
  provider: example-ai-provider
  model_family: example-secure-model
  model_name: example-secure-model-4
  model_version: 4.3.0
  deployment_environment: production
  region_or_endpoint: us.example.provider
  inference_route: primary-agent-route
  fallback_route: fallback-agent-route-v1
  system_policy_reference: policy/sp-2026-03
  tool_policy_reference: tools/tp-2026-03
  behavioral_fingerprint_reference: bf-2026-05-01

change_summary:
  change_type: model_version_update
  reason_for_change: provider security and reliability update
  materiality_decision: not_material
  materiality_basis: same provider and model family; no action-space or refusal-behavior delta observed on safety-critical tests
  affected_domains:
    - APTS-AR-019
    - APTS-TP-002
  requires_re_attestation: false
  customer_notification_required: false

behavioral_comparison:
  test_set_reference: safety-critical-baseline-2026-04
  baseline_run_id: eval-run-2026-04-30-a
  candidate_run_id: eval-run-2026-05-01-b
  scope_decision_delta: none_observed
  escalation_decision_delta: none_observed
  impact_classification_delta: minor_non_material_wording_change
  manipulation_resistance_delta: none_observed
  safety_control_delta: none_observed
  summary_result: passed
  evidence_references:
    - evidence/model-change/eval-run-2026-05-01-b.json
    - evidence/model-change/reviewer-notes-2026-05-01.md

drift_detection:
  drift_alert_id: null
  detected_at: null
  detection_source: scheduled_pre_engagement_baseline
  baseline_reference: bf-2026-03-15
  observed_deviation: no_threshold_exceedance
  affected_decision_paths: []
  tolerance_threshold: no_safety_critical_decision_changes
  threshold_exceeded: false
  operator_alerted_at: null
  blocked_or_limited_paths: []
  acknowledged_by: safety-reviewer-01
  acknowledged_at: 2026-05-01T14:20:00Z

human_review:
  review_required: true
  reviewer_name_or_role: safety-reviewer-01
  review_started_at: 2026-05-01T14:00:00Z
  review_completed_at: 2026-05-01T14:25:00Z
  review_decision: approved_for_deployment
  review_notes: Evaluation output showed no safety-critical decision changes beyond tolerance
  re_attestation_scope: not_required
  conformance_claim_updated: false
  foundation_model_disclosure_updated: false
  customer_notification_status: not_required

rollback:
  rollback_plan_reference: runbooks/model-rollback-v2.md
  rollback_test_run_id: rollback-test-2026-05-01
  rollback_test_result: passed
  previous_record_id: mcdr-2026-0038
  supersedes_record_id: mcdr-2026-0038
  superseded_by_record_id: null
  rollback_completed_at: null
```

## JSON-Equivalent Structure

```json
{
  "record_id": "mcdr-2026-0042",
  "record_type": "planned_model_change",
  "status": "approved_for_deployment",
  "previous_model": {
    "provider": "example-ai-provider",
    "model_name": "example-secure-model-4",
    "model_version": "4.2.1",
    "behavioral_fingerprint_reference": "bf-2026-03-15"
  },
  "candidate_model": {
    "provider": "example-ai-provider",
    "model_name": "example-secure-model-4",
    "model_version": "4.3.0",
    "behavioral_fingerprint_reference": "bf-2026-05-01"
  },
  "change_summary": {
    "change_type": "model_version_update",
    "materiality_decision": "not_material",
    "requires_re_attestation": false
  },
  "behavioral_comparison": {
    "test_set_reference": "safety-critical-baseline-2026-04",
    "summary_result": "passed"
  },
  "human_review": {
    "review_required": true,
    "review_decision": "approved_for_deployment"
  }
}
```

## Field Mapping to APTS Requirements

| Record area | Primary requirements |
| --- | --- |
| Pinned model identity and rollback data | `APTS-TP-002` |
| Behavioral fingerprint and comparison results | `APTS-AR-019`, `APTS-AR-017` |
| Provider-side drift detection and blocking decisions | `APTS-AR-019` |
| Foundation model disclosure updates | `APTS-TP-021`, `APTS-TP-022` |
| Re-attestation scope and customer notification | `APTS-TP-022`, `APTS-AR-018` |
| Human review of safety-critical behavior changes | `APTS-AR-019`, `APTS-MR-020` |

## Validation Guidance for Customers and Reviewers

When reviewing a model change and drift record, consider asking:

1. Does the record identify the previous and candidate model configuration specifically enough to reproduce or roll back the deployment?
2. Does the behavioral comparison cover scope validation, escalation triggers, finding classification, and manipulation-resistance paths?
3. If the change was marked not material, does the materiality basis map to the operator's APTS-TP-022 policy?
4. If drift was detected, were affected decision paths blocked or limited until review and acknowledgement?
5. Do evidence references point to actual baseline runs, candidate runs, drift alerts, and review notes?
6. If re-attestation was required, were the conformance claim and foundation model disclosure updated before customer engagements used the changed configuration?
7. Is the rollback path tested and linked to a prior model configuration?

## Usage Notes

This template is intentionally illustrative. Operators may keep equivalent records in change-management systems, model registries, ticketing systems, or governance platforms as long as the evidence is complete, reviewable, and available to customers when required by APTS.

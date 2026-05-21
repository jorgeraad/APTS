# Operator Competency Record Template

Informative Appendix (non-normative)

This appendix provides an illustrative template for documenting operator qualification, training, competency assessment, remediation, mentoring, and succession evidence for autonomous penetration testing platforms. It helps operators, customers, and reviewers inspect whether personnel assignments align with the competency governance described in APTS-HO-018. It does not create or modify APTS requirements.

## Purpose

APTS-HO-018 defines expectations for operator qualification, autonomy-level assignment, training curriculum, incident-response preparation, ongoing assessment, remediation, mentoring, and succession planning. A dedicated competency record helps organizations show how those expectations are tracked for each operator without relying on informal personnel notes.

This appendix shows:

- a compact field set for an operator competency record
- an example YAML record
- a JSON-equivalent structure
- reviewer questions for customers and security reviewers
- cross-references to related APTS requirements and appendices

## Primary Use Cases

Use an operator competency record when the platform needs to document:

- initial authorization of an operator for L1, L2, L3, or L4 duties
- annual competency reassessment and recertification status
- incident-response readiness for pause, redirect, kill-switch, escalation, and evidence-preservation procedures
- remediation after a failed assessment or operator-involved incident
- temporary restrictions on autonomy level, action class, or approval authority
- mentoring and succession readiness for operational continuity

## Design Principles

An operator competency record should:

- identify the operator, role, and active authorization status unambiguously
- separate general qualifications from platform-specific training evidence
- map competency evidence to the autonomy levels and approval duties the operator may perform
- record assessment outcomes and remediation restrictions in a way reviewers can audit later
- connect operator authorization to the Authority Delegation Matrix and shift-handoff process
- avoid storing sensitive personal details beyond what is needed to verify competency and authorization

## Recommended Template Sections

### 1. Record metadata

Use stable identifiers so the record can be correlated with personnel systems, approval logs, and the engagement audit trail.

Recommended fields:

- `competency_record_id`
- `operator_id`
- `operator_role`
- `record_version`
- `status`
- `created_at`
- `last_updated_at`
- `record_owner`

Suggested status values:

- `active`
- `restricted`
- `suspended_pending_remediation`
- `inactive`

### 2. Authorization scope

Document what the operator is currently authorized to do and where that authority is defined.

Recommended fields:

- `authorized_autonomy_levels`
- `maximum_autonomy_level`
- `authorized_action_classes`
- `approval_authority_roles`
- `authority_delegation_matrix_ref`
- `shift_handoff_eligible`
- `emergency_authority_roles`
- `restrictions`

### 3. Qualification evidence

Capture qualifications that support the assignment without turning the template into a general HR file.

Recommended fields:

- `professional_qualifications`
- `platform_specific_qualifications`
- `experience_summary`
- `certification_expiration_dates`
- `qualification_evidence_refs`

### 4. Training curriculum completion

Record required training modules and the evidence that they were completed.

Recommended fields:

- `module_id`
- `module_name`
- `required_for_levels`
- `completion_status`
- `completed_at`
- `evidence_ref`
- `instructor_or_evaluator`

Suggested module families:

- APTS orientation and compliance tiers
- scope interpretation and Rules of Engagement handling
- approval gates and authority delegation
- pause, redirect, and kill-switch operation
- incident escalation and evidence preservation
- sensitive data and credential handling
- manipulation-resistance and prompt-injection response
- shift handoff and operator fatigue mitigation

### 5. Incident-response readiness

Document the exercises that demonstrate the operator can respond to autonomous testing failures and emergency situations.

Recommended fields:

- `exercise_id`
- `scenario`
- `skills_tested`
- `result`
- `observed_response_time`
- `evidence_ref`
- `follow_up_required`

Example exercise scenarios:

- emergency pause and state preservation
- kill-switch activation and state dump review
- unexpected out-of-scope target access
- legal or compliance escalation trigger
- credential exposure during testing
- failed handoff with stale approvals

### 6. Competency assessment and remediation

Record assessment outcomes, restrictions, and conditions for restoring authority.

Recommended fields:

- `assessment_id`
- `assessment_type`
- `assessment_date`
- `assessor_role`
- `result`
- `gaps_identified`
- `required_remediation`
- `temporary_restrictions`
- `reassessment_due_at`
- `authority_restored_at`

Suggested result values:

- `passed`
- `passed_with_conditions`
- `failed_restricted`
- `failed_suspended`

### 7. Mentoring and succession readiness

Document operational continuity support without silently granting approval authority outside the Authority Delegation Matrix.

Recommended fields:

- `mentor_role`
- `mentoring_plan_ref`
- `backup_operator_role`
- `succession_plan_ref`
- `readiness_status`
- `handoff_practice_refs`

## Example YAML Template

```yaml
competency_record_id: ocr-2026-0042
operator_id: operator-17
operator_role: senior-autonomous-testing-operator
record_version: "1.0"
status: active
created_at: "2026-05-01T10:00:00Z"
last_updated_at: "2026-05-15T16:30:00Z"
record_owner: security-operations-training-lead

authorization_scope:
  authorized_autonomy_levels:
    - L1 Assisted
    - L2 Supervised
    - L3 Semi-Autonomous
  maximum_autonomy_level: L3 Semi-Autonomous
  authorized_action_classes:
    - exploitation-approval-cvss-7-to-8-9
    - scope-uncertainty-escalation-review
    - emergency-pause
    - shift-handoff-primary
  approval_authority_roles:
    - ho-role-senior-operator
  authority_delegation_matrix_ref: adm-2026-001#ho-role-senior-operator
  shift_handoff_eligible: true
  emergency_authority_roles:
    - emergency-pause-operator
    - secondary-kill-switch-operator
  restrictions: []

qualification_evidence:
  professional_qualifications:
    - OSCP
    - 6 years offensive security experience
  platform_specific_qualifications:
    - cloak-autonomous-platform-operator-l3
  experience_summary: Led supervised and semi-autonomous web application assessments
  certification_expiration_dates:
    OSCP: "2028-04-30"
    cloak-autonomous-platform-operator-l3: "2027-05-15"
  qualification_evidence_refs:
    - personnel-training-system#cert-8821
    - personnel-training-system#platform-cert-4462

training_curriculum:
  - module_id: apt-roe-101
    module_name: Scope interpretation and Rules of Engagement handling
    required_for_levels:
      - L1 Assisted
      - L2 Supervised
      - L3 Semi-Autonomous
    completion_status: completed
    completed_at: "2026-05-02T14:00:00Z"
    evidence_ref: lms#completion-771
    instructor_or_evaluator: training-lead
  - module_id: apt-ir-301
    module_name: Incident escalation and evidence preservation
    required_for_levels:
      - L3 Semi-Autonomous
      - L4 Autonomous
    completion_status: completed
    completed_at: "2026-05-08T18:00:00Z"
    evidence_ref: lms#completion-779
    instructor_or_evaluator: incident-response-lead

incident_response_readiness:
  - exercise_id: drill-2026-015
    scenario: emergency pause and state preservation during suspected scope drift
    skills_tested:
      - pause-activation
      - escalation-routing
      - state-preservation
      - evidence-handoff
    result: passed
    observed_response_time: PT2M10S
    evidence_ref: incident-drill-record-2026-015
    follow_up_required: none

competency_assessments:
  - assessment_id: assess-2026-044
    assessment_type: annual-practical-assessment
    assessment_date: "2026-05-15"
    assessor_role: autonomous-testing-program-lead
    result: passed
    gaps_identified: []
    required_remediation: []
    temporary_restrictions: []
    reassessment_due_at: "2027-05-15"
    authority_restored_at: null

mentoring_and_succession:
  mentor_role: principal-autonomous-testing-operator
  mentoring_plan_ref: mentoring-plan-2026-operator-17
  backup_operator_role: senior-autonomous-testing-operator-backup
  succession_plan_ref: succession-plan-ho-2026
  readiness_status: ready-for-primary-shift-duty
  handoff_practice_refs:
    - shift-handoff-record-2026-033
```

## JSON-Equivalent Structure

```json
{
  "competency_record_id": "ocr-2026-0042",
  "operator_id": "operator-17",
  "operator_role": "senior-autonomous-testing-operator",
  "record_version": "1.0",
  "status": "active",
  "authorization_scope": {
    "authorized_autonomy_levels": ["L1 Assisted", "L2 Supervised", "L3 Semi-Autonomous"],
    "maximum_autonomy_level": "L3 Semi-Autonomous",
    "authorized_action_classes": ["exploitation-approval-cvss-7-to-8-9", "scope-uncertainty-escalation-review", "emergency-pause", "shift-handoff-primary"],
    "authority_delegation_matrix_ref": "adm-2026-001#ho-role-senior-operator",
    "shift_handoff_eligible": true,
    "restrictions": []
  },
  "qualification_evidence": {
    "professional_qualifications": ["OSCP", "6 years offensive security experience"],
    "platform_specific_qualifications": ["cloak-autonomous-platform-operator-l3"],
    "qualification_evidence_refs": ["personnel-training-system#cert-8821", "personnel-training-system#platform-cert-4462"]
  },
  "training_curriculum": [
    {
      "module_id": "apt-roe-101",
      "module_name": "Scope interpretation and Rules of Engagement handling",
      "completion_status": "completed",
      "completed_at": "2026-05-02T14:00:00Z",
      "evidence_ref": "lms#completion-771"
    }
  ],
  "competency_assessments": [
    {
      "assessment_id": "assess-2026-044",
      "assessment_type": "annual-practical-assessment",
      "assessment_date": "2026-05-15",
      "result": "passed",
      "reassessment_due_at": "2027-05-15"
    }
  ]
}
```

## Reviewer Questions

When inspecting an operator competency record, ask:

- does the record show the operator was authorized for the autonomy level they operated at
- does the Authority Delegation Matrix reference support any approval or emergency authority the operator used
- were required training modules completed before the operator was assigned to that level or shift
- does incident-response training include pause, redirect, kill-switch, state-preservation, and escalation exercises
- are certifications and platform-specific qualifications current rather than expired
- if an assessment failed, were temporary restrictions applied before the operator resumed duties
- can mentoring, backup, and succession references be cross-checked against shift-handoff and continuity records
- does the record avoid unnecessary personal data while preserving enough evidence for review

## Relationship to Existing APTS Artifacts

This template complements, but does not replace:

- [Authority Delegation Matrix Template](Authority_Delegation_Matrix_Template.md) for role-based approval, escalation, and emergency authority
- [Shift Handoff Template](Shift_Handoff_Template.md) for active engagement continuity and incoming-operator acceptance
- [Incident Response Drill Record Template](Incident_Response_Drill_Record_Template.md) for detailed tabletop or live-drill evidence
- [Human Review Record Template](Human_Review_Record_Template.md) for finding-level human review decisions

## Related Requirements

This template can help collect evidence for:

- APTS-HO-018: Operator Qualification, Training, and Competency Governance
- APTS-HO-004: Authority Delegation Matrix
- APTS-HO-005: Delegation Chain-of-Custody and Decision Audit Trail
- APTS-HO-009: Multi-Operator Kill Switch Authority and Handoff
- APTS-HO-019: 24/7 Operational Continuity and Shift Handoff
- APTS-AL-025: Autonomy Level Authorization, Transition, and Reauthorization

# Evidence-as-Code Schema

> Draft schema for EvidenceSnap evidence-as-code configuration.

EvidenceSnap should begin as a human-friendly evidence tracking platform, but its long-term differentiation is evidence-as-code.

Evidence-as-code means evidence requirements can be represented as structured, versionable YAML or JSON. Users should be able to define, import, export, version, validate, and eventually store evidence configuration in Git.

This document defines an initial schema direction for EvidenceSnap.

---

<a id="table-of-contents"></a>

<details open>
<summary><strong>📚 Table of Contents</strong></summary>

- [1. Purpose](#purpose)
- [2. Design Principles](#design-principles)
- [3. Schema Scope](#schema-scope)
- [4. MVP Schema Objects](#mvp-schema-objects)
- [5. Evidence Requirement Schema](#evidence-requirement-schema)
- [6. Evidence Template Pack Schema](#evidence-template-pack-schema)
- [7. Validation Rules](#validation-rules)
- [8. Example Evidence Requirement](#example-evidence-requirement)
- [9. Example Template Pack](#example-template-pack)
- [10. JSON Schema Draft](#json-schema-draft)
- [11. Mapping to Application Data Model](#mapping-to-application-data-model)
- [12. Future OSCAL Mapping Notes](#future-oscal-mapping-notes)
- [13. Versioning Strategy](#versioning-strategy)
- [14. Import/Export Workflow](#importexport-workflow)
- [15. Open Questions](#open-questions)

</details>

---

<a id="purpose"></a>

## 1. Purpose

The evidence-as-code schema should allow EvidenceSnap to represent compliance evidence requirements in a structured, portable format.

The schema should support:

- Evidence requirement templates
- Framework and control references
- System references
- Evidence ownership
- Review expectations
- Frequency and due dates
- Accepted evidence types
- Validation rules
- Tags and metadata
- Future versioning
- Future Git-backed compliance repositories

The schema should not attempt to fully model all GRC concepts in the MVP.

[Back to top](#table-of-contents)

---

<a id="design-principles"></a>

## 2. Design Principles

### Keep the Schema Human-Readable

YAML should be easy for a technical compliance user to read.

### Keep the Schema UI-Friendly

Users should not be required to write YAML manually. The UI should be able to generate and edit these objects.

### Keep the Schema Versionable

Evidence requirements should be stored in a way that works well with Git diffs.

### Keep the Schema MVP-Sized

Avoid full OSCAL complexity until the product workflow is validated.

### Preserve Future OSCAL Compatibility

The schema should not block future mapping to OSCAL catalogs, profiles, SSPs, assessment results, or back-matter resources.

[Back to top](#table-of-contents)

---

<a id="schema-scope"></a>

## 3. Schema Scope

### In Scope for MVP / Early Post-MVP

- Evidence Requirement definitions
- Template Pack metadata
- Framework references
- Control references
- System references
- Owner/reviewer role hints
- Evidence type expectations
- Frequency and due rules
- Basic validation rules
- Tags and metadata

### Out of Scope for Initial Schema

- Full risk register
- Full policy management
- Vendor risk management
- Full OSCAL SSP generation
- Automated compliance scoring
- Deep scanner integration definitions
- Complex inheritance/common control modeling
- Full assessment results modeling

[Back to top](#table-of-contents)

---

<a id="mvp-schema-objects"></a>

## 4. MVP Schema Objects

The first evidence-as-code implementation should support two object types:

1. `EvidenceRequirement`
2. `EvidenceTemplatePack`

Optional future object types:

- `FrameworkDefinition`
- `ControlDefinition`
- `SystemDefinition`
- `EvidencePeriodDefinition`
- `ValidationRuleSet`
- `ComplianceRepositoryManifest`

[Back to top](#table-of-contents)

---

<a id="evidence-requirement-schema"></a>

## 5. Evidence Requirement Schema

### Required Fields

| Field | Type | Description |
|---|---|---|
| `id` | string | Stable unique identifier |
| `name` | string | Human-readable requirement name |
| `framework` | string | Framework slug/reference |
| `control_code` | string | Control code/reference |
| `system` | string | System slug/reference |
| `frequency` | string | Collection frequency |
| `evidence_type` | array | Accepted evidence types |

### Recommended Fields

| Field | Type | Description |
|---|---|---|
| `description` | string | Detailed instructions |
| `owner_role` | string | Suggested owner role |
| `reviewer_role` | string | Suggested reviewer role |
| `due` | object | Due date rules |
| `validation` | object | Validation rules |
| `approval` | object | Review/approval requirements |
| `tags` | array | Search/filter tags |
| `version` | string | Requirement version |
| `active` | boolean | Whether the template is active |

### Field Reference

#### `id`

Stable identifier.

Recommended pattern:

```text
evidence.{domain}.{purpose}.{frequency}
```

Example:

```text
evidence.identity.mfa-status.monthly
```

#### `framework`

Framework slug.

Examples:

```text
cmmc-level-2
nist-800-171
soc2-security
custom
```

#### `control_code`

Control reference.

Examples:

```text
IA.L2-3.5.3
AC-2
CM-8
RA-5
```

#### `system`

System slug or template placeholder.

Examples:

```text
corporate-it
aws-production
microsoft-365
google-workspace
```

#### `frequency`

Allowed values for MVP:

```text
one_time
monthly
quarterly
semiannual
annual
per_change
per_incident
```

#### `evidence_type`

Allowed values for MVP:

```text
screenshot
pdf
csv
document
external_link
attestation
ticket_export
vulnerability_scan
access_review
inventory_export
backup_report
incident_log
change_log
```

[Back to top](#table-of-contents)

---

<a id="evidence-template-pack-schema"></a>

## 6. Evidence Template Pack Schema

An Evidence Template Pack is a collection of Evidence Requirements designed for a framework, use case, or customer type.

### Suggested Fields

| Field | Type | Description |
|---|---|---|
| `pack_id` | string | Stable pack identifier |
| `name` | string | Pack name |
| `description` | string | Pack description |
| `version` | string | Pack version |
| `framework` | string | Primary framework |
| `author` | string | Author or maintainer |
| `requirements` | array | Evidence Requirement objects |
| `tags` | array | Pack tags |

### Template Pack Examples

- `generic-monthly-evidence`
- `cmmc-level-1-starter`
- `cmmc-level-2-starter`
- `nist-800-171-starter`
- `soc2-security-starter`
- `fedramp-conmon-lite`

[Back to top](#table-of-contents)

---

<a id="validation-rules"></a>

## 7. Validation Rules

Validation rules should help identify obviously incomplete, stale, or mismatched evidence.

Validation should not claim to determine compliance.

### MVP Validation Rules

| Rule | Purpose |
|---|---|
| `required_file_types` | Restrict accepted evidence file types |
| `max_age_days` | Flag stale evidence |
| `required_metadata` | Require fields like evidence date/source system |
| `required_fields` | For structured CSV-like evidence |
| `allow_external_url` | Allow link-based evidence |
| `requires_reviewer_approval` | Require review workflow |

### Example

```yaml
validation:
  max_age_days: 35
  required_file_types:
    - csv
    - pdf
  required_metadata:
    - evidence_date
    - source_system
  required_fields:
    - user
    - mfa_status
    - account_status
```

### Important Limitation

Validation rules should produce findings like:

- Evidence appears stale.
- Expected CSV or PDF.
- Missing evidence date.
- Missing source system.
- Uploaded file does not include expected metadata.

Validation rules should not produce unsupported claims like:

- This control is compliant.
- This evidence guarantees audit success.
- This requirement is certified complete.

[Back to top](#table-of-contents)

---

<a id="example-evidence-requirement"></a>

## 8. Example Evidence Requirement

```yaml
id: evidence.identity.mfa-status.monthly
version: "1.0"
active: true

framework: cmmc-level-2
control_code: IA.L2-3.5.3
system: corporate-it

name: Monthly MFA Status Export
description: >
  Export the current user MFA status report from the identity provider.
  Evidence should show active users, account status, and MFA status.

frequency: monthly

owner_role: IT Administrator
reviewer_role: Compliance Manager

due:
  day_of_month: 5
  grace_days: 3

evidence_type:
  - csv
  - pdf

validation:
  max_age_days: 35
  required_file_types:
    - csv
    - pdf
  required_metadata:
    - evidence_date
    - source_system
  required_fields:
    - user
    - mfa_status
    - account_status

approval:
  required: true

tags:
  - identity
  - access-control
  - monthly
  - mfa
```

[Back to top](#table-of-contents)

---

<a id="example-template-pack"></a>

## 9. Example Template Pack

```yaml
pack_id: generic-monthly-evidence
name: Generic Monthly Evidence Pack
version: "0.1.0"
framework: custom
author: EvidenceSnap
description: >
  Starter evidence requirements for small teams that need monthly
  compliance evidence collection.

tags:
  - starter
  - monthly
  - generic

requirements:
  - id: evidence.identity.user-access-review.monthly
    version: "1.0"
    active: true
    framework: custom
    control_code: AC-USER-REVIEW
    system: corporate-it
    name: Monthly User Access Review
    description: Review active users and privileged access for the system.
    frequency: monthly
    owner_role: IT Administrator
    reviewer_role: Compliance Manager
    due:
      day_of_month: 5
      grace_days: 3
    evidence_type:
      - csv
      - pdf
      - attestation
    validation:
      max_age_days: 35
      required_metadata:
        - evidence_date
        - source_system
    approval:
      required: true
    tags:
      - identity
      - access-review
      - monthly

  - id: evidence.vulnerability.scan-summary.monthly
    version: "1.0"
    active: true
    framework: custom
    control_code: RA-VULN-SCAN
    system: corporate-it
    name: Monthly Vulnerability Scan Summary
    description: Upload the latest vulnerability scan summary report.
    frequency: monthly
    owner_role: Security Analyst
    reviewer_role: Compliance Manager
    due:
      day_of_month: 7
      grace_days: 3
    evidence_type:
      - pdf
      - vulnerability_scan
    validation:
      max_age_days: 35
      required_metadata:
        - evidence_date
        - source_system
    approval:
      required: true
    tags:
      - vulnerability-management
      - monthly
```

[Back to top](#table-of-contents)

---

<a id="json-schema-draft"></a>

## 10. JSON Schema Draft

This is a draft JSON Schema for an individual Evidence Requirement.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "EvidenceSnap Evidence Requirement",
  "type": "object",
  "required": [
    "id",
    "framework",
    "control_code",
    "system",
    "name",
    "frequency",
    "evidence_type"
  ],
  "properties": {
    "id": {
      "type": "string",
      "description": "Stable evidence requirement identifier."
    },
    "version": {
      "type": "string",
      "default": "1.0"
    },
    "active": {
      "type": "boolean",
      "default": true
    },
    "framework": {
      "type": "string"
    },
    "control_code": {
      "type": "string"
    },
    "system": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "description": {
      "type": "string"
    },
    "frequency": {
      "type": "string",
      "enum": [
        "one_time",
        "monthly",
        "quarterly",
        "semiannual",
        "annual",
        "per_change",
        "per_incident"
      ]
    },
    "owner_role": {
      "type": "string"
    },
    "reviewer_role": {
      "type": "string"
    },
    "due": {
      "type": "object",
      "properties": {
        "day_of_month": {
          "type": "integer",
          "minimum": 1,
          "maximum": 31
        },
        "grace_days": {
          "type": "integer",
          "minimum": 0
        }
      }
    },
    "evidence_type": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "screenshot",
          "pdf",
          "csv",
          "document",
          "external_link",
          "attestation",
          "ticket_export",
          "vulnerability_scan",
          "access_review",
          "inventory_export",
          "backup_report",
          "incident_log",
          "change_log"
        ]
      }
    },
    "validation": {
      "type": "object",
      "properties": {
        "max_age_days": {
          "type": "integer",
          "minimum": 0
        },
        "required_file_types": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "required_metadata": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "required_fields": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      }
    },
    "approval": {
      "type": "object",
      "properties": {
        "required": {
          "type": "boolean"
        }
      }
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      }
    }
  }
}
```

[Back to top](#table-of-contents)

---

<a id="mapping-to-application-data-model"></a>

## 11. Mapping to Application Data Model

| Schema Field | Application Entity | Application Field |
|---|---|---|
| `id` | Evidence Requirement | External/template ID |
| `version` | Evidence Requirement | Version |
| `active` | Evidence Requirement | Active |
| `framework` | Framework | Slug/name |
| `control_code` | Control | Code |
| `system` | System | Slug/name |
| `name` | Evidence Requirement | Name |
| `description` | Evidence Requirement | Description |
| `frequency` | Evidence Requirement | Frequency |
| `owner_role` | Evidence Requirement | Owner role hint |
| `reviewer_role` | Evidence Requirement | Reviewer role hint |
| `due.day_of_month` | Evidence Requirement | Due day |
| `due.grace_days` | Evidence Requirement | Grace days |
| `evidence_type` | Evidence Requirement | Evidence type |
| `validation` | Evidence Requirement | Validation rules JSON |
| `approval.required` | Evidence Requirement | Review required |
| `tags` | Evidence Requirement | Tags |

[Back to top](#table-of-contents)

---

<a id="future-oscal-mapping-notes"></a>

## 12. Future OSCAL Mapping Notes

OSCAL support is desirable later but should not block MVP.

Potential future mappings:

| EvidenceSnap Concept | OSCAL-like Concept |
|---|---|
| Framework | Catalog / Profile |
| Control | Control |
| System | System Security Plan system |
| Evidence Requirement | Assessment objective / custom property |
| Evidence Item | Assessment result observation / back-matter resource |
| Review Decision | Assessment result / finding status |
| Implementation Statement | SSP control implementation |
| Export Package | OSCAL JSON/YAML package |

### Guidance

- Start with simple EvidenceSnap-native schemas.
- Avoid promising full OSCAL compatibility early.
- Preserve enough structure to support future OSCAL import/export.
- Add OSCAL mapping after real evidence workflows are validated.

[Back to top](#table-of-contents)

---

<a id="versioning-strategy"></a>

## 13. Versioning Strategy

Evidence Requirement templates should be versioned.

### Recommended Versioning

Use semantic-style versions:

```text
1.0.0
1.1.0
2.0.0
```

### Versioning Rules

- Minor text changes may increment patch version.
- Validation changes should increment minor version.
- Major workflow changes should increment major version.
- Historical Evidence Requests should preserve the requirement version used when generated.
- Do not silently overwrite historical evidence requirements.

### Future Requirement Version Table

Future versions may use a dedicated table:

```text
evidence_requirement_versions
```

Potential fields:

- `id`
- `org_id`
- `evidence_requirement_id`
- `version`
- `definition_json`
- `created_by_user_id`
- `created_at`

[Back to top](#table-of-contents)

---

<a id="importexport-workflow"></a>

## 14. Import/Export Workflow

### Import Flow

```text
Upload YAML/JSON
        ↓
Parse file
        ↓
Validate schema
        ↓
Preview changes
        ↓
Map framework/control/system references
        ↓
Create or update Evidence Requirements
        ↓
Record audit log
```

### Export Flow

```text
Select Evidence Requirements
        ↓
Generate YAML/JSON
        ↓
Include version metadata
        ↓
Download or commit to Git
        ↓
Record audit log
```

### Import Safety

Before applying imports, the app should show:

- New requirements
- Updated requirements
- Deactivated requirements
- Missing framework references
- Missing control references
- Missing system references
- Validation warnings

[Back to top](#table-of-contents)

---

<a id="open-questions"></a>

## 15. Open Questions

- Should evidence-as-code live inside this app repo, a separate template repo, or both?
- Should template pack schemas include full control definitions or only control references?
- Should imported templates be editable after import?
- Should local customer changes fork the template version?
- How should GitHub sync work?
- Should validation rules be app-specific YAML or eventually support policy engines like OPA/Rego?
- Should every Evidence Requirement have an immutable external template ID?
- How much OSCAL alignment should be designed before MVP launch?

[Back to top](#table-of-contents)
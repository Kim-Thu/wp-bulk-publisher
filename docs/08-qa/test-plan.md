# QA Test Plan

## Objective
Verify that WP Bulk Publisher transforms heterogeneous sources predictably and performs remote WordPress mutations safely, with enough evidence to diagnose and retry failures.

## Test levels

### Unit
Import row mapping, term parsing, sanitizer/rules, slug generation, filename strategy, validators, link transforms and WordPress payload construction.

### Integration
DOCX conversion, XLSX parsing, local persistence, WordPress client with mocked HTTP responses, media pipeline and batch orchestration.

### End-to-end
Import -> normalize -> review -> destination -> publish against a controlled WordPress test site.

### Exploratory
Malformed HTML, unusual encodings, large articles, duplicate titles/slugs, broken image URLs, authentication expiration, rate limiting, server errors and mid-batch network loss.

## Critical scenarios

| ID | Scenario | Expected |
| --- | --- | --- |
| QA-001 | CSV with 100 valid rows | 100 canonical articles, no data shifted between rows |
| QA-002 | Excel with multiple sheets | User can select intended sheet |
| QA-003 | DOCX with headings/list/table/images | Semantic content retained within supported policy |
| QA-004 | Paste HTML containing script/onClick/style | Unsafe code removed; content remains reviewable |
| QA-005 | Reapply same preset | No accumulating/destructive transformation |
| QA-006 | Missing title | Blocking validation error |
| QA-007 | Broken external image | Clear warning/error according to preset policy |
| QA-008 | Existing category/tag | Existing destination ID reused |
| QA-009 | Missing category/tag with create enabled | Term created once and ID used |
| QA-010 | Image upload | Filename policy applied and final HTML points to WordPress URL |
| QA-011 | Wrong credentials | Connection/publish fails without exposing secret |
| QA-012 | One article fails in batch | Other eligible articles continue; failed item identifiable |
| QA-013 | Retry failures | Previously successful items are not recreated |
| QA-014 | Publish confirmation | Destination site and selected count are visible before mutation |
| QA-015 | API timeout after possible post creation | System does not blindly duplicate without reconciliation |

## Security tests
- XSS payloads in pasted HTML, DOCX-generated HTML and spreadsheet fields.
- `javascript:`/unexpected URL schemes.
- Credential redaction in logs/errors.
- Renderer cannot access unrestricted filesystem/Node APIs.
- Preview cannot invoke privileged IPC through injected markup.

## Performance targets
- Import/normalize 500 normal text articles without UI becoming unusable.
- Queue/filter interactions remain responsive.
- Publishing uses bounded concurrency.

## Exit criteria for MVP
- All Must requirements have acceptance tests.
- No open critical/high security defect.
- No known data-loss defect in import/normalization.
- Retry behavior verified against partial batch failure.
- Publish tested against at least one controlled WordPress installation.
- Release checklist completed.
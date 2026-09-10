# Business Analysis

## 1. Current workflow

A typical migration/publishing task currently requires the operator to:

1. Receive articles from CSV/Excel/DOCX or copy them from another site.
2. Identify which fields represent title, body, slug, category, tags and images.
3. Remove source-site formatting and unsafe/unwanted HTML.
4. Correct headings, links and image attributes.
5. Download source images.
6. Rename images according to destination convention.
7. Upload images to WordPress.
8. Replace old image URLs.
9. Find or create categories/tags.
10. Select the correct destination site.
11. Create the post.
12. Preview/check the result.
13. Repeat for every article.

The cost is not only typing time. The operator must repeatedly remember destination-specific rules, which creates inconsistency and makes large migrations difficult to verify.

## 2. Target workflow

Import -> Map -> Normalize -> Validate -> Review exceptions -> Select destination/preset -> Publish -> Verify -> Retry failures.

## 3. Actors

### Content Operator
Imports and reviews articles, selects presets/sites, resolves validation errors and starts publishing.

### Administrator
Creates destination site profiles, credentials and reusable publishing presets.

### WordPress REST API
Receives media, taxonomy and post mutations and returns canonical IDs/URLs/errors.

## 4. Business rules

- BR-01: Source format must not change the normalized article contract.
- BR-02: No article with blocking validation errors can be published unless an explicit future override policy is introduced.
- BR-03: A batch may contain valid and invalid items; invalid items must not prevent valid items from being reviewed.
- BR-04: Publishing is scoped to the site explicitly selected by the user.
- BR-05: Category/tag names must resolve to destination IDs before post creation.
- BR-06: Uploaded image URLs must replace source URLs in final content.
- BR-07: Image filenames follow the active preset naming strategy.
- BR-08: External credentials must never be exported with article data or presets.
- BR-09: Batch publishing must report each article independently.
- BR-10: Retry must target failed work without republishing successful articles by default.
- BR-11: Cleaning rules must be deterministic and previewable.
- BR-12: The original imported content remains recoverable during the working session so a user can compare or revert normalization.

## 5. Functional areas

### Source ingestion
CSV, Excel, DOCX, pasted HTML; field mapping; encoding/error detection.

### Normalization engine
HTML sanitization, element/attribute transforms, heading rules, link transforms, image policies, slug policy and destination presets.

### Review workspace
Queue, validation state, before/after comparison, article editor, warnings and bulk actions.

### WordPress destination management
Multiple site profiles, connection test, authentication, taxonomy lookup/create and capability/error feedback.

### Media pipeline
Discover image sources, fetch/decode, determine type, rename, upload, set alt text, replace URLs and optionally assign featured image.

### Publishing
Draft/publish, progress, item result, WordPress post link, error details and retry.

## 6. Risks and controls

| Risk | Control |
| --- | --- |
| Wrong site selected | Persistent destination indicator + confirmation summary before batch publish |
| Destructive HTML cleaning | Keep source snapshot + before/after preview |
| Duplicate posts on retry | Persist remote post ID/result for successful items; retry failed items only |
| Image hotlink remains | Media validation reports unresolved external image sources |
| Category/tag mismatch | Resolve destination taxonomy before post creation |
| Partial batch failure | Item-level state machine and result log |
| Credentials exposed | OS-protected credential storage; never log password |
| Source HTML contains scripts/events | Allow-list sanitization before preview/publish |

## 7. Acceptance-level outcomes

A successful batch is not merely one where the API returned HTTP 201. The operator must be able to determine which articles were published, where they were published, what failed, why it failed and what can safely be retried.
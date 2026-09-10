# Product Vision

## Problem
Content operators frequently receive articles from spreadsheets, DOCX files and existing websites. Publishing them manually to several WordPress sites requires repetitive HTML cleanup, image handling, taxonomy assignment, link replacement and final checking. The work is slow and inconsistent, while a small mistake can publish broken content to production.

## Vision
WP Bulk Publisher is a desktop content operations workspace that converts heterogeneous article sources into a predictable, reviewable and repeatable WordPress publishing pipeline.

## Primary user
A content administrator, web administrator or developer responsible for migrating or publishing many articles across one or more WordPress websites.

## Core outcome
A user can import a batch, apply reusable rules, see exactly what will change, fix exceptions, choose a destination site and publish safely without manually rebuilding each post in wp-admin.

## Principles
1. Preview before mutation.
2. Batch automation with per-item control.
3. Never silently discard content.
4. Destination-specific rules belong to presets, not source data.
5. A failed article must not block unrelated articles.
6. Every publish attempt must produce an understandable result.
7. Credentials must not be stored as plain text.

## MVP success criteria
- Supported sources: CSV, Excel, DOCX and pasted HTML.
- One normalized internal article model regardless of source.
- Reusable normalization presets.
- Validation before publishing.
- Image rename/upload/relink workflow.
- Category/tag resolution.
- Multiple WordPress site profiles.
- Draft and publish modes.
- Per-article success/failure status and retry.

## Out of scope for MVP
- WordPress page-builder reconstruction.
- Gutenberg block-perfect migration from arbitrary websites.
- AI rewriting of article meaning.
- WordPress plugin installation/administration.
- Multi-user cloud collaboration.
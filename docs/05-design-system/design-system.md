# Design System v0.1

## Direction
A compact desktop operations UI optimized for scanning batches and resolving exceptions. Neutral surfaces keep article content visually dominant. Status colors communicate system state, not decoration.

## Foundations

### Spacing
4px base grid: 4, 8, 12, 16, 24, 32, 40.

### Radius
- Controls/cards: 8px
- Small status chips: 6px
- Modal/dialog: 12px

### Typography
System UI stack for application chrome; monospace stack for raw HTML, IDs and logs.

Suggested scale:
- 12: metadata/helper
- 14: default controls/body
- 16: emphasized body/subheading
- 20: screen title
- 24: major empty-state/title only

### Color tokens
Semantic tokens rather than hardcoded feature colors:
- `surface.canvas`
- `surface.panel`
- `surface.raised`
- `text.primary`
- `text.secondary`
- `border.default`
- `action.primary`
- `status.success`
- `status.warning`
- `status.danger`
- `status.info`

Exact palette is selected during visual design and must meet WCAG contrast requirements.

## Components

### Button
Variants: primary, secondary, ghost, danger. States: default, hover, focus-visible, disabled, loading.

### Field
Text input, textarea, select, searchable select, tag/category token input. Error/help text is structurally attached to the field.

### StatusBadge
Text + optional icon. Never color-only.

### ArticleQueueItem
Checkbox, title, source, validation count, state and dirty indicator. Designed for high-density scanning.

### ValidationMessage
Severity, concise problem, affected field/rule and recovery action.

### DestinationBadge
Site name + environment/URL hint. Persistent in publishing contexts to prevent wrong-site mistakes.

### RuleRow
Enable switch, rule name, concise behavior, configuration, example/test action.

### DiffViewer
Source vs normalized HTML/content with additions/removals and a preview mode.

### ProgressItem
Article, current pipeline stage, result and retry/open-remote actions.

### Modal
Reserved for short, blocking decisions. Full workflows use screens/drawers rather than nested dialogs.

## Accessibility
- Keyboard navigation for queue and primary actions.
- Visible focus state.
- Minimum target size appropriate for desktop pointer/keyboard use.
- Labels remain visible; placeholders do not replace labels.
- Status is never represented by color alone.
- Preview content is separated from app controls semantically.

## Content language
Use operational verbs: Import, Apply preset, Validate, Publish, Retry. Error copy says what failed and what the user can do next. Avoid generic messages such as “Something went wrong” when API/import context is available.
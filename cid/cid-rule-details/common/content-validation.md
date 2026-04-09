# Content Validation

Before creating any documentation file, validate content according to these rules.

## Diagram Validation

- Validate Mermaid diagram syntax before writing to file
- Validate ASCII art diagrams for alignment and readability
- Escape special characters in markdown properly
- Provide text alternatives for complex visual content

## Gherkin Validation

- Every Gherkin feature must have a valid `Feature:` header
- Every scenario must have at least one `When` and one `Then`
- `Background` sections must only contain `Given` steps
- Scenario names must be unique within a feature
- Steps must be specific enough to be directly translatable to automated tests

## VOS Document Validation

Before a VOS enters `.vos/todo/`:
- All five required sections (WHY, WHAT, HOW, CONTEXT, OUTCOME) are present
- Intent statement is one paragraph maximum
- Acceptance contract has valid Gherkin syntax
- Task plan has numbered tasks with checkboxes
- Context bundle references existing files
- Outcome hypothesis has a measurable metric
- VOS number matches the `.last` file

## File Naming

- VOS files: `<number>_<type>_<domain>_<intent-dashed>.md`
- Documentation files: lowercase, hyphenated
- No spaces in filenames

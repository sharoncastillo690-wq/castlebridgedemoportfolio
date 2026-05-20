# Parta Course Builder

Build a Parta course from a pasted course document using the Parta MCP.

## Environment notes
- Style guide: `~/Documents/Claude Stuff/CastleBridgeCX Resources/CBX_Writing_Style_Guide.md`

---

## Step 1 — Load style references

Before doing anything else, read all reference files above in full. All content written in subsequent steps must comply with the referenced files. Do not skip this step.

---

## Step 2 — Parse the pasted document

Extract the following from the user's document:

| Field | Notes |
|-------|-------|
| Course title | Required |
| Learning objectives | What the learner will be able to do after the course |
| Content sections / modules | Title + body content for each |

---

## Step 3 — Validate completeness BEFORE touching Parta

Review every extracted field. Flag each one that is:
- Missing entirely
- Too vague to write to without guessing
- Internally contradictory

**Hard rule**: Ask the user to resolve every flagged gap before proceeding. Present all gaps at once in a single message so the user can answer them together. Do not proceed to Step 4 until every gap is resolved.

**Hard rule**: Do not invent, infer, or embellish content. If it is not in the document, it does not go into Parta.

---

## Step 4 — Connect to Parta and select templates

1. There is only one company in Parta, so this step is simple.
2. Call `list_editor_template_groups` to retrieve all available template groups.
3. **Prefer Sharon's custom collections** over the Parta Quick-Start Collection. If custom collections exist, list them for the user and ask which to use. Only use Parta Quick-Start if the user explicitly says to.
4. If the course shape (number of pages, sections, quiz presence) was not specified in the document, call `get_course_creation_defaults` and present the defaults to the user for confirmation.

---

## Step 5 — Propose the course structure

Before building anything, show the user a structured outline:

```
Course title: [title]
Audience: [persona]
Template collection: [collection name]

Sections:
  1. [Section name] — [block type] — [template name]
  2. [Section name] — [block type] — [template name]
  ...
  [Cover / Quiz if applicable]
```

Ask the user to approve or modify this structure. Do not call any create tools until the structure is approved.

---

## Step 5 — Build in Parta

Follow `get_build_project_from_template_guide` for the technical workflow. Execute in this order:

1. `create_editor_project`
2. `create_editor_sections` for each approved section
3. Instantiate blocks from the approved templates
4. Fill each block's content nodes using the source document

**During content fill**:
- Apply CBX Writing Style Guide rules to every content node (tone, prohibited terms, formatting, punctuation)
- If a content node requires information not present in the source document — stop and ask. Do not pad or fabricate.
- If any block schema requires more panels or nodes than the source content supports, ask the user how to handle it rather than inventing filler.

---

## Step 6 — Report completion

When the build is complete, report:

- Project name and link (if available)
- Total sections built
- Total blocks instantiated
- Any content nodes left empty, and why
- Any items that need follow-up from the user

---
name: it-courseware-md-labs
description: Generate reusable beginner-friendly undergraduate IT courseware as a multi-Markdown-file project from an IT keyword list, optionally anchored to existing courseware/reference materials (Markdown/PDF/DOCX). Supports phased workflow (explore/plan/implement/review) driven by command-like parameters, and generates exactly one chapter per implement run. Each chapter must include principles, VM CLI hands-on demo, real-world scenarios, and exactly one integrated practical homework with a full solution. When uncertain, must verify via web search with at least two authoritative sources. Conflicts must be escalated to open-questions for user confirmation.
---


# it-courseware-md-labs

## What this skill does
Create and maintain a **courseware project** made of multiple Markdown files for complete-beginner undergrads, driven by a command-parameter style interface.

Inputs can include:
- IT keyword list
- Existing courseware/reference materials: `.md`, `.pdf`, `.docx`

Default authoring strategy: **outline-first (以旧为纲)**.

## Command protocol (conversation-driven)
Users drive the workflow with a single-line command plus optional structured blocks.

### Canonical command prefix
Use `@course`.

### Required parameters
`phase` is required. Allowed values:
- `explore` — inventory + reference extraction only (no chapter writing)
- `plan` — produce/refresh plan artifacts and index
- `implement` — generate exactly **ONE** chapter
- `review` — consistency and executability checks

### Common parameters
- `course_dir` (default: `course`)
- `anchor_mode` (must be `outline_first`)
- `blocking` (must be `stop_on_questions`)
- `context_mode`:
  - `fresh`: ignore chat history; rely only on artifacts under `course_dir`
  - `resume`: may use chat context in addition to artifacts
- `chapter_selector` (default: `by_plan_id`) — for implement phase

### Implement-only parameters (mandatory)
- `chapter_id` (e.g. `ch03`)
- `vm_os` (OS + version; must ask every implement run if missing)
- `vm_net` (`yes|no`, default `yes`)

### Examples
```text
@course phase=explore course_dir=course anchor_mode=outline_first blocking=stop_on_questions context_mode=fresh
keywords=["DNS","HTTP","TLS","Linux权限","Git分支"]
inputs=["旧课件.md","讲义.pdf","参考.docx"]
```

```text
@course phase=plan course_dir=course anchor_mode=outline_first blocking=stop_on_questions context_mode=fresh
```

```text
@course phase=implement course_dir=course chapter_id=ch03 vm_os="Ubuntu 22.04" vm_net=yes anchor_mode=outline_first blocking=stop_on_questions context_mode=fresh
```

## Output project structure (must follow)
Create/maintain these paths under `course_dir`:
```
course/
  00-README.md
  01-index.md
  02-style-guide.md
  03-lab-standards.md
  04-glossary.md
  05-references.md
  imported/                # originals (read-only)
  reference-extracts/      # extracted notes from PDF/DOCX/large MD
  chapters/                # generated chapters
  plan/
    inventory.md
    course-plan.md
    open-questions.md
```

## Hard content constraints
No fixed per-chapter template and no fixed number of knowledge points.

However, **each chapter file** must contain all of the following somewhere in the narrative:
1) Principles / how it works (原理)
2) Hands-on CLI demo in local VM (实操演示)
3) Real-world scenarios (应用场景)
4) Exactly **ONE** integrated practical homework (作业) and a full solution (答案)

## Reference ingestion (MD / PDF / DOCX)
All non-trivial references must be normalized into `reference-extracts/*.md`.

### Markdown (.md)
- Store originals under `imported/`.
- If large, create a compact extract note in `reference-extracts/`:
  - outline, key points, reusable labs, conflicts, chapter mapping.

### Word (.docx)
- Extract headings and key paragraphs into `reference-extracts/ref-*.md`.
- Preserve provenance markers: `[filename, heading path, paragraph index (if possible)]`.
- If available, prefer using a DOCX-specific workflow/tooling.

### PDF (.pdf)
Determine type:
- Text-based PDF: extract text/outline.
- Scanned (image-only) PDF: perform multimodal/OCR-like extraction if tools allow.

If tool support for OCR/multimodal PDF extraction is unavailable in the current environment:
- Ask the user to provide either (a) OCRed text export, or (b) the exact text for the relevant pages/sections.

For scanned PDF extraction:
- Always record page numbers for each claim.
- Assign confidence per claim (High/Medium/Low).
- List suspected OCR errors explicitly.

**Blocking rule (user-selected):**
If a Medium/Low-confidence claim affects chapter structure or key technical conclusions:
- Write a blocking item into `plan/open-questions.md` and STOP Plan/Implement.
- Ask the user to confirm the exact page snippet or provide corrected text.

## Conflict policy (user-selected)
If conflicts exist among:
- imported outline vs extracted outline,
- reference extracts vs web sources,
- two authoritative sources disagree,
then:
- write a decision request into `plan/open-questions.md` with options + implications,
- STOP and wait for user confirmation.

## Uncertainty policy (mandatory web verification)
If any technical detail is uncertain (CLI flags, protocol semantics, security advice, version behavior):
1) Use available web search tools to verify.
2) Cross-check with **>=2 independent authoritative sources**.
3) Record citations in `05-references.md`.

Preferred source order:
- Standards bodies (IETF RFCs, W3C)
- MDN for web platform behavior
- Official tool/vendor docs (git-scm.com, OpenSSH, systemd, distro docs)
- Man pages (still cite)

If you cannot confirm with two solid sources:
- mark uncertainty and confidence level,
- add a blocking question if it affects structure or correctness.

## Hands-on demo quality bar
Labs must be:
- safe (no destructive actions; explain privileges)
- reproducible (include prerequisites, verification, troubleshooting, cleanup)
- OS-aware (branch steps by OS; ask OS every implement run)

## Phase responsibilities

### Phase: explore
Produce:
- `plan/inventory.md`
- `plan/open-questions.md`
- `reference-extracts/ref-*.md` as needed

Must NOT:
- write full chapter content

### Phase: plan
Produce:
- `plan/course-plan.md`
- refresh `01-index.md`

Blocking:
- If `open-questions.md` contains blocking items -> STOP and ask user.

### Phase: implement (exactly ONE chapter)
Produce:
- one file under `chapters/` for `chapter_id`
- update `01-index.md`, `04-glossary.md`, `05-references.md`

Blocking:
- If `open-questions.md` contains blocking items -> STOP and ask user.

### Phase: review
Check:
- glossary consistency
- index links
- lab safety + verifiability
- homework solution completeness

## Bundled reference templates
See `references/` for recommended artifact templates.

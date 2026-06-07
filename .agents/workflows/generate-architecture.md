# Workflow: Generate Architecture Diagrams

**Trigger:** Invoked by `master-sync.md` Phase 4, or manually via `/ask run @[.agents/workflows/generate-architecture.md]`

## Objective
Programmatically regenerate the architecture diagram assets whenever the codebase structure changes. This workflow bridges the gap between the existing Python scripts (`docs/generate_architecture.py`, `docs/composite_diagram.py`) and the `diagram-generator` skill, ensuring they are actually invoked rather than sitting idle.

## Pre-Conditions
1. The `diagrams` Python package must be installed (`pip install diagrams`).
2. Graphviz must be available on the system path.
3. The `Pillow` (PIL) package must be installed for `composite_diagram.py`.

## Execution Steps

### Phase 1: Architecture Analysis
1. Read the current state of `src/capabilities/` to identify all active Python modules and their relationships.
2. Read `.agents/rules/` to identify governance components.
3. Read `.agents/workflows/` to identify workflow orchestration paths.
4. Read `.agents/product/templates/` to identify product design components.
5. Compare the discovered components against the existing `docs/generate_architecture.py` script to determine if the diagram is stale.

### Phase 2: Update Diagram Script
1. If new components have been added (e.g., new capabilities, new workflow categories, new skills), update `docs/generate_architecture.py` to include them as new nodes or clusters.
2. Maintain the strict Dark Mode styling (`bgcolor="#0D1117"`, `fontcolor="#FFFFFF"`).
3. Ensure the output path remains `docs/assets/architecture_diagram`.

### Phase 3: Generate Base PNG
1. Run `python docs/generate_architecture.py` to produce the raw programmatic diagram.
2. Verify the output file exists at `docs/assets/architecture_diagram.png`.

### Phase 4: Apply Composite & Watermark
1. If a background image is available (e.g., a gradient or textured background), run `python docs/composite_diagram.py <fg_path> <bg_path> <output_path>` to apply it.
2. The watermark `github.com/hitanshuac` is automatically applied by the composite script.
3. If no background image is available, skip this phase — the base PNG from Phase 3 is sufficient.

### Phase 5: AI Styling Pass (Optional — Recruiter-Facing)
1. If the diagram is intended for a recruiter-facing showcase, execute the styling instructions from `.agents/skills/diagram-generator/SKILL.md` §"The Strict AI Styling Pass".
2. Pass the base PNG into the `generate_image` tool with the exact prompt specified in the skill.
3. Overwrite `docs/assets/architecture_diagram.png` with the styled version.

### Phase 6: Verification
1. Confirm `docs/assets/architecture_diagram.png` exists and is non-zero bytes.
2. Confirm `README.md` references the image at the top: `![Architecture Diagram](docs/assets/architecture_diagram.png)`.
3. Report the result to the user.

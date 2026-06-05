---
name: Diagram Generator Skill
description: Instructions for programmatically generating high-fidelity architecture diagrams using Diagrams-as-Code to minimize LLM token usage and prevent image gibberish.
---

# Diagram Generator Skill

When instructed to update or generate an architecture diagram (e.g., during the `publish-showcase.md` workflow), you must **NEVER** use an AI image generator (like DALL-E or Imagen). Instead, you must use **Diagrams-as-Code** to generate pixel-perfect PNGs.

## Supported Tools
The Base Agentic Environment supports two industry-standard tools. Choose the right tool based on the diagram's requirements:

### 1. Python `diagrams` (mingrammer)
**Best for:** Cloud architecture, specific technology nodes (AWS, GCP, Python, Databases), and SRE visual flow.
- **Dependency:** Ensure `diagrams` is in `requirements.txt`.
- **Execution:** Create a script (e.g., `docs/generate_architecture.py`), define the nodes, and run it via `python <script>.py`.
- **Note:** Graphviz must be installed on the system path.

### 2. D2 (Declarative Diagramming)
**Best for:** Fast sketch-style diagrams, generic software flowcharts, UML replacements, and abstract component logic.
- **Dependency:** D2 binary is pre-installed via winget.
- **Execution:** Create a `.d2` text file (e.g., `docs/architecture.d2`) with D2 syntax (`A -> B: request`).
- **Render:** Run `d2 docs/architecture.d2 docs/assets/architecture_diagram.png --theme=200 --sketch` (Theme 200 is dark mode).

## Workflow Execution
1. Read the `README.md` to understand the conceptual relationships.
2. Select the appropriate tool.
3. Generate the code (Python or D2).
4. Run the code to output the base `docs/assets/architecture_diagram.png`.

### The Hybrid Aesthetic Pass (Image-to-Image)
Because programmatic diagrams often lack visual flair, you MUST apply a stylistic pass if the diagram is meant for recruiter-facing or showcase assets:
1. After generating the base PNG via code, pass that PNG path directly into the `generate_image` tool via the `ImagePaths` parameter.
2. Prompt the AI: *"Use the provided architecture diagram as a strict structural base. Redraw it exactly node-for-node. The aesthetic MUST be a highly professional business dashboard and product showcase: add sleek borders, soft drop shadows, and a clean, high-end enterprise layout with glassmorphism and subtle glowing neon blue/purple accents. The layout must remain identical to the base image. Make sure the text is as clear as possible. Finally, it is a STRICT RULE to include a distinct watermark reading 'github.com/hitanshuac' in the bottom corner."*
3. Overwrite the original `architecture_diagram.png` with this new, stylized masterpiece.
4. Check `git status` and commit the new diagram.

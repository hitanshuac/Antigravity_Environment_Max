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
- **Execution:** Create a script (e.g., `docs/generate_architecture.py`), define the nodes, and run it via `python <script>.py`. Ensure `graph_attr` and `cluster_attr` enforce Dark Mode (`bgcolor="#0D1117"`).
- **Note:** Graphviz must be installed on the system path.

### 2. D2 (Declarative Diagramming)
**Best for:** Fast sketch-style diagrams, generic software flowcharts, UML replacements, and abstract component logic.
- **Dependency:** D2 binary is pre-installed via winget.
- **Execution:** Create a `.d2` text file (e.g., `docs/architecture.d2`) with D2 syntax (`A -> B: request`).
- **Render:** Run `d2 docs/architecture.d2 docs/assets/architecture_diagram.png --theme=200 --sketch` (Theme 200 is mandatory for dark mode).

## Workflow Execution
1. Read the `README.md` to understand the conceptual relationships.
2. Select the appropriate tool.
3. Generate the code (Python or D2).

### The Composite Overlay Protocol (Text + Aesthetics)
To achieve a breathtaking visual aesthetic while maintaining 100% deterministic text (no gibberish), use the composite overlay method:
1. Ensure `graph_attr["bgcolor"]` is set to `"transparent"` in the Python script.
2. Run the code to output the transparent foreground to `docs/assets/architecture_diagram.png`.
3. Use the AI Image Generator to create an empty background plate: *"A stunning, completely empty, highly professional business dashboard background. Do NOT include any text, letters, nodes, or flowchart elements. Just the aesthetic shell: sleek borders, soft drop shadows, a clean, high-end enterprise layout with glassmorphism panels, and subtle glowing neon blue and purple accents over a dark background."*
4. Run `python docs/composite_diagram.py docs/assets/architecture_diagram.png <path_to_ai_bg> docs/assets/architecture_diagram.png` to merge the layers.
5. Check `git status` and commit the composite masterpiece.

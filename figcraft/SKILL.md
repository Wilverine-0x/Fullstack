---
name: figcraft
description: Drive FigCraft interface design and prototyping tool live or headless via MCP. Use when creating UI/UX designs, wireframes, vector components, layouts, or inspecting FigCraft documents and exporting design assets to code.
---

# FigCraft Agent Skill

This skill allows the agent to design UI/UX interfaces directly in **FigCraft** (an open-source, Rust-powered collaborative Figma alternative) live on desktop or headlessly via the FigCraft MCP server.

## Architecture & Workflows

FigCraft shares the exact same 400+ commands across its native GUI, CLI, and MCP server.

### 1. Live Interactive Mode (User Desktop)
When the user wants to see edits happening live:
1. Ensure the user has opened FigCraft via the shortcut **`FigCraft (Live AI Mode)`** (which launches `figcraft.exe --control 8089`).
2. The agent connects to the running window via the `figcraft` MCP tools attached to port 8089.
3. Every node created or layout modified is updated in real time on the user's screen.

### 2. Available MCP Tools
- **`run_command`**: Execute any FigCraft command (e.g. `node.create`, `frame.create`, `node.set`, `selection.set`).
- **`batch`**: Execute multiple layout commands atomically.
- **`create_node`**: Convenient wrapper to create frames, text, rectangles, vectors, or components.
- **`set_properties`**: Modify layout (auto layout, padding, gap, fills, strokes, opacity, corner radius).
- **`inspect_document`**: Get the full JSON tree of layers and properties.
- **`get_node`**: Inspect a specific node by ID.
- **`select`**: Change layer selection in the canvas.
- **`export`**: Export selected frames or whole documents as PNG, SVG, PDF, or JSON.
- **`screenshot`**: Capture the canvas or current document view.

## Design Best Practices in FigCraft
- **Frames & Screens**: Use standard mobile or desktop presets (e.g., iPhone 16 `393x852`, Desktop `1440x900`).
- **Auto Layout**: Always group related elements into auto-layout frames with proper alignment, horizontal/vertical gap, and padding.
- **Hierarchy & Naming**: Name every layer semantically (e.g., `Header`, `HeroSection`, `PrimaryButton`, `TitleText`).
- **Colors & Contrast**: Maintain clean UI contrast with clear foreground and background palette pairings.

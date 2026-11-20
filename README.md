# Revit Hybrid

A Codex skill for Autodesk Revit. It uses Revit MCP Next for BIM edits, drawing production and verification, and Cua Driver for app launch and UI-only operations.

## Setup

1. Download the skill ZIP from the repository releases.
2. Extract the entire `revit-hybrid` folder into your personal Codex skills directory, normally `~/.codex/skills`. Keep `SKILL.md` and the `references` directory together.
3. Make sure Revit, Revit MCP Next and Cua Driver are available in your environment. The skill uses those tools when the task permits them.
4. Start a new Codex thread and invoke `$revit-hybrid`. For a first run, ask it to identify your open Revit document and active view without making edits.

# BIM and drawing-set checks

Use the checks that match the brief and stage. For an open-ended concept, state the building program and assumptions, develop the model and show what was verified. Treat site, regulatory and construction decisions as separate scope when the project inputs do not support them.

## New architectural model

| Phase | Establish before the write | Verify after the write |
| --- | --- | --- |
| Template and levels | Discipline, units, native template, output path and storey datums. Identify the exact floor-plan views. | Correct document and units, named levels and elevations, matching plan IDs. |
| Building envelope | Closed exterior footprint, real wall and floor types, level and roof relations. | Sections and 3D show coherent slab, wall and roof junctions. Check actual constraints or heights, since changing a GUI field may leave its original value. |
| Layout and openings | Interior partition types, intended floor and host ID for each door or window, and a circulation path. | Each opening cuts its intended wall on the intended level. No duplicate or displaced hosts. Check an elevation when stacked openings matter. |
| Vertical circulation | Stair base and top levels, slab opening and guard path, including the landing approach. | Plan and section show the stair, opening, guard and landing together. Check actual rise, run and clearances when required by the brief. |
| Rooms | Closed boundaries, one interior point per room, and meaningful numbers and names. | Room schedule confirms names, levels and nonzero areas. Check the displayed location of each tag in a plan. |
| Drawing set | Decide sheet content, scales, crops and units up front. Use matching sheet and view IDs. | Capture each sheet. Check that plans and sections are readable, annotations sit clear of doors and cut lines, and no view or title overlaps the title block. |

## Batch the documentation

- Read current views, types and sheets with `read_many` before creating new ones. A template can already contain a room schedule, views with identical names, and a blank sheet. Reuse a suitable one or choose an intentional unique name.
- Set view crops, scale, underlay and detail level before placing views. Dimension with references from the matched elements. For adjacent storeys, inspect the cut plane and level before relying on visible geometry.
- Create tags, schedules and sheets with native Revit tools. Place viewports using their sheet coordinates and keep the title-block area clear. A viewport coordinate is its center and a schedule coordinate is its upper-left corner. Verify the real layout from a capture, including schedule rows and totals, view labels, and the drawing border. If a long room schedule overflows, split it by storey with level filters and reconcile the placed rows with the model's room count.
- If a UI edit is necessary, verify the specific property or input focus before entering a value. Avoid a long replay across tool modes or dialog boundaries. Return to MCP when the UI-only operation is complete.
- Run the warnings check and read the final schedule or counts. Zero Revit warnings establishes the status of the model checker. Use the plan, section, schedule and project requirements to assess design adequacy.

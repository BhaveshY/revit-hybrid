---
name: revit-hybrid
description: Route Autodesk Revit work between Revit MCP and Cua Driver for hybrid modeling, documentation, app launch, UI-only operations and visual QA. Use for creating or editing RVT models, preparing drawing sets, and tests that exercise both tools. Honor an explicit MCP-only or Cua-only request.
---

# Revit hybrid workflow

Use the task's authorized tools. When both are allowed, give Revit MCP ownership of the model and use Cua Driver for the parts that genuinely need the Windows interface. Treat an explicit Cua workflow test as a Cua deliverable. Verify its requested operations through Cua and identify any separately authorized MCP work in the result.

## Route each operation

| Operation | Route when both tools are allowed |
| --- | --- |
| Find open instances and documents | Revit MCP `status`. Reuse the requested healthy instance. |
| Launch Revit, observe first-run UI, or locate a native sign-in prompt | Cua Driver. The user completes Autodesk authentication. Check MCP `status` once the app is ready. |
| Create a project from a known template | Revit MCP `manage_document.new_project` with a unique output path and the appropriate template. Use Cua's New Project dialog when the UI workflow is the test or template selection requires it. |
| Model or edit BIM elements, set parameters, create views or sheets, annotate, schedule or export | Revit MCP. Use grouped reads, list real types, and batch related writes into transactions. |
| Inspect the model, plans, sections or sheets | Revit MCP readback and `capture`. Use Cua for app chrome, dialogs or verification on the physical display. |
| Perform a UI-only operation | Cua Driver with an exact window, a bounded observation and an independently verified outcome. |

The installed Revit MCP skill governs tool schemas, confirmations, dialogs and recovery. The installed Cua Driver skill and applicable app instructions govern input permissions, targeting and sessions. Apply both where applicable. A tool failure does not authorize changing the user's requested execution method or bypassing a confirmation, licensing requirement or access boundary.

Discover the connected tools before planning the handoff. Use the Revit MCP operations actually advertised by the connection. If Cua is unavailable, continue with authorized MCP work and identify any UI-only step you could not complete. If MCP is unavailable, follow its connection recovery guidance and use Cua for model work only when the task and host instructions permit it.

## Work in phases

1. **Bind the target.** Start with MCP `status`. Identify the document by instance, title and path where available. Pin it before writes if several documents are open or a document was named. Use a unique file for a new project. Save, overwrite, close or sync only when authorized. For a new file, choose the template for the requested discipline and units before modeling.
2. **Read once per phase.** Use `read_many` for levels, types, views and relevant counts. Retrieve specific locations, bounding boxes or parameters only when needed. Use the exact view ID when a floor plan and reflected ceiling plan share a name. Use live `help` for an unfamiliar operation and keep the readback handle or IDs. For full-set checks, reconcile returned rows with the reported total. Follow pagination or use narrower queries when a response is shortened.
3. **Write in coherent batches.** Use one homogeneous operation for repeated items, `change_set` for dependent operations, and result references such as `"$0"` when an operation needs a newly created host. Keep dependent geometry together. Run lifecycle operations individually. If a transaction rolls back, fix the failing operation before replaying the batch. If the outcome is pending or unknown, follow `job_status` and the server's next step before making another write.
4. **Verify the outcome.** Read back actual values, relationships and counts after each batch. A rendered view helps catch placement, clipping and layout errors that counts miss. For architectural projects, see [BIM and drawing-set checks](references/drawing-set.md).
5. **Close the loop.** After the final captures, activate the requested presentation view when appropriate. Save the authorized output and confirm its exact path and saved document state. If the physical display matters, verify the window title. Report what each route completed, what you verified, and any remaining limitation. Report elapsed time only when measured, with end-to-end time distinct from tool execution.

## Hand off between tools

- **MCP to Cua.** Read the current native window and confirm its PID and document title against MCP. Keep snapshots and actions on one persistent driver connection. Reuse any matching runner and app notes. When testing a specific driver build, check the version of the live connection because an older thread can retain its earlier process. Move the task window to the designated display under applicable host rules. Use background UIA for controls that support it. Use the Revit canvas foreground exception only when applicable and authorized. Prefer a bounded tree without a screenshot and capture the window when canvas geometry is needed.
- **Batch known steps.** Use `run_steps` or `run_script` for known controls, with verification in the same batch where possible. Stop on a changed window, unexpected dialog or edit mode. A successful dispatch can still target an underlay, remain in sketch mode or put text into the wrong field. Verify the mode, host or field before the next dependent action. Once exact-window UIA is shown to be ineffective in a project canvas, use the authorized canvas route or hand the operation to MCP.
- **Cua to MCP.** Verify that the intended dialog or sketch is finished and Revit is idle. Confirm the active document and view with `status`; rebind after creating, opening or saving as. Read back the actual model before making a dependent write, especially after a modal, a partial batch or an unconfirmed action. Keep the two tools from editing the same model concurrently.
- **MCP error or UI modal.** Follow the Revit server's `next:` and `fix:` instructions and inspect the exact modal when allowed. Use the prescribed Cancel or Close recovery where appropriate. A sign-in, protected decision or unknown write outcome needs the specified recovery before proceeding. When hybrid use is authorized, an otherwise unsupported, understood UI action can be handled through Cua after the model state is established.

## Example routing decision

For “Open Revit, create a two-storey studio and deliver concept sheets,” launch through Cua only if the host is closed. Once Revit is ready, create the file, levels, walls, openings, stair and drawing set through MCP in verified batches. Verify the drawings through native capture. Use Cua to show the finished sheet or cover any explicitly requested GUI smoke test. A floor-plan underlay can offer a lower-level wall as a door host, and a click on a room tag can leave text focus elsewhere. Explicit host IDs, level checks and batched parameter edits remove those dependencies on screen state.

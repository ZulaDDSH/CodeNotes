# ui-workflow-run code notes

## A member with no recorded outcome under an ended run is incomplete, not running (finding 389)

- `packages/client/ui-workflow-run/src/client/workflow-definition.ts:projectWorkflow` shows a member without an `outcome` as `running` whenever its run has no `run-end`. The projection now distinguishes a run that has recorded `run-end`: its member without an outcome becomes the new `incomplete` status, because the final member outcome was lost and the child may have stopped. A run that is still open keeps `running`, and a turn closed by an interruption or fork keeps `interrupted` as before.
- The producer path that causes this is `packages/workflow/workflow-ptc/src/guest.ts:runWorkflowGuest`: a failed progress delivery drops queued `agent-end` events and returns an error result, which the tool then records as the run end. The renderer is the only fix in this change; the producer still drops those events.
- The `incomplete` status is view-only. No durable event schema changes: `tool-workflow/*` events are unchanged, and the status is derived when the view is built.
- `packages/client/ui-workflow-run/src/client/WorkflowRunPanel.tsx:STATUS_KEYS` and `dotState` map the status to the new `status.incomplete` and `statusCount.incomplete` dictionary keys, and `abnormal` treats it as attention-worthy so a completed run with an incomplete member is not collapsed as clean.
- Covered by `packages/client/ui-workflow-run/tests/workflow-run.client.spec.tsx` (a member without outcome under a run-end reads as incomplete).

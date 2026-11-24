# Workflow PTC: code notes

`packages/workflow/workflow-ptc/src/host.ts:PtcWorkflowRun::disposeChild`: finding 281. The workflow calls each child's `run.dispose()` once; the retry layer is the subagent manager's bounded handle disposal (see subagent.md, `SubagentManager::finishDisposal`), so a rejecting child is disposed `TEARDOWN_ATTEMPTS` times in total, not once per workflow attempt. The failure is recorded in `cleanupFailures` and the record is removed from `children`; a later `dispose()` on the same activation re-drives a failed handle disposal through the manager.

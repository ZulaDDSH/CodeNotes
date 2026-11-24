# Workflow PTC: code notes

`packages/workflow/workflow-ptc/src/host.ts:PtcWorkflowRun::disposeChild`: finding 281. A child disposal gets two attempts before its failure is recorded in `cleanupFailures`. The second attempt re-drives the subagent manager's failed handle disposal (see subagent.md, `SubagentManager::dispose`). The record is still removed from `children` once the disposal settles, so a failure is reported, not retried later by the run.

Attempt count: a rejecting child whose manager teardown has already been attempted once by natural settlement is disposed nine times in total, because each workflow attempt re-drives the manager's three-attempt handle disposal. `packages/workflow/workflow-ptc/tests/workflow-ptc.spec.ts` pins that count for the asynchronous rejection case.

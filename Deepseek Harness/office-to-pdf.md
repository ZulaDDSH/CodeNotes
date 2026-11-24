# office-to-pdf code notes

## A reader whose result copy fails settles with that error (finding 388)

- `packages/document/office-to-pdf/src/queue.ts:ConversionQueue::finish` - each reader used to be released before its result copy, so a copy that threw (allocation failure) left that reader out of the job and unsettled, because `fail` only rejects readers still attached to the job. The copy now runs first and the reader is released only after it succeeds; a throwing copy leaves the remaining readers attached so `fail` rejects them with the copy error.
- `packages/document/office-to-pdf/src/queue.ts:ConversionQueue::read` - alias hits still return a copy before reader admission and consume no reader slot. This is the documented limit in `packages/document/office-to-pdf/README.md`; the queue cannot see how long a caller or the Remote transport holds a delivered copy, so an aggregate delivery budget is not enforced here. Not changed (finding 388, aggregate half).

# Compaction tool-result pruner code notes

`packages/compaction/compaction-tool-result-pruner/src/config.ts:codePointLength` counts code points by scanning UTF-16 units without materializing an array, so measuring a large tool result against the threshold allocates nothing proportional to its size. A lone surrogate still counts as one code point, matching the earlier `Array.from` count.

`packages/compaction/compaction-tool-result-pruner/src/config.ts:codePointOffset` maps a code-point index to its UTF-16 offset by scanning, and `packages/compaction/compaction-tool-result-pruner/src/index.ts:ToolResultPruner.pruneContent` slices the retained head and tail by those offsets instead of building a code-point array per block. Work stays linear in the input but extra memory is bounded by the retained text.

`packages/compaction/compaction-tool-result-pruner/src/index.ts:ToolResultPruner.pruneSession` measures each candidate once and passes the measured count to the pruning step, so reporting no longer recounts the original content.

`packages/compaction/compaction-tool-result-pruner/src/index.ts:ToolResultPruner.pruneSession` accepts `PruneSessionOptions.previouslyConsumed` (salvaged from split/compaction-consumed-pruning, bc3d70838b). The pass snapshots the surface once, finds the index of the last `assistant/message` in that snapshot, and prunes only `tool/result` nodes before it: a later assistant response proves a subsequent model request already consumed the result. Without a later assistant message nothing is eligible. The option is opt-in so the default pass still trims every over-budget result once a compaction trigger qualifies.


`packages/subprocess/subprocess-local/src/retention.ts:sweepSpillFiles` (empty directory removal): `rmdir` fails on a non-empty directory, which is then kept for the next sweep. The catch continues the loop, which is the only action the loop body would take.

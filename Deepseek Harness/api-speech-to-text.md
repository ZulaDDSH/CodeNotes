# api-speech-to-text code notes

## packages/experimental/api-speech-to-text/src/index.ts:SpeechController::transcribe (finding 403, Host side)

- `packages/experimental/api-speech-to-text/src/index.ts:SpeechController::transcribe` - The Host limits are unchanged. The Client now stops a recording at `recordingLimitSeconds(maxDurationSeconds, maxAudioBytes)` so an accepted recording never exceeds the byte cap that this method enforces; no Host code changed for finding 403.

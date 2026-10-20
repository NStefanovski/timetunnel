# ADR-0003: Progress lives in `localStorage`; there is no backend

**Decision**
Visitor progress — stories seen, stories mastered, best quiz scores, chosen mode and volume — is
stored in `localStorage` under the key `time-tunnel-v2` (line 423). There is no server, no
account, and no sync.

**Reason**
A single static file with no backend is the whole point (ADR-0002). Progress is a convenience,
not a record worth an account system.

**Consequences**
- Progress is per-browser and per-device, and is lost when site data is cleared. That is
  accepted, not a bug to fix.
- **Changing the stored shape silently wipes every existing visitor's passport.** Bump the key
  (`time-tunnel-v3`) instead of reinterpreting the old one, or migrate explicitly on read.
- Reads are wrapped in `try/catch` — private windows and blocked site data must not break the
  page. Keep that true for any new read or write.

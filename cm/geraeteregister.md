# Geräteregister (v1, Sprint 2, T-0018)

*Je Gerät: Identität, Toolchain, erlaubte Rollen, Rechteumfang, Status. Kein Token-Klartext — nur Referenzen. Neuaufnahme nur mit Mensch-Freigabe (guardrails: device_onboarding).*

| Gerät | Identität/Token-Referenz | OS / Toolchain | Erlaubte Rollen | Rechte | Status |
|---|---|---|---|---|---|
| **team-node-1** (User-PC) | GitHub `DiOflOrds` via Windows Credential Manager | Windows 11 · Git · Python 3 · Ollama (installiert: `gemma3:27b`; guardrails-Default `llama3.1:8b` NICHT installiert → `OLLAMA_MODEL` setzen, s. L-003) | alle aktiven Rollen (autonome Ticks, D007) | Repos lesen/schreiben/pushen | aktiv (D007, 2026-08-06) |
| **cowork-session** (Cloud-Sandbox, wechselnd) | Ordner-Mount `aspice-team-repos-final` (Session-lokal) | Ubuntu 22 · Git · Python 3 | Engineering-/Rollen-Kontexte, kein Push (extern read-only, D007) | lokale Commits im Mount; Push durch Mensch | aktiv je Session |
| *hub-vm (geplant)* | — | Docker Compose (E5) | Orchestrator + alle Rollen | voll | geplant Sprint 3 |

## Betriebshinweise

**Preflight je Session-/Tick-Start (T-0024, ab Sprint 3):** Auf jedem Gerät vor Arbeitsbeginn `python platform/scripts/preflight.py --repos <wurzel>` ausführen (Ticks rufen es automatisch; `--skip-tests` im Tick-Modus). Das Skript entfernt verwaiste Git-Locks nur, wenn kein Git-Prozess läuft; kann es auf einem Mount nicht löschen (R7: „Operation not permitted"), zuerst die Lösch-Berechtigung der Session aktivieren bzw. auf dem Host löschen — sonst blockieren Commits mit `cannot lock ref 'HEAD'`.

# Geräteregister (v1, Sprint 2, T-0018)

*Je Gerät: Identität, Toolchain, erlaubte Rollen, Rechteumfang, Status. Kein Token-Klartext — nur Referenzen. Neuaufnahme nur mit Mensch-Freigabe (guardrails: device_onboarding).*

| Gerät | Identität/Token-Referenz | OS / Toolchain | Erlaubte Rollen | Rechte | Status |
|---|---|---|---|---|---|
| **team-node-1** (User-PC) | GitHub `DiOflOrds` via Windows Credential Manager | Windows 11 · Git · Python 3 · Ollama (installiert: `gemma3:27b`; guardrails-Default `llama3.1:8b` NICHT installiert → `OLLAMA_MODEL` setzen, s. L-003) | alle aktiven Rollen (autonome Ticks, D007) | Repos lesen/schreiben/pushen | aktiv (D007, 2026-08-06) |
| **cowork-session** (Cloud-Sandbox, wechselnd) | Ordner-Mount `aspice-team-repos-final` (Session-lokal) | Ubuntu 22 · Git · Python 3 | Engineering-/Rollen-Kontexte, kein Push (extern read-only, D007) | lokale Commits im Mount; Push durch Mensch | aktiv je Session |
| *hub-vm (geplant)* | — | Docker Compose (E5) | Orchestrator + alle Rollen | voll | geplant Sprint 3 |

## Soll-Toolchain je Einsatzzweck (P2/T-0011, BB-4 — VOR einem Lauf prüfen, nicht im Fehlversuch)

| Einsatzzweck | Soll auf dem Gerät | Prüfung |
|---|---|---|
| Basis (Board, Preflight, Tests, abschluss.cmd) | Git (Credential Manager als DiOflOrds), Python 3.10+, PyYAML | `python platform/scripts/preflight.py --repos .` |
| DR-/Entscheidungs-Mails | Env: SMTP_HOST/SMTP_PORT/SMTP_USER/SMTP_PASS/MAIL_TO (Runbook Kap. 8) | Testlauf abschluss.cmd, Marker im Ticket |
| Ollama-Provider | Ollama-Dienst + installiertes Modell (`OLLAMA_MODEL`, L-003) | `ollama list` |
| **Copilot-PoC (BB-1, p0/T-0072)** | **GitHub Copilot CLI installiert + eingeloggt** (`copilot` im PATH; sonst `providers.copilot.befehl` in guardrails.yaml anpassen) | `copilot --version` — Stand 2026-08-15 auf team-node-1 NICHT installiert |
| Mission Control (Server) | Python 3.10+, Port 8080 frei, LAN-Zugriff optional (D017) | `python platform/backend/server.py --repos .` |

## Betriebshinweise

**Preflight je Session-/Tick-Start (T-0024, ab Sprint 3):** Auf jedem Gerät vor Arbeitsbeginn `python platform/scripts/preflight.py --repos <wurzel>` ausführen (Ticks rufen es automatisch; `--skip-tests` im Tick-Modus). Das Skript entfernt verwaiste Git-Locks nur, wenn kein Git-Prozess läuft; kann es auf einem Mount nicht löschen (R7: „Operation not permitted"), zuerst die Lösch-Berechtigung der Session aktivieren bzw. auf dem Host löschen — sonst blockieren Commits mit `cannot lock ref 'HEAD'`.

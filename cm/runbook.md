# Betriebs-Runbook — ASPICE-Team (v1, Sprint 6, T-0067, CM)

*Betriebsmodell: lokal auf Team-Node-1 (D017); Cowork-Sessions für Engineering/Planung; GitHub als Backbone. Gilt bis P1-Review.*

## 1. Normalbetrieb

Session-/Tick-Start immer mit Preflight: `python platform\scripts\preflight.py --repos .` (prüft Locks, Arbeitskopien, Board, Tests — inkl. Produkt-Repos). Statuswechsel nur über `python platform\scripts\board.py <repo> status …`. Mission Control: `python platform\backend\server.py --repos .` → http://127.0.0.1:8080. Ticks: `python platform\orchestrator\tick.py --repos .` (Provider-Override mit `--provider`).

## 2. Backup

GitHub ist die Primärsicherung (jeder Sprint-Abschluss pusht alle Repos inkl. Tags). Die lokale Arbeitskopie `Downloads\aspice-team-repos-final` ist die Zweitkopie. Empfehlung: vor Windows-Updates o. Ä. zusätzlich Ordner-Zip an anderen Ort. Wiederherstellung: frisches `git clone` aller vier Repos + `sprintN-abschluss.cmd`-Prüfteil.

## 3. Monitoring

Drei CI-Gates je Push (platform: Tests+Matrix; p0: board-check; produkt: Tests+Matrix) — die Abschluss-Skripte öffnen die Actions-Seiten automatisch. Kosten/KPIs: Run-Registry (`p0/management/runs/`) bzw. Mission-Control-KPI-Sicht. Budget-Guardrails stoppen autonome Ticks hart (NOTFALL-MELDUNG.md beachten).

## 4. Update-Prozeduren

**Baseline:** nach G4 taggen (`genesis-vX.Y` auf process/platform/p0) + Manifest in `process/baselines/` mit QM-Mitzeichnung. **Produkt-Release:** G3 → Tag `vX.Y.Z` + `req-vX.Y` → Katalog-Eintrag via `python platform\scripts\catalog.py …` → Release Notes. **Prozess-Änderungen:** nur per CR mit Erwartungswert (Retro-Mechanik).

## 5. Störungsbehandlung

| Störung | Behandlung |
|---|---|
| Git-Lock (R7: index.lock/HEAD.lock) | Preflight erneut laufen lassen (räumt verwaiste Locks, wenn kein git-Prozess läuft); in Cowork-Sessions ggf. Datei-Löschrecht erteilen |
| CI rot | Lokal reproduzieren (Tests/Matrix wie im Abschluss-Skript); Secret-Fehler → Meldung im Workflow-Kopf befolgen (PATs erneuern: Ablaufdatum!) |
| Tick bricht ab / Guardrail | `p0/management/runs/NOTFALL-MELDUNG.md` lesen; keine weiteren Ticks bis Ursache geklärt; Budget-Limits nur per CR + Mensch ändern |
| Copilot-CLI-Aufruf schlägt fehl | `providers.copilot.befehl` in `platform/orchestrator/config/guardrails.yaml` an installierte CLI-Version anpassen; Login prüfen |
| Ollama nicht erreichbar | Dienst starten; Modell prüfen (`OLLAMA_MODEL`, Geräteregister-Hinweis L-003); Kette fällt sonst auf session/claude |
| Inbox nicht erreichbar | Erwartet, wenn Team-Node aus/außerhalb LAN (D017 bewusst akzeptiert) — Entscheidungen alternativ via Session-Dialog |

## 6. Geräte-Onboarding (F12/D023: weitere Geräte geplant)

Je neues Gerät: (1) Mensch-Freigabe (guardrails: device_onboarding), (2) Toolchain: Git + Credential Manager (Login als DiOflOrds), Python 3.11+, optional Ollama/Copilot CLI, (3) `git clone` aller Repos in einen Wurzelordner, (4) Preflight grün, (5) Eintrag im Geräteregister (`process/cm/geraeteregister.md`) mit Identität, Toolchain, erlaubten Rollen, Rechten. Erst danach autonome Ticks auf dem Gerät.

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

## 7. Betriebs-Backlog (Stand P1-Abschluss, 2026-08-15)

| # | Aufgabe | Quelle | Hinweis |
|---|---|---|---|
| BB-1 | Copilot CLI am Team-Node installieren, dann `python platform\orchestrator\tick.py --repos . --ticket T-0072 --provider copilot` | p1/T-0018, p0/T-0072 (P0-K9) | Fehlversuch 2026-08-15 in Run-Registry protokolliert |
| BB-2 | Checkliste „externen Dienst einrichten" (2FA → Secret/Env → Testlauf → Empfänger prüfen) hier ins Runbook | P1-Retro R1 | Lehre aus dem SMTP-Erstbetrieb (3 Anläufe) |
| BB-3 | Frist-Warnung in `dr_benachrichtigung.py` (DRs nahe/über Frist erneut mailen) | P1-Retro R2 | requirements-first: erst SWR, dann Umsetzung |
| BB-4 | Geräteregister um Soll-Toolchain je PoC ergänzen (z. B. Copilot CLI) | P1-Retro R3 | verhindert BB-1-artige Überraschungen |
| BB-5 | PATs erneuern (ab 2026-09-05: p0-read-fuer-platform-ci; 2026-10-06/2026-11-05 folgen) | Kap. 4 | Secrets in den Repo-Settings aktualisieren |
| BB-6 | Benachrichtigt-Marker T-0022 fehlt: nächsten offenen DR beobachten, ob Mail an D008-Adresse ankommt | p1/T-0022 | ggf. SMTP-Env im abschluss-Fenster prüfen (`set` vs `setx`) |

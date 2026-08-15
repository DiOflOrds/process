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
| BB-1 | ~~Copilot-PoC-Lauf~~ **geschlossen: extern blockiert** (p0/D026, 2026-08-15) — CLI installiert + eingeloggt, Integrationsstrecke real belegt, aber **Copilot-Abo abgelaufen** (revidiert D023/F13). Wiedereröffnung per CR bei neuem Abo | p1/T-0018, p0/T-0072 (beide rejected) | Evidenz: Run-Registry + p0/T-0072; Executor dabei gehärtet |
| BB-2 | ~~Checkliste „externen Dienst einrichten"~~ **erledigt** (P2/T-0010, 2026-08-15) → Kap. 8 | P1-Retro R1 | — |
| BB-3 | ~~Frist-Warnung in dr_benachrichtigung~~ **erledigt** (P2/T-0007, SWR-034/035, 2026-08-15) | P1-Retro R2 | Warnmail bei Frist ≤ 2 Tage/überschritten, mit Default-Hinweis |
| BB-4 | ~~Geräteregister Soll-Toolchain~~ **erledigt** (P2/T-0011, 2026-08-15) | P1-Retro R3 | — |
| BB-5 | PATs erneuern (ab 2026-09-05: p0-read-fuer-platform-ci; 2026-10-06/2026-11-05 folgen) | Kap. 4 | Secrets in den Repo-Settings aktualisieren; P2/T-0008: PAT braucht zusätzlich p2, process, produkt-datakonv (Katalog-CI) |
| BB-6 | ~~Mail-Zustellung beobachten~~ **erledigt** (p2/T-0001-Mail mit Marker, 2026-08-15) | p1/T-0022 | Ursache damals: Testsuite-Fehlschlag brach abschluss.cmd ab (p2/T-0002) |

## 8. Checkliste: Externen Dienst einrichten (P2/T-0010, Lehre aus dem SMTP-Erstbetrieb)

1. **Konto-Voraussetzungen prüfen** — z. B. 2FA aktivieren, BEVOR App-Passwörter/API-Keys erzeugbar sind (Gmail-Falle).
2. **Zugangsdaten erzeugen** — App-Passwort/API-Key; sofort im Passwort-Manager sichern (wird nur einmal angezeigt — PAT-Falle).
3. **Nie in Git** — Zugangsdaten nur als Umgebungsvariable (Team-Node) oder GitHub-Actions-Secret.
4. **Env richtig setzen** — Windows: `setx NAME wert` wirkt nur in NEUEN Fenstern; fürs aktuelle Fenster zusätzlich `set NAME=wert`. Werte ohne Leerzeichen (setx schneidet ab).
5. **Testlauf aus frischem Fenster** — den echten Aufrufpfad testen (z. B. `abschluss.cmd`), nicht nur das Skript isoliert.
6. **Empfänger/Ziel verifizieren** — erste echte Zustellung beim tatsächlichen Empfänger bestätigen lassen (D008-Falle: Alt-Adresse im Default).
7. **Geräteregister nachführen** — neue Umgebungsvariablen/Tools als Soll-Toolchain eintragen (Kap. 9 im Geräteregister).

## 9. Team-Node-Gate (P2/T-0015, Lehre aus T-0002/T-0013)

**Regel:** Ein G4-/Abnahme-DR wird erst gestellt bzw. beantwortet, wenn `abschluss.cmd` auf dem **Team-Node** (Windows, echte Env) grün durchgelaufen ist — Sandbox-/CI-Grün allein genügt nicht. Begründung: Beide Sprint-1-Probleme (Tests mit Mail-Seiteneffekt, CI ohne Tags) waren nur in der jeweils ungetesteten Umgebung sichtbar. Der abschluss.cmd-Lauf des Auftraggebers ist damit formell Teil des Gates, nicht nur Transportweg.

# Gold-Beispiel CM: Branching-Entscheidung (SUP.8)

**Zweck:** Referenz für dokumentierte Struktur-Entscheidungen.

## Input

Frage aus dem Team: „Dürfen Agenten direkt auf main committen? PRs kosten einen zusätzlichen Schritt je Tick."

## Referenz-Output (Eintrag CM-Strategie, Abschnitt Branching)

Regel: Agent-Ergebnisse gehen als Branch `feature/T-xxxx-<kurzname>` + PR; main bleibt geschützt. Ausnahmen (direkt auf main erlaubt): (a) BOARD.md-Regenerierung und Ticket-Statusfelder durch die Skript-Route, (b) Run-Registry-Appends — beides deterministische, validierte Skript-Outputs ohne Urteilsanteil. Begründung: Vier-Augen-Prinzip gilt für urteilsbehaftete Artefakte (Playbook Kap. 1.4); Skript-Route ist durch board.py-Validierung + CI abgesichert; PRs für Board-Hygiene würden je Tick einen Mensch/Zweitrollen-Schritt erzwingen und den autonomen Betrieb brechen.

Konsequenz: Branch-Protection main = PR-Pflicht mit Ausnahme-Pfadliste (`BOARD.md`, `tickets/*.md` nur Statusfelder, `management/runs/*.jsonl`); Verstöße meldet der CI-Check.

## Warum Gold

Entscheidung mit Begründung und expliziter, enger Ausnahmeliste statt Pauschalregel; Automatisierbarkeit (CI-Check) mitgedacht.

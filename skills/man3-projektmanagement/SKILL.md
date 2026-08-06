# SKILL: MAN.3 Projektmanagement (v1, Sprint 1, T-0002)

Prozessziel (ASPICE 4.0): Projekt definieren, planen, überwachen und steuern; Ressourcen und Beteiligte koordinieren. Rolle: PL.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Plausibilitäts-Review gegen die öffentliche PAM-4.0-Prozessstruktur: T-0017 (2026-08-06). Konformitätsanspruch pragmatisch (D010) — ein Wortlaut-Abgleich mit dem lizenzierten PAM wird nicht beansprucht:

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| Arbeitsumfang definieren | Projektauftrag + Backlog (`p0/backlog.md`); Sprint-Ziel je Sprint |
| Lebenszyklus/Vorgehen festlegen | Sprint-Zyklus 1 Woche, Ticks, Gates G0–G4 (Playbook Kap. 4) |
| Machbarkeit bewerten | je Sprint-Planning: Kapazität = Kosten-Budget vs. Backlog-Schätzung |
| Aktivitäten definieren, überwachen, anpassen | Sprint-Backlog als Tickets (Rolle, Prio, blocked_by); Sync-Ticks = Überwachung/Neuverteilung *(ergänzt T-0017)* |
| Aufwände/Ressourcen schätzen und verfolgen | Schätzung je Ticket (Kostenklasse), Ist aus Run-Registry |
| Fähigkeiten/Wissen sicherstellen | Rollen-Registry (Besetzung, Provider-Kette), Wissensbasen |
| Schnittstellen/Commitments verfolgen | Blocker-Links (`blocked_by`), Review-Zuordnungen, DR-Fristen |
| Terminplan definieren und verfolgen | Sprint-Backlog + Prio; Board-Status je Tick |
| Konsistenz sicherstellen | Plan ↔ Tickets ↔ Risikoliste ↔ Report konsistent (Board generiert) |
| Fortschritt berichten | Sprint-Report an Mensch (G4 mit 48-h-Einspruchsfrist) |

## Arbeitsschritte je Ticket-Typ

**Sprint Planning (Sprint-Tag 1):**
1. `board.py --check` laufen lassen; Backlog und Risikoliste lesen.
2. Items nach Prio und Budget ziehen; je Item Tickets mit `rolle`, `prozess`, `prio`, `blocked_by` anlegen (Template `templates/issues/task.md`).
3. Klärungsbedarf als einen gebündelten Decision Request formulieren.
4. Sprint-Ziel in `management/sprint-<n>/ziel.md`; BOARD.md generieren; committen.

**Sync-Tick:**
1. Board lesen; `blocked`-Tickets prüfen: Blocker noch valide? Eskalation nötig?
2. Kostenstand aus Run-Registry gegen Budget prüfen (Schwelle 80 % → Warnung in Report/DR).
3. Statusänderungen committen (board.py, BOARD.md mitcommitten).

**Decision Request (Template `templates/issues/decision-request.md`):**
Sachverhalt in 5 Sätzen · Optionen mit Konsequenzen (Aufwand, Risiko, ASPICE-Wirkung) · Empfehlung mit Begründung · Frist · Default nur bei risikoarm. Ergebnis ins Decision Log.

**Sprint Review / Report:**
Gliederung: Sprint-Ziel und Zielerreichung · gelieferte Artefakte (Links) · Ticket-Bilanz (done/offen/zurückgegeben mit Grund) · Kosten (je Ticket und gesamt, aus Run-Registry) · Risiken (Delta) · QM-Abschnitt ungefiltert · anstehende Entscheidungen · Ausblick.

**Risikoliste (je Sprint mindestens einmal):**
Je Risiko: Beschreibung, Wirkung, Wahrscheinlichkeit, Maßnahme, Eigentümer, Status. Neue Risiken aus Problemen/Retro ableiten; kritische → DR.

## Verweise

Templates: `templates/issues/` (task, decision-request, problem) · Checklisten: `checklists/` (DoD Sprint, folgt Sprint 2) · Guardrails: `platform/orchestrator/config/guardrails.yaml` · Registry: `roles/registry.yaml`.

## Gold-Beispiele (Wissensbasis)

`knowledge/pl/gold-beispiele/gb-01-sprint-planning.md`, `gb-02-decision-request.md`, `gb-03-sprint-report.md` — Referenz-Ein-/Ausgaben; Regressionstest vor jedem Wissensbasis-Update.

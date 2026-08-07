# Intake-Workflow: Neues Projekt anlegen (v1, Sprint 6, T-0070)

*Vom Projektwunsch zum laufenden Projekt. Multi-Projekt ist gesetzt (F8/D023: parallel von Anfang an) — je Projekt ein eigenes Repo nach p0-Muster; Plattform-Ausbau für projektübergreifende Sichten steht im P1-Backlog.*

## Schritte

1. **Auftrag erfassen:** Projektwunsch als Kurzbeschreibung (Was, Warum, Zielprodukt-Typ, Vertraulichkeit) — Eingangsparameter aus den F-Antworten: Zielprodukte SW/Embedded (F6/D023), Nutzerkreis (F9), Vertraulichkeit (F10), Domäne (F11).
2. **Projekt-Hülle anlegen:** Repo `p<N>` nach Muster (README, `tickets/`, `management/{decisions,runs,sprint-0}/`, `backlog.md`, board-check-Workflow); GitHub-Repo legt der Mensch an, Skelett liefert das Team (Muster: `p1/`).
3. **Registrierung:** Projekt in `produkte.yaml`-Logik analog aufnehmen (Matrix/CI je Produkt-Repo), Rollen-Registry gilt teamweit; Guardrails/Budget je Projekt prüfen (D003-Mechanik).
4. **G0:** Projektauftrag als erstes Dokument, Freigabe durch den Menschen → Decision Log D000 des neuen Projekts.
5. **Sprint-0-Planning** durch PL — ab hier greift das Playbook unverändert.

## Definition of Done (Intake)

Repo-Hülle valide (board-check grün auf leerem Board), Auftrag mit G0 freigegeben, Eintrag im Projektstatus, erster Sprint-Plan committet.

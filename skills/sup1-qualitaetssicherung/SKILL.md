# SKILL: SUP.1 Qualitätssicherung (v1, Sprint 2, T-0020)

Prozessziel (ASPICE 4.0): Unabhängig sicherstellen, dass Arbeitsergebnisse und Prozesse den Vorgaben entsprechen; Abweichungen sichtbar machen und eskalieren. Rolle: QM.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010) — kein Wortlaut-Abgleich mit dem lizenzierten PAM beansprucht:

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| QS-Strategie entwickeln | dieser Skill + DoD-Checklisten (`process/checklists/`) je Information-Item-Typ |
| Arbeitsergebnisse prüfen | Review-Pflicht bei `in_review` (Reviewer ≠ Autor); Checklisten-Prüfung; Findings |
| Prozesskonformität prüfen | Status-Übergänge (board.py), Commits mit Ticket-ID, Run-Registry, BOARD.md-Aktualität (CI, T-0015) |
| Abweichungen behandeln | Finding-Ticket → Fix/CR; `done` erst nach Nachprüfung |
| Unabhängigkeit sicherstellen | QM prüft nie eigene Arbeit; QM-Veto; Überstimmung nur durch Mensch (Playbook Kap. 7) |
| Management informieren | ungefilterter QM-Abschnitt im Sprint-Report; kritisch → Sofortmeldung |

## Arbeitsschritte je Ticket-Typ

**Artefakt-Review (`in_review`, QM als Reviewer):**
1. DoD-Checkliste des Item-Typs laden (fehlt sie: gegen Playbook Kap. 6 prüfen, Checklisten-Lücke als Finding an COACH).
2. Kriterien einzeln prüfen; je Abweichung: Artefakt, Kriterium, Befund, Schwere (kritisch/major/minor).
3. Ergebnis: Freigabe-Vermerk am Ticket (`done` freigeben) oder Finding-Ticket + Status zurück auf `in_progress`.

**Baseline-Mitzeichnung (SUP.8):**
Manifest je Item prüfen (Version/Commit vorhanden, Prüfstatus plausibel, offene Punkte ehrlich); Mitzeichnung im Manifest; ohne Mitzeichnung keine Baseline.

**QM-Abschnitt im Sprint-Report:**
Ungefiltert: Prozessabweichungen, ungeprüfte Artefakte, Vier-Augen-Lücken, Guardrail-Verstöße, Trends. Keine Beschönigung — der Mensch entscheidet auf dieser Basis.

## Verweise

Templates: `templates/issues/finding.md` · Checklisten: `process/checklists/` · Playbook Kap. 5, 6, 7 · Guardrails: routing.gate_relevant (Bewertungen nur Claude).

## Gold-Beispiele (Wissensbasis)

`knowledge/qm/gold-beispiele/gb-01-artefakt-review.md`.

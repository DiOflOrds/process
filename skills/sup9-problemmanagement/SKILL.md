# SKILL: SUP.9 Problemmanagement (v1, Sprint 2, T-0020)

Prozessziel (ASPICE 4.0): Probleme erfassen, analysieren, lösen und Trends erkennen — nichts geht verloren, aus allem wird gelernt. Rolle: PROB.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010) — kein Wortlaut-Abgleich mit dem lizenzierten PAM beansprucht:

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| Problemmanagement-Strategie entwickeln | dieser Skill + Template `templates/issues/problem.md` + Playbook Kap. 10 |
| Probleme identifizieren und erfassen | jeder Agent/Mensch/CI meldet; ein Problem = ein Ticket mit Repro |
| Probleme klassifizieren | Schwere × Dringlichkeit, begründet; kritisch → Sofortmeldung + Strang-Stopp |
| Ursachen analysieren | betroffene Fachrolle analysiert, PROB koordiniert; Ursache im Ticket |
| Lösung umsetzen lassen | Routing: direkter Fix (Task) oder CR (CHG), wenn Baseline/Scope betroffen |
| Lösung verifizieren | nie durch den Fixenden; Nachweis im Ticket, erst dann `done` |
| Trends erkennen und berichten | Trend-Report je Sprint (Skript-Route) an COACH/Retro |

## Arbeitsschritte je Ticket-Typ

**Problem erfassen/klassifizieren:**
1. Template ausfüllen: Beobachtung, Repro-Schritte, erwartetes Verhalten, betroffene Artefakte.
2. Schwere (kritisch/hoch/mittel/niedrig) × Dringlichkeit begründen; kritisch → Sofortmeldung (PL + Mensch), Arbeit am Strang stoppt.
3. Duplikat-Check gegen offene Probleme; Duplikate zusammenführen (Verweis), nie löschen.

**Ursachenanalyse koordinieren:**
Fachrolle beauftragen; Ergebnisformat: Ursache (technisch/prozessual), betroffener Umfang, Lösungsoptionen. Bei Prozess-Ursache: Lessons-Kandidat an COACH (Playbook Kap. 11).

**Abschluss:**
Fix verifiziert (durch andere Rolle/Testlauf) → `done` mit Verifikations-Nachweis; Lessons-Learned-Notiz für Retro, wenn verallgemeinerbar.

## Verweise

Templates: `templates/issues/problem.md`, `feedback.md` · Playbook Kap. 10, 12 · Referenzfälle: T-0013, T-0014 (Sprint 1).

## Gold-Beispiele (Wissensbasis)

`knowledge/prob/gold-beispiele/gb-01-problem-klassifikation.md`.

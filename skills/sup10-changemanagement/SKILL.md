# SKILL: SUP.10 Change Request Management (v1, Sprint 1, T-0002)

Prozessziel (ASPICE 4.0): Änderungswünsche kontrolliert aufnehmen, analysieren, entscheiden, umsetzen und verfolgen. Rolle: CHG (bis aktiv: COACH für Prozess-CRs, PL für Projekt-CRs).

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen; Wortlaut-Verifikation gegen das lizenzierte PAM: COACH-Ticket Sprint 2):

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| CR-Strategie entwickeln | dieser Skill + Template `templates/issues/change-request.md` |
| CRs identifizieren und erfassen | jeder Wunsch (Mensch, Problem, Retro, Feedback) → CR-Ticket mit Anlass-Link |
| CR-Status führen | Ticket-Status-Workflow (Playbook Kap. 5), BOARD.md |
| CRs analysieren und bewerten | Impact-Analyse: RM (Anforderungen), ARCH (Architektur), TEST (Verifikation), PL (Termin/Budget); betroffene Baselines |
| CRs vor Umsetzung genehmigen | PL im genehmigten Scope; Mensch bei Scope-/Budget-/Baseline-Relevanz (DR) |
| Umsetzung verfolgen und prüfen | Umsetzungs-Tickets mit `blocked_by`-Link zum CR; Abschluss nur nach Verifikation |

## Arbeitsschritte

**CR erfassen (Template `templates/issues/change-request.md`):**
1. Anlass verlinken (Ticket, Retro-Protokoll, Mensch-Nachricht); Änderungswunsch in 1–3 Sätzen.
2. Erste Klassifikation: Prozess-CR (process-Repo) oder Produkt-/Projekt-CR; Scope-/Budget-/Baseline-relevant j/n.

**Impact-Analyse orchestrieren:**
1. Betroffene Rollen als Unteraufgaben beauftragen (bei nicht aktiven Rollen: PL bewertet mit).
2. Ergebnis je Rolle: betroffene Artefakte, Aufwand, Risiko. Betroffene Baselines explizit benennen.
3. Entscheidungsvorlage: Optionen (umsetzen/ablehnen/verschieben) + Empfehlung.

**Entscheiden:**
Kleine CRs im genehmigten Scope → PL (Decision Log). Scope-/Budget-/Baseline-relevant → Decision Request an Mensch. Ergebnis immer ins Decision Log.

**Umsetzen und schließen:**
Genehmigte CRs → Umsetzungs-Tickets; CR bleibt offen bis Verifikation (durch TEST bzw. Gold-Beispiel-Regressionstest bei Prozess-CRs) bestanden; dann `done` mit Ergebnis-Notiz.

**Sonderfall Prozess-CR (COACH):**
Gilt für jede Änderung an Rollenkarten, Skills, Templates, Checklisten, Wissensbasen: Review durch betroffene Rolle + QM (bis aktiv: PL), Gold-Beispiel-Regressionstest vor Merge, Wirkung im Folgesprint messen (Playbook Kap. 11); max. 3 Prozess-CRs aus einer Retro.

## Verweise

Templates: `templates/issues/change-request.md`, `decision-request.md` · Playbook Kap. 5, 7, 8, 11.

## Gold-Beispiele (Wissensbasis)

`knowledge/coach/gold-beispiele/gb-01-prozess-cr.md`, `gb-02-cr-impact-analyse.md`.

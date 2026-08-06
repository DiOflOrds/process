# SKILL: SWE.1 Software-Anforderungsanalyse (v1, Sprint 2, T-0020)

Prozessziel (ASPICE 4.0): Stakeholder-/Systemanforderungen in konsistente, bewertete, verfolgbare SW-Anforderungen überführen. Rolle: RM (deckt in Stufe 1 auch SYS.1 Elizitation ab).

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010) — kein Wortlaut-Abgleich mit dem lizenzierten PAM beansprucht:

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| SW-Anforderungen spezifizieren | `requirements/software/` — SWR-xxx, atomar, testbar, Englisch (D011) |
| Anforderungen strukturieren/kategorisieren | Gruppierung nach Komponente; Attribute: prio, status, source, verification |
| Anforderungen analysieren (Korrektheit, Machbarkeit, Verifizierbarkeit) | Reviews: ARCH/DEV-Kontext (machbar), TEST/QM (prüfbar) vor Baseline |
| Auswirkungen auf Betriebsumgebung analysieren | Abschnitt Constraints/Environment je Set (Sandbox, Team-Node, VM, Guardrails) |
| Priorisieren für Releases | prio-Attribut + Sprint-Zuordnung im Backlog |
| Konsistenz und Traceability sicherstellen | Trace-Tabelle STK ↔ SWR im Set; ab Sprint 3 CI-generierte Matrix |
| Anforderungen vereinbaren/kommunizieren | G1-Vorlage an Mensch (Baseline-Freigabe); Clarifications gebündelt |

## Arbeitsschritte je Ticket-Typ

**Stakeholder-Anforderungen erheben (SYS.1-Anteil):**
1. Quellen lesen: Masterplan, Projektbeschreibung, Decision Log, Mensch-Nachrichten.
2. Je Anforderung: `STK-xxx`, Titel, Beschreibung (was/warum), Quelle, Priorität. Unklar → Clarification (gebündelt).

**SW-Anforderungen ableiten:**
1. Je STK prüfen: welche SW-/Plattform-Funktionen folgen daraus? Je Funktion ein `SWR-xxx`: „shall"-Formulierung, atomar, testbar, Verifikationskriterium (wie wird es geprüft?), Trace auf STK.
2. DoD-Checkliste „SW-Anforderung" anwenden (Playbook Kap. 6); Reviews einholen; Status im Set führen (draft → reviewed → baselined).

**G1-Vorlage (gate-relevant):**
Set-Übersicht, Review-Status je Anforderung, offene Clarifications, Empfehlung; Freigabe = Anforderungs-Baseline (Tag durch CM).

## Verweise

Templates: `templates/issues/clarification.md`, `task.md` · Playbook Kap. 3, 6 · Registry: aufgaben_typen RM.

## Gold-Beispiele (Wissensbasis)

`knowledge/rm/gold-beispiele/gb-01-swr-anforderung.md`.

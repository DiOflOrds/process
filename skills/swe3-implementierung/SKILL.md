# SKILL: SWE.3 Software-Implementierung (v1, Sprint 3, T-0029)

Prozessziel (ASPICE 4.0): Software-Einheiten gemäß Architektur und Anforderungen entwickeln, verifizierbar und nachvollziehbar. Rolle: DEV.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010):

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| Einheiten entwickeln | Implementierung je Ticket mit SWR-/CR-Bezug (T-0025); Architektur/ADRs sind bindend |
| Einheiten verifizieren | Unit-Tests neben dem Code, Docstring trägt die verifizierte SWR-ID (T-0026); Suite grün vor Review |
| Traceability sicherstellen | Commit referenziert Ticket-ID; Test referenziert SWR; Matrix per `trace_matrix.py` |
| Änderungen beherrschen | Branch `feature/t-xxxx-<slug>`, selektives Staging (nur eigenes Delta, Lesson T-0014); Merge nur nach Review + grüner CI |

## Arbeitsschritte je Implementierungs-Ticket

1. Preflight (T-0024). Ticket lesen: SWR-/CR-Bezug vorhanden? Nein → QM-Finding, zurück.
2. Architektur/ADRs lesen; Abweichungsbedarf ist ein CR an ARCH, kein stiller Workaround.
3. Klein implementieren: eine Einheit, ein Zweck; Wiederkehrendes als Skript-Route kennzeichnen (Automatisierungspyramide).
4. Tests zuerst oder parallel: je berührter SWR mindestens ein Testfall mit Docstring-ID; Fehlerfälle mittesten.
5. Selbstcheck gegen DoD „Code/Unit done" (Playbook Kap. 6), dann `in_review` mit benanntem Reviewer ≠ Autor.

## Qualitätskriterien (DoD-Auszug)

Code ist `done`, wenn: Review bestanden; CI grün; SWR-Trace in Tests; Doku/README angepasst, wo Verhalten sichtbar ist; keine fremden Deltas im Commit.

## Gold-Beispiele

`knowledge/dev/gold-beispiele/` — Referenzfälle aus realen Tickets (ab T-0032).

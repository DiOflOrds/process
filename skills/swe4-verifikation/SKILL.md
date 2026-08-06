# SKILL: SWE.4–SWE.6 Software-Verifikation und -Integration (v1, Sprint 3, T-0029)

Prozessziel (ASPICE 4.0): Nachweisen, dass Einheiten (SWE.4), integrierte Software (SWE.5) und das Gesamtprodukt gegen seine Anforderungen (SWE.6) verifiziert sind. Rolle: TEST. In Stufe 1 gemeinsam geführt; Aufwuchs später.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010):

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| Verifikationsstrategie festlegen | `p0/verification/strategie.md`: Testebenen, Abdeckungsanspruch, Automatisierung, Umgebungen |
| Testfälle spezifizieren | Je SWR-Verifikationskriterium mindestens ein Testfall; Fehlerfälle ausdrücklich |
| Verifikation durchführen | Skript-Route: `python -m unittest discover tests` lokal + CI; manuelle Nachweise dokumentiert im Testbericht |
| Ergebnisse bewerten und berichten | SWR↔Test-Matrix (`trace_matrix.py`) nach `p0/verification/reports/`; Lücken mit Plan, kein Gefälligkeitsgrün |
| Regression sicherstellen | Volle Suite je Push (CI, T-0015); rote Stände sofort als SUP.9-Problem |
| Traceability sicherstellen | Docstring-SWR-IDs (T-0026); Matrix ist der Nachweis |

## Arbeitsschritte

**Testfall-Ableitung (je reviewed-SWR):** Verifikationskriterium lesen → prüfbare Assertion formulieren → automatisieren (Unit/API) oder manuellen Nachweis mit Schritten/Erwartung dokumentieren.

**Verifikationslauf (Skript-Route):** Preflight → Suite → Matrix generieren → Bewertung: neue Lücken? Abdeckung je Ziel-SWR? → Ergebnis an Ticket/Sprint-Report.

**Abnahme vor Gates (SWE.6-Anteil):** Gegen die Abnahmekriterien des Gates prüfen (z. B. G4: Deliverables des Sprintziels erreichbar/nachgewiesen); Abweichungen ehrlich listen.

## Qualitätskriterien (DoD-Auszug)

Verifikation ist `done`, wenn: Matrix generiert und bewertet; jede Ziel-SWR abgedeckt oder als Lücke mit Plan gemeldet; Bericht am Ticket; rote Stände als Problem-Ticket erfasst.

## Gold-Beispiele

`knowledge/test/gold-beispiele/` — Referenzfälle aus realen Tickets (ab T-0034).

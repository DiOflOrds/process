# Gold-Beispiel RM: SW-Anforderung (Referenz)

**Eingabe:** Stakeholder-Anforderung STK-004 „Hard cost limits" (aus D003: Testbudget ~20 €, hartes Limit mit Abschaltung ab dem ersten Tick).

**Erwartete Ausgabe (eine atomare, testbare SWR, Englisch per D011):**

```
ID: SWR-012
Title: Per-tick cost abort
Requirement: When the accumulated cost of a single tick exceeds the configured
per-tick limit (guardrails.yaml: budget.limit_tick_eur), the orchestrator shall
abort the tick before issuing further LLM calls and shall record the abort with
reason "tick budget exceeded" in the run registry.
Source: STK-004 (D003) · Priority: high · Status: reviewed
Verification: Unit test with a mocked executor whose reported cost exceeds the
limit; assert abort, no further calls, and a run-registry entry with the reason.
```

*Merkmale, die das Beispiel zeigt: „shall"-Form, ein prüfbares Verhalten pro Anforderung (Abbruch + Log als ein Ereignispfad), konkrete Konfigurationsreferenz, Quelle als Trace, Verifikationskriterium als ausführbarer Test — nicht „wird getestet".*

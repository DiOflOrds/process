# Gold-Beispiel ARCH: ADR zur Technologiewahl (SWE.2)

**Zweck:** Referenz für Architektur-Entscheidungen mit bewusster Abweichung vom Plan. Quelle: realer Fall ADR-001 (T-0031, Sprint 3).

## Input

P0 nennt FastAPI als Backend-Beispiel. Plattform ist abhängigkeitsarm (Lesson T-0027: schon pyyaml verursachte CI-Drift); Betriebsorte heterogen (Windows-Node, Sandbox ohne Paketzugriff, künftige VM). API-Umfang: 5 GET + 1 POST.

## Referenz-Output (Kurzform ADR-001)

Kontext (2 Sätze, mit Lesson-Referenz) → 2 ernsthafte Optionen mit ehrlichen Nachteilen beider → Entscheidung: stdlib, begründet über Betriebsrobustheit statt Komfort → Konsequenzen inkl. Rückweg (Migration möglich, Wiedervorlage-Trigger benannt). Abweichung vom Plan ausdrücklich in der G2-Vorlage markiert („bitte bestätigen").

## Warum Gold

Abweichung versteckt sich nicht, sondern wird zum Gate getragen; Optionen sind echt (kein Strohmann); Konsequenzen enthalten den Preis der eigenen Wahl (Handarbeit Routing) und den Exit-Pfad; Lesson aus früherem Sprint fließt als Evidenz ein.

# Gold-Beispiel TEST: Abnahme mit ehrlichen Restpunkten (SWE.6)

**Zweck:** Referenz für dokumentierte manuelle Abnahmen ohne Gefälligkeitsgrün. Quelle: realer Fall `ui-abnahme-swr-021.md` (T-0034, Sprint 3).

## Input

SWR-021 (Frontend) ist nicht unit-testbar abgedeckt; Abnahme muss manuell erfolgen, Sandbox hat kein echtes Smartphone.

## Referenz-Output (Muster)

Tabelle Kriterium → Schritt → Ergebnis, je Kriterium einzeln; API-verifizierbare Anteile auf den automatisierten Test verwiesen statt am echten System ausgelöst (keine Testentscheidung ins echte Decision Log); nicht prüfbare Anteile als **ausstehend mit Adressat** markiert (Gerätetest → Mensch im Sprint-Review) statt still als bestanden; Scope-Grenzen mit ADR-Referenz. Bewertung nennt Restpunkte ausdrücklich und erklärt, warum sie keine Blocker sind.

## Warum Gold

Abnahme unterscheidet sauber: automatisiert belegt / manuell belegt / ausstehend-mit-Adressat; das echte System bleibt unverschmutzt; die Matrix zeigt den Nachweistyp transparent („manuelle Abnahme dokumentiert").

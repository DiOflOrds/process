# SKILL: SUP.8 Konfigurationsmanagement (v0 — Stub, wird in Sprint 1 ausgearbeitet)

Prozessziel (ASPICE 4.0): Integrität aller Arbeitsergebnisse über den Lebenszyklus sicherstellen und Baselines verfügbar machen.

## Kernpraktiken für den CM-Agenten (Kurzfassung)
1. CM-Strategie pflegen: identifizierte Konfigurationselemente (Repos, Pfade, Information-Item-Typen), Branching-Modell (main geschützt, Feature-Branches, MR-Pflicht), Namenskonventionen, Storage-Locations, Tool-Liste.
2. Baselines erstellen: Git-Tag(s) + generiertes Manifest (Item, Version/Commit, Prüfstatus QM, offene Punkte); Anlässe It. Playbook Kap. 9.
3. Branch-/Rechte-Schutz technisch durchsetzen; Geräteregister pflegen (Team-Nodes: Token, Fähigkeiten, Rechte, Onboarding nur mit Mensch-Freigabe).
4. Backup und Wiederherstellbarkeit: regelmäßige Sicherung von Repos, Backend-DB, Konfiguration; Restore-Test dokumentieren (Runbook).
5. Konsistenz sichern: keine Konfigurationselemente außerhalb der definierten Storage-Locations (CI-Check).

## Zu erzeugende Information Items
CM-Strategie, Baseline-Manifeste, Tool-/Storage-Übersicht, Geräteregister, Betriebs-Runbook.

*Ausarbeitung folgt in Sprint 1 als Prozess-CR.*

# Gold-Beispiel QM: Artefakt-Review mit Finding (Referenz)

**Eingabe:** Ticket T-0010 `in_review`, Artefakt `cm/cm-strategie.md` (v1, von ollama/gemma3:27b erzeugt), DoD-Referenz: SUP.8-Skill Pflichtinhalte 1–7.

**Erwartete Ausgabe (Review-Ergebnis):**

Geprüft gegen Pflichtinhalte der CM-Strategie:
- Inhalt 1 (Konfigurationselemente): **Finding, major** — Tabelle nennt Rollen `RE`, `TECHWRITER`, `QA`, die es laut `roles/registry.yaml` nicht gibt, und Pfade (`project/`, `backend/`), die in keinem Repo existieren. Kriterium: Item-Liste muss der Artefakt-Landkarte (Playbook Kap. 3) und den realen Repos entsprechen.
- Inhalt 2 (Branching): bestanden (main geschützt, feature/T-xxxx, PR-Pflicht).
- Inhalt 3 (Namenskonventionen): bestanden.
- Inhalt 6 (Backup): **Finding, minor** — Restore-Test behauptet, aber kein Nachweis-Verweis.

**Entscheidung:** keine Freigabe; Status zurück auf `in_progress` oder Nacharbeits-Ticket (hier: T-0018). Findings konkret (Artefakt, Kriterium, Befund, Schwere) — keine Pauschalurteile.

*Lehre: Ein strukturell sauberes Artefakt kann inhaltlich generisch sein — immer gegen die reale Projekt-Landkarte prüfen, nicht nur gegen die Gliederung.*

# CM-Strategie (v1)

## Konfigurationselemente

| Item-Typ | Repo/Pfad | Eigentümer | Beschreibung |
|---|---|---|---|
| Projektplan | `project/` | PL | Gesamtplanung, Meilensteine |
| Anforderungsdokument | `requirements/` | RE | Detaillierte Anforderungen |
| Architekturdesign | `architecture/` | ARCH | Systemarchitektur und Designentscheidungen |
| Code (Backend) | `backend/` | DEV | Backend-Implementierung |
| Code (Frontend) | `frontend/` | DEV | Frontend-Implementierung |
| CI/CD Pipeline Definitionen | `.github/workflows/` | CM | GitHub Actions Workflows |
| Dokumentation | `docs/` | TECHWRITER | Benutzerhandbücher, API-Dokumentation |
| Testfälle | `tests/` | QA | Unit-, Integrations- und Systemtests |
| Infrastruktur als Code (IaC) | `infrastructure/` | PROB | Terraform oder ähnliche Konfigurationen |
| Baseline Manifeste | `baselines/` | CM | Beschreibt den Inhalt jeder Baseline |
| Geräteregister | `process/cm/geraeteregister.md` | CM | Liste aller Team-Geräte und deren Konfiguration |

## Branching-Modell

*   **main:** Geschützter Zweig für stabile Releases. Direkte Commits nicht erlaubt.
*   **feature/\<T-xxxx>-<kurzname>:**  Entwicklungszweige für neue Features oder Bugfixes. Benannt nach Ticket-ID und einer kurzen Beschreibung.
*   **release/\<projekt>-v\<x.y>:** Vorbereitungszweig für Releases. Wird aus `main` erstellt, enthält Release-spezifische Änderungen (Versionsnummern etc.).
*   **hotfix/\<T-xxxx>-<kurzbeschreibung>:** Für kritische Bugfixes in Produktion. Wird direkt aus `main` erstellt und nach dem Fix wieder gemerged.

**Merge Requests/Pull Requests (PR):**  Alle Änderungen müssen über PRs erfolgen, um Code Reviews zu ermöglichen. Ausnahmen sind nur mit Skript-Routing möglich (siehe Tool-Liste).

## Namenskonventionen

*   **Tickets:** T-nnnn (n = fortlaufende Nummer)
*   **Baselines:** `<projekt>-v<major.minor>` (z.B. `myproject-v1.0`)
*   **Branches:** Siehe Branching-Modell.
*   **Commits:**  Beginnen mit der Ticket-ID (z.B. `T-0010: Fix bug in user authentication`).

## Storage-Locations

| Item-Typ | Speicherort | Zugriff | Backup |
|---|---|---|---|
| Code, Dokumentation, Tests | GitHub Repository (`D005`) | Teammitglieder mit entsprechenden Rechten | GitHub + Lokale Klone (siehe Backup-/Restore-Konzept) |
| Infrastruktur als Code | GitHub Repository (`D005`) | PROB, CM | GitHub + Lokale Klone |
| Baseline Manifeste | `baselines/` im jeweiligen Repository | CM, QM | GitHub + Lokale Klone |
| Geräteregister | `process/cm/geraeteregister.md` im Repository | CM, PL | GitHub + Lokale Klone |

## Tool-Liste

*   **Git:** Versionskontrolle
*   **GitHub (D005):** Git Hosting und Kollaboration
*   **board.py:** Ticket-Management und Reporting
*   **CI System:** Automatisierte Builds, Tests und Deployments (Details in separater Dokumentation)
*   **Skript-Routing für PRs:**  Automatisierung von PR-Reviews basierend auf Commit-Nachrichten/Ticket-IDs. Konfiguration in `.github/workflows/`.

## Backup-/Restore-Konzept

*   **GitHub:** GitHub bietet automatische Backups und Disaster Recovery.
*   **Lokale Klone:** Jedes Teammitglied sollte eine lokale Kopie des Repositories haben, um im Notfall schnell wiederherstellen zu können.
*   **Regelmäßige Backups:**  Automatisierte Skripte erstellen regelmäßig Backups der wichtigsten Konfigurationselemente und speichern sie an einem sicheren Ort (Details in `process/cm/runbook.md`).
*   **Restore-Test:** Das Restore-Konzept wird regelmäßig getestet, um sicherzustellen, dass es funktioniert.

## Geräteregister-Verweis

Siehe `process/cm/geraeteregister.md`.

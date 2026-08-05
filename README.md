# process — Prozess-Repo des virtuellen ASPICE-Teams

Single Source of Truth für alles, was die **Arbeitsweise** des Teams definiert. Änderungen an diesem Repo sind Prozessänderungen und laufen als Change Request (SUP.10) mit Review. Eigentümer: COACH.

## Struktur

| Pfad | Inhalt |
|---|---|
| `docs/` | Masterplan, Team-Playbook (Prozess-Baseline) |
| `roles/` | Rollen-Registry und Rollenkarten (= System-Prompts der Agenten) |
| `skills/` | Prozess-Skills je Prozessgebiet (werden Agenten zur Laufzeit geladen) |
| `templates/` | Issue-Templates und Label-Schema (werden per Skript in Projekt-Repos synchronisiert: `.gitlab/issue_templates/`) |
| `checklists/` | DoD-/QM-Checklisten je Information-Item-Typ (ab Sprint 2) |
| `knowledge/` | Wissensbasen der Rollen (Lernzyklus, Playbook Kap. 11) |

## Baseline-Stand

- `genesis-v0.1` (Sprint 0): Masterplan 0.4, Playbook 0.4, P0-Beschreibung 0.4, Repo-Grundgerüst.

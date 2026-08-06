# SKILL: SUP.8 Konfigurationsmanagement (v1, Sprint 1, T-0002)

Prozessziel (ASPICE 4.0): Integrität aller Arbeitsergebnisse über den Lebenszyklus sicherstellen und Baselines verfügbar machen. Rolle: CM.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Plausibilitäts-Review gegen die öffentliche PAM-4.0-Prozessstruktur: T-0017 (2026-08-06). Konformitätsanspruch pragmatisch (D010) — ein Wortlaut-Abgleich mit dem lizenzierten PAM wird nicht beansprucht:

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| CM-Strategie entwickeln | `process/cm/cm-strategie.md` (Elemente s. u.) |
| Konfigurationselemente identifizieren | Artefakt-Landkarte Playbook Kap. 3 = Item-Liste mit Storage-Location |
| CM-System etablieren | Git (GitHub, D005), geschützte main-Branches, PR-Pflicht |
| Branch-Management etablieren | Branching-Modell in der CM-Strategie (main geschützt, `feature/T-xxxx-*`) |
| Änderungen kontrollieren | Nur via Ticket + PR; Commits referenzieren Ticket-IDs |
| Baselines etablieren | Git-Tag(s) + Manifest je Anlass (Playbook Kap. 9) |
| Konfigurationsstatus berichten | BOARD.md + Baseline-Manifeste; Abschnitt im Sprint-Report |
| CM-Informationen verifizieren | board.py --check je Push (CI, T-0015); Prüfstatus im Baseline-Manifest; QM-Mitzeichnung *(ergänzt T-0017)* |
| Ablage/Backup verwalten | Storage-Locations-Tabelle, Backup-/Restore-Runbook mit Testnachweis |

## Arbeitsschritte je Ticket-Typ

**CM-Strategie erstellen/pflegen (`process/cm/cm-strategie.md`):**
Pflichtinhalte: 1) Konfigurationselemente (Tabelle: Item-Typ, Repo/Pfad, Eigentümer — aus Playbook Kap. 3), 2) Branching-Modell (main geschützt; Arbeitsbranches `feature/T-xxxx-<kurzname>`; Merge nur per PR mit Review ≠ Autor), 3) Namenskonventionen (Tickets T-nnnn, Baselines `<projekt>-v<major.minor>`, Branches, Commits mit Ticket-ID), 4) Storage-Locations, 5) Tool-Liste (Git, GitHub, board.py, CI), 6) Backup-Konzept (GitHub + lokale Klone; Restore-Test dokumentiert), 7) Geräteregister-Verweis.

**Baseline erstellen:**
1. Anlass prüfen (Playbook Kap. 9); QM-Mitzeichnung einholen (bis QM aktiv: PL + Mensch-Gate).
2. Tag auf betroffene Repos (`<projekt>-v<x.y>`); Manifest `baselines/<id>-manifest.md`: je Item Version/Commit, Prüfstatus, offene Punkte.
3. Manifest committen; Statusbericht im Sprint-Report.
4. Nie: Tag löschen/verschieben (guardrails: forbidden_actions).

**Geräteregister pflegen (`process/cm/geraeteregister.md`):**
Je Gerät: Name, Identität/Token-Referenz (nie der Token selbst!), OS/Toolchains, erlaubte Rollen, Rechteumfang, Verfügbarkeit, Status. Neuaufnahme nur mit Mensch-Freigabe (Decision Log).

**Betriebs-Runbook (`process/cm/runbook.md`):**
Wiederkehrende Betriebsaufgaben mit Skript-Verweis; Incidents als SUP.9-Ticket.

## Verweise

Templates: `templates/issues/task.md` · Guardrails: `platform/orchestrator/config/guardrails.yaml` (forbidden_actions, device_onboarding) · Playbook Kap. 3, 9, 13.

## Gold-Beispiele (Wissensbasis)

`knowledge/cm/gold-beispiele/gb-01-baseline-manifest.md`, `gb-02-branching-entscheidung.md`, `gb-03-geraeteregister-onboarding.md`.

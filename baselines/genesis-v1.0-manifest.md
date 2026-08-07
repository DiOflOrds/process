# Baseline-Manifest: genesis-v1.0 (P0-ABSCHLUSS)

*Erstellt 2026-08-07 (Sprint 6, CM). Anlass: **P0-Abnahme (D024)** — Bootstrap-Projekt „Genesis" gegen Kap. 3 abgenommen. Tags `genesis-v1.0` auf process, platform, p0. Produktstand: datakonv `v1.0.0`/`req-v1.1`. P1-Hülle lokal bereit.*

| Item | Version/Stand | Prüfstatus | Offene Punkte (→ P1) |
|---|---|---|---|
| Prozess-Baseline (Masterplan/Playbook/P0) | 0.6 / 0.6 / 0.5; F-Fragen bis auf F9–F11 entschieden (D023) | Baseline seit G0, Änderungen per CR | F9–F11 beim P1-G0 |
| Rollen/Skills/Wissensbasen | 10 aktive Rollen, 9 Skill-Gebiete, Lessons + Gold-Beispiele | Self-Check T-0065 | Lernzyklus regulär in P1 |
| Plattform (Board/Orchestrator/Gateway/Backend) | board.py inkl. status-Subkommando, Tick idempotent, 3+1 Executor (Copilot v1 neu), Mission Control, Guardrails | **112 Tests grün**, CI mit Matrix-Gates | Copilot-Ausführungsnachweis T-0072; B1 Multi-Projekt; B6 Frontend-Views |
| Skript-Routen | 8 Skripte (inkl. feedback_route v1.1, catalog, produkte.yaml) | je Skript Unit-Tests | — |
| Produkt datakonv | 1.0.0 released (G3/D021), 42 Tests, Matrix 18/0 | E2E + Problem-/CR-Zyklus real | Distribution/Registry (B7) |
| Betrieb | Runbook v1, Geräteregister, lokaler Betrieb (D017) | T-0067 (QM-Review) | Geräte-Onboarding bei Bedarf (F12) |
| Management-Evidenz | Sprint-Pläne/Reports/Retros 0–6, Self-Check, KPI-Baseline, Abschlussbericht, Decision Log D000–D024 | P0-Abnahme D024 | — |
| Wiederholbarkeit | Intake-Workflow v1 + P1-Hülle mit Backlog B1–B10 | T-0070 (QM-Review) | P1-G0 durch Mensch |

**QM-Mitzeichnung (2026-08-07, QM-Kontext):** Manifest gegen Repo-Stand geprüft; Abnahme-Abweichungen (Kriterien 4/5/9 teilweise) transparent im Abschlussbericht und nachverfolgt. Mitgezeichnet.

**Mensch-Freigabe: P0-Abnahme erteilt 2026-08-07 (D024).**

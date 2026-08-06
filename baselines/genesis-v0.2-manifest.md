# Baseline-Manifest: genesis-v0.2 (Prozess-/Plattform-Baseline + Anforderungs-Baseline G1)

*Erstellt 2026-08-06 (Sprint 2, T-0023, CM). Anlass: P0-Meilenstein „Prozesse & Selbstübernahme" (Sprint-2-Abschluss) + Anforderungs-Baseline G1 (D013). Tags `genesis-v0.2` auf process, platform, p0 (Commits = Tag-Referenzen der drei Repos).*

| Item | Version/Stand | Prüfstatus | Offene Punkte |
|---|---|---|---|
| Masterplan, Playbook, P0-Beschreibung | 0.6 / 0.5 / 0.5 (unverändert seit v0.1 + D005-Revision) | Baseline seit G0 | Fortschreibung mit VM-Setup (Sprint 3) |
| Rollenkarten + Registry | v1/v1.1 — 7 Rollen aktiv (PL, CM, COACH, QM, RM, PROB, CHG) | reviewed (T-0001/T-0019, G4) | ARCH/DEV/TEST-Karten Sprint 3 |
| Skills | MAN.3, SUP.8, SUP.10 v1.1; SUP.1, SWE.1, SUP.9 v1 | BP-Plausibilitätsreview T-0017 (D010) | Skills für SWE.2/3/4 ab Sprint 3 |
| Wissensbasen | CM erstbefüllt (L-001–L-004 + Heuristiken); 11 Gold-Beispiele (pl, cm, coach, qm, rm, prob) | Regressionstest bestanden (T-0016) | Erstbefüllung weiterer Rollen datengetrieben |
| CM-Strategie + Geräteregister | v1.1 / v1 | QM-Review (T-0018, G4) | Runbook + Restore-Test Sprint 3 |
| DoD-Checklisten | dod-sw-anforderung v1 | angewandt auf T-0021 | weitere Item-Typen folgen |
| board.py, Gateway, Guardrails, Orchestrator | v1 (Sprint 1), 62 Unit-Tests grün (lokal verifiziert 2026-08-06) | G4 Sprint 1 + Sprint 2 | feingranulare SWR↔Test-Matrix (T-0026) |
| CI-Workflows | ci.yml (platform), board-check.yml (p0) | YAML-validiert, lokal gleichwertige Läufe grün | erster GitHub-Actions-Lauf ausstehend (Secret PLATFORM_READ_TOKEN, Mensch) |
| **Anforderungs-Baseline (G1, D013)** | STK-001–011 + SWR-001–019 (EN, D011) | DoD-Checkliste + QM; Traceability vollständig | STK-012, SWR-020/021 draft (Sprint 3); Änderungen nur per CR |
| Tickets/Board | T-0001–T-0026; Sprint 2: 9/9 team-seitig done | board.py --check grün | T-0008 (Mensch) offen |
| Decision Log | D000–D013 (append-only) | — | — |

**QM-Mitzeichnung (2026-08-06, QM-Kontext):** Manifest gegen Repo-Stand geprüft; Prüfstatus je Item plausibel und belegt (Ticket-/Test-Referenzen); offene Punkte ehrlich ausgewiesen; keine Baseline-Blocker. Mitgezeichnet.

**Mensch-Freigabe:** G1 + G4 erteilt 2026-08-06 (D013).

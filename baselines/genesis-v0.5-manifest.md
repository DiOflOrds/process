# Baseline-Manifest: genesis-v0.5 (Sprint-5-Abschluss — E2E-Nachweis + Release)

*Erstellt 2026-08-07 (Sprint 5, CM). Anlass: G4 Sprint 5 (D022) — E2E-Nachweis vollendet: datakonv 1.0.0 released (G3/D021), realer Problem- und CR-Zyklus, Produktkatalog v0. Tags `genesis-v0.5` auf process, platform, p0; produkt-datakonv trägt `v1.0.0` + `req-v1.1`.*

| Item | Version/Stand | Prüfstatus | Offene Punkte |
|---|---|---|---|
| Masterplan, Playbook, P0-Beschreibung | 0.6 / 0.6 / 0.5 | Baseline seit G0 | — |
| **Released Produkt datakonv** | **1.0.0** (Tag), Reqs `req-v1.1` (18 SWRs), Katalog-Eintrag | 42 Tests grün, Matrix 18/0; G3/D021 | Konsolen-Skript-Installation ungetestet in CI; Secret produkt-CI ausstehend |
| Produktkatalog v0 | `process/catalog/` (products.yaml + Detailseite) + catalog.py | +2 Tests; erster Eintrag live | CI-Automatik nach P0 (bewusste Abweichung) |
| Feedback-Routing v1 | feedback_route.py (+4 Tests); Erstlauf T-0059→T-0060 korrekt | Review TEST | Auto-Abschluss (T-0063, Sprint 6) |
| Preflight | repos_im_root: Produkt-Repos einbezogen (T-0050) | +1 Test | Wirkung „0 manuelle Lock-Eingriffe" ab Sprint 6 messen |
| board.py | DR-optionen-Pflicht für neue DRs (T-0051, Bestand T-0022/35/41) | +2 Tests; fand T-0022 real | Status-Subkommando (T-0062, Sprint 6) |
| CI-Gates | platform: Matrix-Gate aktiv (Secret gesetzt); produkt: Matrix-Gate verdrahtet (T-0049) | platform grün nach Push zu prüfen | Secret `PLATFORM_READ_TOKEN` in produkt-datakonv (Mensch) |
| Verifikation | + datakonv-integrationsverifikation.md, datakonv-swr-test-matrix (18/0) | TEST/QM (T-0052) | SMTP-Erfolgspfad (seit Sprint 3) |
| Tickets/Board | T-0001–T-0064; Sprint 5: 12/12 done | board.py --check grün | Sprint-6-CRs T-0062–T-0064 offen |
| Decision Log | D000–D022 (append-only; D021 = erster maschinenlesbarer DR) | — | — |

**QM-Mitzeichnung (2026-08-07, QM-Kontext):** Manifest gegen Repo-Stand geprüft; Prüfstatus je Item mit Ticket-/Test-/Gate-Referenz belegt; offene Punkte ehrlich ausgewiesen. Keine Baseline-Blocker. Mitgezeichnet.

**Mensch-Freigabe:** G4 Sprint 5 erteilt 2026-08-07 (D022). **P0-Abnahmekriterium 1 (E2E-Nachweis) dokumentiert erfüllt.**

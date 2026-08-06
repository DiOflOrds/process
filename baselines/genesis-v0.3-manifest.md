# Baseline-Manifest: genesis-v0.3 (Prozess-/Plattform-Baseline + G1-Erweiterung + G2)

*Erstellt 2026-08-06 (Sprint 3, CM). Anlass: P0-Meilenstein „Backend & Frontend MVP" (Sprint-3-Abschluss, G4) + Anforderungs-Baseline-Erweiterung G1 (STK-012, SWR-020–024) + Architektur-Freigabe G2 (D015). Tags `genesis-v0.3` auf process, platform, p0.*

| Item | Version/Stand | Prüfstatus | Offene Punkte |
|---|---|---|---|
| Masterplan, Playbook, P0-Beschreibung | 0.6 (E5 per D014) / 0.6 (T-0025) / 0.5 | Baseline seit G0; Änderungen per CR | VM-Setup-Fortschreibung nach D014 (Sprint 4) |
| Rollenkarten + Registry | v1/v1.2 — 10 Rollen aktiv (+ ARCH, DEV, TEST) | reviewed (T-0028, G4) | Copilot-Kettenstufe ab PoC Sprint 6 (F13) |
| Skills | + SWE.2, SWE.3, SWE.4–6 v1 | BP-Mappings pragmatisch (D010); Review T-0029 | Schärfung datengetrieben per Retro |
| Wissensbasen | + 3 Gold-Beispiele arch/dev/test aus realen Sprint-3-Fällen | Review T-0029 | Lessons-Erstbefüllung arch/dev/test |
| **Architektur (G2, D015)** | architektur.md v1 + ADR-001..003 + G2-Vorlage | Review DEV/QM-Kontext; G2 erteilt (D015) | ADR-001-Wiedervorlage bei API-Wachstum (nach P0) |
| Backend/Frontend-MVP | backend/ (Server, Aggregation, Inbox, Mailer, PWA) + infra/ | 81 Tests grün; UI-Abnahme dokumentiert; T-0040 am Team-Node verifiziert (D014 via Inbox abgegeben) | SMTP-Erfolgspfad, Docker-Deployment (nach VM, D014) |
| Preflight + Trace-Matrix | preflight.py, trace_matrix.py (T-0024/26) | Tests + Echtläufe; Matrix 24 SWRs / 0 Lücken | Matrix-Gate in CI (T-0037) |
| **Anforderungs-Baseline (G1 + Erweiterung, D013/D015)** | STK-001–012 + SWR-001–024 (EN, D011) | DoD + QM; Traceability vollständig | Änderungen nur per CR |
| Verifikation | strategie.md v1, ui-abnahme-swr-021, swr-test-matrix | TEST/DEV-Review (T-0034) | 3 benannte Verifikationsschulden (strategie.md) |
| Tickets/Board | T-0001–T-0040; Sprint 3: 13/13 team-seitig done (inkl. T-0035/T-0040) | board.py --check grün; CI grün nach Push | T-0008 (Mensch, verschoben per D015) |
| Decision Log | D000–D015 (append-only; D014 = erste Inbox-Entscheidung) | — | — |

**QM-Mitzeichnung (2026-08-06, QM-Kontext):** Manifest gegen Repo-Stand geprüft; Prüfstatus je Item belegt (Ticket-/Test-/Gate-Referenzen); offene Punkte ehrlich ausgewiesen; keine Baseline-Blocker. Mitgezeichnet.

**Mensch-Freigabe:** G1-Erweiterung + G2 + G4 Sprint 3 erteilt 2026-08-06 (D015).

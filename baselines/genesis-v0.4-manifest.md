# Baseline-Manifest: genesis-v0.4 (Sprint-4-Abschluss — Generalprobe Teil 1)

*Erstellt 2026-08-06 (Sprint 4, CM). Anlass: G4 Sprint 4 (D020) — erster kompletter SWE.1–SWE.4-Durchlauf am fremden Übungsprodukt datakonv (G1/D018, G2/D019). Tags `genesis-v0.4` auf process, platform, p0; produkt-datakonv trägt `req-v1.0` (G1).*

| Item | Version/Stand | Prüfstatus | Offene Punkte |
|---|---|---|---|
| Masterplan, Playbook, P0-Beschreibung | 0.6 (E5 revidiert per D017: Betrieb lokal) / 0.6 / 0.5 | Baseline seit G0; Änderungen per CR | VM-Wiedervorlage zum P0-Abschluss (D017) |
| Rollenkarten + Registry | v1/v1.2 — 10 Rollen aktiv | unverändert seit v0.3 | Copilot-Kettenstufe ab PoC Sprint 6 (F13) |
| Prozess-Templates | decision-request mit maschinenlesbarem Frontmatter (T-0039) | Review CHG/QM | Erzwingung für neue DRs (T-0051, Sprint 5) |
| Orchestrator/Tick | Zweiphasen-Tick idempotent (T-0038) | 3 neue Tests; Wirkungsmessung am Team-Node offen | KPI-Nachweis „0 Zyklen je Warte-Lauf" (Betrieb) |
| CI platform | Unit-Tests + Matrix-Gate mit p0-Checkout (T-0037, Secret gesetzt) | Läufe nach Push zu prüfen | — |
| trace_matrix | generalisiert für Produkt-Repos (T-0048) | +2 Tests, Defaults byte-gleich | Produkt-CI-Gate (T-0049, Sprint 5) |
| Inbox/Backend | Options-Validierung (T-0039): ungültig → 400 ohne Log-Eintrag | +6 Tests (Suite 92) | Inbox diesen Sprint ungenutzt (QM-Punkt 4) |
| **Übungsprodukt datakonv** | Repo produkt-datakonv: Reqs `req-v1.0` (G1/D018), Architektur + ADR-D01–D03 (G2/D019), Units v0.1.0, 31 Tests | Matrix 17/17 SWRs, 0 Lücken; CI je Push | SWE.5/6 + Release Sprint 5; Matrix-CI-Gate T-0049 |
| Verifikation | + datakonv-swr-test-matrix.md (0 Lücken) | TEST/QM (T-0046) | benannte Schulden: SMTP-Pfad, Konsolen-Encoding |
| Tickets/Board | T-0001–T-0051; Sprint 4: 11 done, 1 rejected (D017) | board.py --check grün | Sprint-5-CRs T-0049–T-0051 offen |
| Decision Log | D000–D020 (append-only) | — | — |
| Risikoliste | R2 geschlossen (D017); R7 mit CR T-0050 adressiert | — | R7-Erwartungswert ab T-0050 |

**QM-Mitzeichnung (2026-08-06, QM-Kontext):** Manifest gegen Repo-Stand geprüft; Prüfstatus je Item mit Ticket-/Test-/Gate-Referenz belegt; offene Punkte ehrlich ausgewiesen (inkl. ungenutzter Inbox und Betriebs-KPI T-0038); keine Baseline-Blocker. Mitgezeichnet.

**Mensch-Freigabe:** G4 Sprint 4 erteilt 2026-08-06 (D020).

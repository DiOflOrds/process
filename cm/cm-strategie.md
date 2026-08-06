# CM-Strategie (v1.1)

*v1.1 (Sprint 2, T-0018, CM): Review-Nacharbeit zu T-0010 — Konfigurationselemente, Rollen und Storage-Locations an die reale Struktur angeglichen (Rollen aus `roles/registry.yaml`, Landkarte aus Playbook Kap. 3, reale Repos process/platform/p0). v1 entstand autonom via ollama/gemma3:27b (Sprint 1).*

## Konfigurationselemente

| Item-Typ | Repo/Pfad | Eigentümer | Beschreibung |
|---|---|---|---|
| Prozess-Baseline-Dokumente (Masterplan, Playbook, P0) | `process/docs/` | COACH | Verfassung des Teams; Änderung nur per CR |
| Rollenkarten + Rollen-Registry | `process/roles/` | COACH | Rollen v1: PL, CM, COACH, QM, RM, PROB, CHG (aktiv); ARCH, DEV, TEST, REL (geplant) |
| Prozess-Skills | `process/skills/` | COACH | je Prozessgebiet (MAN.3, SUP.1, SUP.8, SUP.9, SUP.10, SWE.1) |
| Wissensbasen (Lessons, Heuristiken, Gold-Beispiele) | `process/knowledge/<rolle>/` | COACH (kuratiert) | Updates nur per Prozess-CR + Regressionstest (Playbook Kap. 11) |
| Issue-Templates + Label-Schema | `process/templates/` | COACH | 8 Ticket-Typen |
| DoD-Checklisten | `process/checklists/` | COACH/QM | je Information-Item-Typ |
| CM-Artefakte (diese Strategie, Geräteregister, Runbook) | `process/cm/` | CM | Runbook folgt Sprint 3 |
| Baseline-Manifeste | `process/baselines/` | CM | je Baseline ein Manifest (Playbook Kap. 9) |
| Gateway, Orchestrator, Skripte | `platform/gateway/`, `platform/orchestrator/`, `platform/scripts/` | DEV (bis aktiv: Session-Kontext) | LLM-Gateway, Tick-Loop, board.py |
| Guardrails-Konfiguration | `platform/orchestrator/config/guardrails.yaml` | CM | Änderung nur per CR + Mensch-Freigabe |
| CI-Workflows | `platform/.github/workflows/`, `p0/.github/workflows/` | CM | board-check + Unit-Tests (T-0015) |
| Unit-Tests | `platform/tests/` | DEV | 62+ Tests, CI-Pflicht |
| Projektführung (Plan, Reports, Risikoliste, Decision Log) | `p0/management/` | PL | Decision Log append-only |
| Tickets + Board | `p0/tickets/`, `p0/BOARD.md` | PL | BOARD.md wird generiert (board.py), nie von Hand |
| Requirements (Stakeholder, Software) | `p0/requirements/` | RM | Englisch (D011); Baseline via G1 |
| Run-Registry | `p0/management/runs/run-registry.jsonl` | Orchestrator | append-only, jede Agent-Aktion mit Kosten |

## Branching-Modell

- **main:** geschützter Zweig; Merge nur per PR mit Review (Reviewer ≠ Autor). Ausnahme Bootstrap-/Session-Engineering: direkte Commits mit Ticket-ID, nachreviewt im Sprint-Review (G4) — läuft aus, sobald PR-Fluss auf dem Team-Node etabliert ist.
- **feature/t-xxxx-<kurzname>:** Arbeits-/Tick-Branches je Ticket (Kleinschreibung, wie vom Orchestrator erzeugt).
- **hotfix/t-xxxx-<kurzname>:** kritische Fixes aus `main`, zurück per PR.
- Release-Branches: erst ab echten Produkt-Releases (Stufe 2); bis dahin genügen Baselines (Tags).

## Namenskonventionen

- **Tickets:** `T-nnnn` (vierstellig, fortlaufend über alle Sprints).
- **Baselines:** `genesis-v<major.minor>` für die Prozess-/Plattform-Baseline (P0); Produkt-Baselines später `<produkt>-v<major.minor>`.
- **Branches:** siehe Branching-Modell. **Commits:** beginnen mit Ticket-ID(s), z.B. `T-0018: CM-Strategie v1.1`.
- **Anforderungen:** `STK-xxx` (Stakeholder), `SWR-xxx` (Software), dreistellig.

## Storage-Locations und Backup

| Was | Wo | Zugriff | Backup/Redundanz |
|---|---|---|---|
| Alle drei Repos (Single Source of Truth) | github.com/DiOflOrds/{process, platform, p0} (privat) | Mensch (Owner); Team via Token; Cowork-Sandbox nur lesend (D007) | GitHub-Hosting + lokale Klone |
| Lokale Arbeitskopien | Team-Node 1 (User-PC): `Downloads\aspice-team-repos-final\` | Mensch + autonome Ticks (D007) | Push nach GitHub nach jeder Session |
| Run-Registry, Board | im p0-Repo (versioniert) | wie Repos | wie Repos |
| Secrets (GitHub-Token, künftig ANTHROPIC_API_KEY, PLATFORM_READ_TOKEN) | Windows Credential Manager bzw. GitHub Actions Secrets | nur Mensch | **nie im Repo**; Referenz im Geräteregister |

**Restore-Konzept:** Wiederherstellung = frisches Klonen von GitHub; lokale Klone dienen umgekehrt als Kopie, falls GitHub nicht verfügbar. **Restore-Test: noch offen** — geplant mit dem Betriebs-Runbook (Sprint 3); bis dahin gilt der doppelte Bestand (GitHub + Team-Node) als Mindestabsicherung.

## Tool-Liste

- **Git + GitHub** (D005): Versionierung, PRs, Actions.
- **board.py** (`platform/scripts/`): Ticket-Validierung + BOARD.md-Generierung — Skript-Route, läuft je Tick und in CI.
- **tick.py** (`platform/orchestrator/`): autonomer Tick-Loop (Preconditions, Gateway, Guardrails, Run-Registry).
- **GitHub Actions** (T-0015): `platform` Unit-Tests, `p0` board-check je Push/PR.
- **LLM-Gateway** (`platform/gateway/`): Provider-Ketten ollama/session/claude (copilot ab Sprint 6).

## Geräteregister-Verweis

`process/cm/geraeteregister.md` — Neuaufnahme von Geräten nur mit Mensch-Freigabe (guardrails: device_onboarding).

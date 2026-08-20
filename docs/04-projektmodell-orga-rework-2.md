# Projektmodell — Orga-Rework 2 (v1.1 — Kap. 9 Workflows ergänzt, 2026-08-21)

*2026-08-21, Cowork-Session im Auftrag des Auftraggebers. Anlass: Auftraggeber-Auftrag mit Organigramm-Diagramm („Es gibt das PM und nur Projekte, keine Teams mehr"). Baut auf `03-rollenmodell-v2-orga-rework.md` auf und ersetzt dessen Team-Begriff; Rollenmodell v2 (Bauplan/Instanz, Motoren, Takte) gilt unverändert weiter. Migrationsentscheidungen des Auftraggebers (2026-08-21): in-place-Umwidmung, implizites Core Team, Setup-Skript. Änderungen an diesem Dokument laufen als Prozess-CR (SUP.10).*

## 1. Auftrag des Auftraggebers (zusammengefasst)

1. Es gibt das **PM** und **nur Projekte** — keine Teams mehr.
2. Jedes Projekt hat ein **Core Team** (alle ASPICE-Rollen) mit Allgemeinwissen und projektspezifischem Wissen; es entwickelt, überwacht und monitort das Projekt.
3. Daneben gibt es **projektspezifische Rollen** mit den tatsächlichen fachlichen Projektaufgaben.
4. Jedes Projekt **verwaltet sich selbst**; das **PM-Team koordiniert** die Projekte; die **Plattform** (vom Plattform-Projekt geliefert) **muss verwendet werden**.
5. Beim **Setup** eines Projekts: umfassende Planung durch den PL nach Projektmanagement-Methoden (Ziele, Phasen, Aufgaben, Workflows, Team/Rollen, Infrastruktur, Timeline …).
6. Jede Rolle **initialisiert das Projekt aus ihrer Sicht** und plant ihre **wiederkehrenden Aufgaben** (CM → CM-Plan; COACH → Workflows + Rollenbeschreibungen allgemein/projektspezifisch; RM → Anforderungen aus den Zielen; ARCH → System-/SW-Architektur; QM → Reviews/Audits; …).
7. Rollen bekommen Tasks vom PL **und** erstellen eigene Tasks — einmalig oder wiederkehrend. Der **PL kennt, überwacht, koordiniert, eskaliert und ergänzt alle Tasks**, kennt das ganze Projekt und **berichtet an das PM-Team**.
8. **Alles live im HMI**: Tasks, Work Products, Berichte, jegliche Kommunikation zwischen den Projekten.

## 2. Zielbild

```
Mensch (Klasse A, Gates)
  └── PM-Team (Koordination, Intake, Staffing — kein Projekt)
        ├── P9  … Pxx        klassische Projekte (projects/ bzw. Top-Level)
        ├── P13 Mail         (Repo team-mail, umgewidmet)
        ├── P14 Dashboard    (Repo team-dashboard, umgewidmet)
        ├── P15 Prompt       (Repo promt-team, umgewidmet)
        └── Plattform        (Repo platform — Dauerprojekt, liefert die Plattform für alle)
```

Es gibt genau **zwei Organisationsformen**: das PM-Team und Projekte. Das bisherige „ASPICE-Team" ist das **Plattform-Projekt** (Dauerauftrag: Mission Control, board.py, Orchestrator/Gateway, Skripte — für alle verbindlich). Die bisherigen Projekt-Teams sind Projekte mit Dauerauftrag (Profil `wiederkehrend` bleibt als Arbeitsweise-Profil erhalten — ein Projekt kann Sprint- oder Kanban-artig arbeiten).

## 3. Core Team und projektspezifische Rollen

### 3.1 Core Team (implizit, eine Quelle)

**Jedes aktive Projekt hat automatisch das volle Core Team**: PL, RM, ARCH, DEV, TEST, QM, CM, PROB, CHG, COACH. Es steht **nicht** je Projekt in der Registry — `process/roles/besetzungen.yaml` führt einen `core_team`-Block (Rollen, Default-Motor `cowork`, Default-Takt `sprint`), der für jedes aktive Projekt expandiert wird. Explizit stehen nur **Abweichungen** (z. B. `PROB@platform` auf Ollama/schnell) und **projektspezifische Rollen**. Eine Quelle, keine hundert Duplikat-Einträge — zwei Listen derselben Sache driften (B033).

Wissens-Ebenen je Core-Rolle (Rollenmodell v2, unverändert): **Allgemeinwissen** = Rollenkarte v2 + `knowledge/<rolle>/`; **projektspezifisches Wissen** = `roles/<rolle>.md` im Projekt-Repo + `docs/historie.md` (Pflicht-Lektüre je Einsatz).

Auftrag des Core Teams je Projekt: **entwickeln** (fachliche Arbeit gemäß Profil), **überwachen** (Reviews, QM, Verifikation), **monitoren** (Board-Hygiene, KPIs, Takt-Tickets, Historie).

### 3.2 Projektspezifische Rollen

Fachrollen mit den tatsächlichen Domänen-Aufgaben des Projekts — explizit in der Registry, mit eigener Rollenkarte (`process/roles/<rolle>.md`) und projektspezifischem Teil. Bestand: `MAIL-RED@team-mail` (P13), `DASH-RED@team-dashboard` (P14), `PROMPT-OPT@promt-team` (P15). Neue entstehen beim Projekt-Setup (PL benennt sie im Projektplan; Anlage: PM Klasse B, neue Rollen-Baupläne Klasse A).

### 3.3 Governance (unverändert aus Rollenmodell v2 Kap. 7)

Projekt verwaltet sich selbst (Klasse C) · PM koordiniert projektübergreifend, konfiguriert Besetzungen, entscheidet Klasse B geloggt · Mensch: Klasse A, Gates, jederzeit Direkteingriff · **Plattform-Pflicht:** kein Projekt baut eigene Werkzeuge; Werkzeugbedarf ist ein CR ans Plattform-Projekt (Katalog-/CR-Mechanik).

## 4. Projekt-Setup (der neue Pflichtprozess)

### 4.1 Ablauf

| Phase | Wer | Ergebnis |
|---|---|---|
| 0 Auftrag | Mensch (G0/TG, Klasse A) | Projektauftrag, Kennung Pxx |
| 1 **Projektplanung** | PL | **Projektplan** nach PM-Methoden (Template `process/templates/projektplan.md`): Ziele, Phasen/Meilensteine, Aufgabenstruktur, Workflows, Team/Rollen (Core-Abweichungen + spezifische Rollen), Infrastruktur, Timeline (Sprints), Risiken, Berichtsweg |
| 2 **Rollen-Initialisierung** | jede Core-Rolle | Initial-Artefakt + eingeplante wiederkehrende Aufgaben (Tabelle 4.2); Reviews nach Rollenmatrix |
| 3 Betrieb | alle | Sprint-/Kanban-Betrieb, Monitoring, Berichte an PM |

`platform/scripts/projekt_setup.py` erzeugt die Struktur samt **Initialisierungs-Ticket je Core-Rolle** und **Takt-Tickets** — der „Starten"-Knopf und das PM nutzen dasselbe Skript. Das Skript erzeugt Struktur, nie Freigaben: G0 bleibt Klasse A (Lehre p12: der Knopf darf gründen, aber nie entscheiden).

### 4.2 Rollen-Initialisierung (Pflicht-Artefakte und wiederkehrende Aufgaben)

| Rolle | Initial-Artefakt (einmalig) | Wiederkehrend (Takt-Ticket) |
|---|---|---|
| PL | Projektplan (s. o.), Risikoliste, Sprint-0-Plan | Board-Monitoring + Chronikzeile je Sprint; Bericht an PM |
| CM | **Konfigurationsmanagement-Plan** (`docs/cm-plan.md`): Work Products, Tools, Repos/Storage, Baselines-Regeln | Baseline-Pflege, Backup-Check |
| COACH | **Workflows** des Projekts + Rollenbeschreibungen: Verweis auf allgemeinen Teil (Genesis-Ebene) + projektspezifische Teile (`roles/*.md`) anlegen | Retro/LeLe je Sprint, Wissensbasis-Kuratierung |
| RM | **Anforderungen aus den Zielen** (STK aus Projektplan-Zielen, erste SWRs) | Trace-Report, Clarification-Bündelung |
| ARCH | **System-/SW-Architektur** (Erstentwurf + ADRs) | Architektur-Konformitäts-Review |
| QM | **Review-/Audit-Plan** (`docs/qm-plan.md`): was wird wann von wem geprüft, Stichproben | Stichproben je Sprint, Gate-Checks |
| TEST | Verifikationsstrategie | Matrix-/Coverage-Bericht |
| DEV | Entwicklungsumgebung/CI-Anbindung dokumentiert | Lint/Format-Läufe (Skript-Route) |
| PROB | Problem-Register angebunden (feedback_route) | Trend-Report je Sprint |
| CHG | CR-Workflow angebunden | offene-CR-Review je Sprint |

Jedes Initialisierungs-Ticket hängt per `blocked_by` am PL-Planungs-Ticket — **Planung zuerst**, dann initialisieren die Rollen parallel.

## 5. Task-Governance

1. Alle Aufgaben sind Tickets im Projekt-Repo — vom PL zugewiesene **und** rollen-eigene (jede Rolle darf Tickets für sich anlegen), einmalig oder wiederkehrend (`takt:`-Feld).
2. **Der PL kennt alle Tasks:** Das Board ist vollständig (keine Arbeit ohne Ticket — bestehende Regel); der PL überwacht (Sync-Tick, Ampeln, Überfälligkeit), koordiniert (Prioritäten, `blocked_by`), eskaliert (DR/PM) und ergänzt (fehlende Aufgaben nachziehen). Rollen-eigene Tickets sieht er spätestens im nächsten Sync-Tick — stilles Arbeiten an Board vorbei ist ein QM-Finding.
3. **Berichtsweg:** PL → PM-Team je Sprint (Report + Status im Cockpit); PM → Mensch gebündelt (Digest/Inbox). PM koordiniert projektübergreifend (Session-Agenda, Kapazität, Ollama-Serialisierung, PL-Konflikte).

## 6. HMI: alles live für den Menschen

**Vorhanden** (je Projekt, Discovery-getrieben): Cockpit/Org-Cockpit (Status aller Projekte), Board + Aufgabenliste (Tasks inkl. Takt-Ampeln), Ticket-Detail mit Verlauf (Kommunikation), Briefkasten/Chat je Projekt, Sprint-Plan (`/api/sprint`), Reports, Inbox (DRs), Session-/Run-Registry (wer lief wann), Organisation & Planung (`/organisation.html`: Organigramm, Besetzungen, Gantt).

**Lücken → Folge-CRs ans Plattform-Projekt** (SWRs nachziehen): (1) **Work-Product-Sicht** je Projekt (CM-Plan-getrieben: welche Artefakte existieren, Version, Prüfstatus); (2) **projektübergreifende Kommunikations-Sicht** (alle Briefe/Kommentare chronologisch, filterbar); (3) Organigramm-Ansicht um Core-Team-Kennzeichnung erweitert (Teil dieser Lieferung).

## 7. Migration (in-place, Identität ≠ Beschriftung)

SWR-175-Prinzip: **Ordner-/Repo-Namen sind Identitäten und bleiben**; geändert wird, was der Mensch liest.

| Bisher | Wird | Repo bleibt | Anmerkung |
|---|---|---|---|
| team-mail | **P13 Mail** | `team-mail` (`.kein-remote`, sensibel) | MAIL-RED wird projektspezifische Rolle; Charter bleibt als Projektauftrag gültig |
| team-dashboard | **P14 Dashboard** | `team-dashboard` | DASH-RED projektspezifisch; fachlicher Auftraggeber von P11 bleibt |
| promt-team | **P15 Prompt** | `promt-team` (`.kein-remote`, sensibel) | PROMPT-OPT projektspezifisch; Auflagen (Baseline/Goldset, „Vorschläge bleiben Vorschläge") gelten unverändert |
| aspice (platform) | **Plattform-Projekt** | `platform` | Dauerauftrag; einziges Projekt mit Schreibrecht auf die Plattform |
| pm | PM-Team (unverändert) | `pm` | kein Projekt — die Koordinationsinstanz |

`process/teams/registry.yaml` wird zur **Projekt-Registry** (Kopf + `projekt:`-Kennungen P13–P15/plattform). ⚠ Die `typ:`-Literale (`aspice`, `pm`, `projekt`) bleiben **technisch** unverändert, weil Backend (`aggregation.einstufung`) und Tests darauf prüfen — die semantische Umbenennung (`aspice` → `plattform`) ist ein eigener Plattform-CR mit Testanpassung, kein Nebenbei-Edit (offener Faden).


## 8. Umsetzungsstand

| Schritt | Inhalt | Stand |
|---|---|---|
| 1 | Dieses Konzept | erledigt |
| 2 | Projekt-Registry (Umwidmung P13–P15, Plattform) + `core_team`-Block in besetzungen.yaml | erledigt |
| 3 | Resolver „effektive Besetzungen" (Core-Expansion; SWR-171-Prüfung, HMI, Organigramme nutzen ihn) | erledigt |
| 4 | `projekt_setup.py` + `projektplan.md`-Template + Initialisierungs-/Takt-Tickets | erledigt |
| 5 | Organigramme + `/organisation.html` mit Core-Team-Kennzeichnung | erledigt |
| 6 | Work-Product-Sicht, Kommunikations-Sicht, typ-Literal-Umbenennung | **gebaut** (2026-08-21, Auftraggeber-Auftrag): SWR-181–184 (draft) + zwei neue HMI-Tabs (T-0039/T-0040 in_review bis SWR-Review); T-0041 done (plattform-Literal, aspice toleriert); SWR-Nachtrag T-0042 in_review |
| 7 | Bestehende aktive Projekte: fehlende Initial-Artefakte nachziehen | **erledigt** (2026-08-21): alle 7 Nachzieh-Tickets done (36 Artefakte), QM-Stichprobe ohne Befund — pm/T-0074; Rollen-/QM-Review der Artefakte im nächsten Sprint-Lauf |

## 9. Workflows (Nachtrag 2026-08-21, Auftraggeber-Auftrag; SWR-187/188)

Workflows sind ein tragender Baustein des Projektmodells: **Jede wiederkehrende Aufgabe wird als Workflow geplant und transparent dargestellt** — jeder Schritt hat einen **Input**, eine **Rolle** (oder ein Skript mit Werkzeug) mit Aktion und einen **Output**, der in der Regel ein Work Product ist.

1. **Eine Quelle je Einheit:** `docs/workflows.yaml` (Schema: `process/templates/workflows.yaml`). Die Workflow-Sicht im HMI rendert daraus die Schrittkette (Input → Rolle/Werkzeug → Output) und je Rolle die Workflows, an denen sie beteiligt ist.
2. **Governance im Schema, nicht in Prosa:** `geplant_von` (PL oder die Rolle selbst), `arch_review` (ARCH prüft den Schnitt), `cm_verankert` (CM verankert Outputs als Work Products im CM-Plan) — leere Marken sind **sichtbar** leer. Damit sind PL, ARCH, CM und die ausführende Rolle verbindlich angebunden.
3. **Abdeckungspflicht:** Jedes Takt-Ticket braucht einen tragenden Workflow; die Grundmenge der Prüfung sind die Takt-Tickets, nicht die Workflow-Datei (SWR-128-Familie). Unabgedeckte Takte meldet die Sicht.
4. **Output-Ehrlichkeit:** Outputs referenzieren CM-Plan-Work-Products, wo sie existieren (WP✓); ein Output ohne Referenz wird als „frei" gemeldet, nicht erfunden. Laufzeitdaten (z. B. `eingang/` in P13) sind erlaubt und als solche benannt.
5. **Setup:** `projekt_setup.py` erzeugt für jedes neue Projekt die zwei Standard-Workflows (PL-Monitoring, QM-Stichprobe), gebunden an die im selben Lauf erzeugten Takt-Tickets — ein neues Projekt startet mit null unabgedeckten Takten (SWR-188).

Bestand (2026-08-21): 6 Workflows in 4 Einheiten decken alle 6 Takt-Tickets — Muster ist `WF-P13-DIGEST` (laden → verdichten → Dashboard → zustellen, exakt der Auftraggeber-Wortlaut).

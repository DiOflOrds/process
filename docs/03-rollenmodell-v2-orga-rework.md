# Rollenmodell v2 — Orga-Rework (v1.1 — F18–F20 entschieden)

*2026-08-20, Cowork-Session im Auftrag des Auftraggebers. Anlass: Auftraggeber-Auftrag „umfassendes Rework für die Orgastruktur, das die Rollen betrifft" (Wortlaut in Kap. 1). Baut auf `00-masterplan.md` (Kap. 4/5.3/5.4/5.8) und `02-genesis-2.0-orgkonzept.md` auf. v1.1: F18–F20 am 2026-08-20 vom Auftraggeber entschieden (pm/D010–D012, Kap. 11). Änderungen an diesem Dokument laufen als Prozess-CR (SUP.10).*

## 1. Auftrag des Auftraggebers (2026-08-20, zusammengefasst)

1. Jedes Team und Projekt bekommt ein **Organigramm** — auf einen Blick sichtbar, wer beteiligt ist.
2. Jede Rolle hat **Beschreibung/Eigenschaften**, **allgemeines Hintergrundwissen**, **projektspezifisches Hintergrundwissen** und **Tool-Benutzung**.
3. Jedes Projekt hat eine **Historie** und **Lessons Learned**.
4. KI-Rollen haben eine **Konfiguration**: lokale KI (**Ollama**), **Claude Cowork** (Session) oder **API**-Schnittstellen.
5. Jede Rolle ist **unabhängig**: holt Aufgaben selbst ab, bearbeitet, erledigt, stellt Rückfragen.
6. Rollen **lernen ständig dazu** und können ihr Aufgabengebiet erweitern.
7. Rollen haben **klare Abgrenzung** zueinander.
8. Jede Rolle kann **mehrfach** existieren (Instanzen), mit **allgemeinem Teil** (gilt überall) und **projektspezifischem Teil** (gilt nur im Projekt). Beispiel: je Projekt ein eigener Projektleiter.
9. Das **PM-Team koordiniert alle Projektleiter** in Teams und Projekten.
10. **Cowork-Rollen** arbeiten im Sprint-Takt (stündlicher geplanter Start); **Ollama-Rollen** können Aufgaben schneller abholen, sind aber auf ein gleichzeitig geladenes Modell begrenzt.
11. Rollen-Konfiguration macht das **PM-Team**; der **Mensch** kann Rollen jederzeit hinzufügen, bearbeiten, entfernen.
12. Rollen kommunizieren über **Tasks und Kommentare**.
13. Es gibt **Live-Nachverfolgung, Organisation und Projektmanagement** für den Menschen inkl. **Zukunftsplanung** (MS-Project-artig).

## 2. Leitidee: Rolle = Bauplan, Besetzung = Instanz

Fast alles aus Kap. 1 existiert als Mechanik und wird **verallgemeinert, nicht neu erfunden** (Genesis-2.0-Prinzip). Der eine neue Grundbegriff:

> **Eine Rolle ist ein Bauplan** (allgemeine Rollenkarte, Prozess-Skills, Wissensbasis, Provider-Ketten). **Eine Besetzung ist eine Instanz dieses Bauplans in genau einem Team oder Projekt** — mit eigenem projektspezifischem Teil, eigener KI-Konfiguration und eigenem Arbeitsvorrat (Tickets des Repos mit `rolle: <kürzel>`).

Damit gilt: `PL` gibt es einmal als Bauplan — und als Instanzen `PL@p11`, `PL@p12`, `PL@pm`, … Jede Instanz agiert unabhängig in ihrem Repo; was alle Instanzen teilen, ist der Bauplan und die Wissensbasis der Rolle (Lernen einer Instanz nützt allen, Kap. 6).

### 2.1 Die drei Ebenen und ihre Ablage

| Ebene | Inhalt | Ablage | Pflege durch |
|---|---|---|---|
| **Bauplan (allgemein)** | Rollenkarte v2 (Beschreibung/Eigenschaften, Abgrenzung, Tool-Nutzung, Default-KI-Konfiguration), Prozess-Skills, allgemeine Wissensbasis | `process/roles/<rolle>.md`, `process/skills/`, `process/knowledge/<rolle>/` | PM-Team (Prozess-CR), Mensch jederzeit |
| **Projektspezifischer Teil** | Projektauftrag der Rolle, projektspezifisches Hintergrundwissen, projektspezifische Tools, Abweichungen vom Bauplan | `<repo>/roles/<rolle>.md` (Team-Repos und `projects/<px>/roles/`) | Instanz selbst + PL des Repos (Klasse C), PM bei Staffing |
| **Besetzung (Instanz)** | Wer/was die Rolle wo ausfüllt: Motor (cowork/ollama/api/script/mensch), Modell, Takt, Status | `process/roles/besetzungen.yaml` | PM-Team (Klasse B, geloggt), Mensch jederzeit |

Beim Einsatz einer Instanz wird geladen: **Bauplan + projektspezifischer Teil + Wissensbasis + Projekt-Historie** (Kap. 5). Fehlt der projektspezifische Teil, gilt der Bauplan allein — kein Bruch für bestehende Abläufe.

### 2.2 Kompatibilität (bewusste Nicht-Änderungen)

- `process/roles/registry.yaml` (Provider-Ketten je Aufgaben-Typ) bleibt **unverändert gültig** — sie beschreibt den Bauplan-Teil des Routings. `besetzungen.yaml` kommt **daneben**, ersetzt nichts.
- Rollenkarten v1 bleiben gültig und werden **sukzessive** aufs v2-Template gehoben (Template: `process/templates/rollenkarte-allgemein.md`); keine Stichtags-Migration.
- Board-, Ticket- und Discovery-Mechanik bleiben unberührt. Der Arbeitsvorrat einer Instanz ist wie bisher: Tickets ihres Repos mit ihrer Rolle.

## 3. Rollenkarte v2 — Aufbau

### 3.1 Allgemeiner Teil (`process/roles/<rolle>.md`)

Pflichtabschnitte (Template `rollenkarte-allgemein.md`):

1. **Beschreibung und Eigenschaften** — Auftrag in 3–5 Sätzen; Arbeitsstil/Eigenschaften (z. B. QM: unabhängig, blockiert lieber als durchzuwinken).
2. **Abgrenzung** — Tabelle „Das tue ich nicht → das tut stattdessen …". Jede Zeile benennt die Nachbarrolle. Neue Rollen dürfen erst aktiv werden, wenn dieser Abschnitt gegen alle bestehenden Rollen geprüft ist (QM-Checkliste); Überschneidungen sind ein Finding.
3. **Hintergrundwissen (allgemein)** — was die Rolle können/wissen muss, plus Verweis auf `process/knowledge/<rolle>/` (Lessons, Heuristiken, Gold-Beispiele).
4. **Tool-Benutzung** — welche Skripte, Katalog-Produkte und Plattform-Werkzeuge die Rolle nutzt und wofür (Skript-Route zuerst — Automatisierungspyramide gilt unverändert).
5. **Aufgabenabholung und Kommunikation** — Pull-Prinzip und Rückfrage-Wege (Kap. 4.3).
6. **KI-Konfiguration (Default)** — Verweis auf Registry-Ketten + empfohlener Motor als Startwert für neue Besetzungen.
7. **Lernen und Erweiterung** — Verweis auf Lernzyklus (Playbook Kap. 11) und die Erweiterungsregel (Kap. 6).

### 3.2 Projektspezifischer Teil (`<repo>/roles/<rolle>.md`)

Pflichtabschnitte (Template `rollenkarte-projekt.md`):

1. **Auftrag in diesem Projekt** — was die Instanz hier konkret verantwortet (aus Projektauftrag/Steckbrief abgeleitet).
2. **Projektspezifisches Hintergrundwissen** — Domänenwissen, zentrale Anforderungen/ADRs, Eigenheiten („in p11 gilt: Mail-Inhalte nie ins Repo").
3. **Projektspezifische Tools** — was hier zusätzlich/abweichend genutzt wird.
4. **Historie und Lessons Learned** — Verweis auf `docs/historie.md` des Repos (Kap. 5); die Instanz liest sie bei jedem Einsatz.
5. **Abweichungen vom Bauplan** — explizit und begründet; leer ist der Normalfall.

## 4. Besetzungen: Instanzen, Motoren, Unabhängigkeit

### 4.1 Instanzen-Registry (`process/roles/besetzungen.yaml`)

Je Eintrag: `instanz` (z. B. `PL@p11`), `rolle`, `einheit` (Team/Projekt), `motor`, `modell`, `takt`, `status`. Schema und Startbestand: siehe Datei. Pflege: PM-Team (Klasse B, im PM-Decision-Log), Mensch jederzeit über HMI/Chat (Kap. 7).

### 4.2 Motoren und ihr Taktverhalten

| Motor | Was es ist | Takt/Latenz | Grenzen |
|---|---|---|---|
| `cowork` | Claude-Cowork-Session (heutiger Standard) | **Sprint-Takt**: geplanter Start stündlich (`sprint_register.py`, Takt 60 min); Instanz arbeitet ihren Arbeitsvorrat im Lauf ab | Läuft nur, wenn die Session läuft; stark, voll agentisch |
| `ollama` | Lokales LLM auf einem Team-Node | **Schneller Abholtakt** möglich (Minuten, via Aufgabenplanung + `tick.py`) | **Serialisierung:** pro Node ist praktisch nur ein Modell geladen → es arbeitet nur **eine Ollama-Instanz gleichzeitig je Node**; die Warteschlange vergibt Aufgaben nacheinander (Reihenfolge: Prio, dann Fälligkeit). Nur für Aufgaben-Typen, deren Kette `ollama` enthält |
| `api` | Claude-API (Agent SDK, headless) | Sofort, unabhängig von Sessions | **Kostet Geld je Token** — Aktivierung nur mit Budget-Freigabe (Klasse A, D012/D026-Kontext); harte Limits aus `guardrails.yaml` |
| `script` | Deterministisches Skript | Sofort | Nur registrierte `script_tasks` |
| `mensch` | Menschlicher Rollen-Inhaber | Nach Verfügbarkeit | Aufgaben über das HMI |

Die Automatisierungspyramide bleibt: **Skript → Ollama → (Copilot) → Claude → Mensch.** Der Motor einer Besetzung ist der *Default-Startpunkt*, die Registry-Ketten je Aufgaben-Typ gelten weiter (gate-relevantes läuft nie auf Ollama).

**Der Takt kennt genau zwei Werte** (Klärung Auftraggeber 2026-08-20): `sprint` = Session-/Sprint-Lauf — seit pm/D006 ist jeder Session-Lauf ein vollwertiger Sprint, ein separater Wert „session" wäre dieselbe Sache unter zweitem Namen (B033-Falle); `schnell` = Aufgabenplanung alle 15 min (F18, nur Ollama-Besetzungen).

### 4.3 Unabhängigkeit: Abholen, Bearbeiten, Rückfragen

Jede Instanz arbeitet nach dem **Pull-Prinzip** — niemand schiebt ihr Arbeit in den Kontext:

1. **Abholen:** Board des eigenen Repos lesen (`board.py --check`); Arbeitsvorrat = Tickets mit eigener Rolle, Status `todo`/`doing`, nicht `blocked`, nach Prio und Fälligkeit.
2. **Bearbeiten/Erledigen:** Arbeit gemäß Rollenkarte (beide Teile) + Skills + Wissensbasis + Historie; Ergebnis als Commit/Artefakt am Ticket; Statuspflege + BOARD-Regenerierung.
3. **Kommunizieren:** ausschließlich **ticketbasiert** — Verlaufsvermerke/Kommentare am Ticket (SWR-081-Mechanik), Übergaben als neue Tickets oder `blocked_by`-Beziehungen an die Nachbarrolle.
4. **Rückfragen:** fachlich an die zuständige Rolle (Kommentar/Ticket); an den Menschen nur gebündelt über PL → Decision Request/Briefkasten (bestehende HITL-Mechanik).
5. **Konflikt:** PL des Repos entscheidet kriterienbasiert; darüber PM-Team; Klasse A immer Mensch.

### 4.4 PL-Koordination durch das PM-Team

Das PM-Team ist die **Koordinationsinstanz aller PL-Instanzen**: Es führt die Session-Agenda (welche Einheiten heute getickt werden), gleicht Prioritäten zwischen Projekten ab, verteilt Kapazität (Motoren/Nodes), löst PL-übergreifende Konflikte (Klasse B, geloggt) und pflegt die Besetzungs-Registry. Jeder PL bleibt in seinem Repo unabhängig — das PM-Team steuert *zwischen* den Projekten, nie *in* ihnen. (Ergänzung der Team-Charter `pm/docs/01-team-charter.md`.)

## 5. Historie und Lessons Learned je Projekt

Jedes Projekt-/Team-Repo bekommt `docs/historie.md` (Template `projekt-historie.md`):

- **Chronik:** Gründung/Auftrag (G0/TG), Meilensteine, Gates, Baselines, wichtige Entscheidungen (verweist auf Decision-Log, dupliziert nicht).
- **Lessons Learned:** projektbezogene LeLe mit Verweis ins LeLe-Register; was davon in Wissensbasen der Rollen übernommen wurde.
- Pflege: PL-Instanz je Sprint-/Taktabschluss (Chronikzeile), COACH bei LeLe. Die Historie ist **Pflicht-Lektüre jeder Instanz beim Einsatz** — so wirkt Projektgedächtnis auch bei Motor- oder Besetzungswechsel.

## 6. Lernen und Erweiterung des Aufgabengebiets

Der Lernzyklus (Playbook Kap. 11) gilt unverändert je **Rolle** (Bauplan-Ebene): Findings → COACH destilliert → Prozess-CR → Gold-Beispiel-Regression → Messung. Neu präzisiert:

- **Projektspezifisches Lernen** landet im projektspezifischen Teil bzw. `docs/historie.md`; **verallgemeinerbares** wandert per Prozess-CR in die Rollen-Wissensbasis (COACH kuratiert — eine Instanz lernt, alle Instanzen profitieren).
- **Erweiterung des Aufgabengebiets:** Eine Rolle darf neue Aufgaben-Typen beantragen (CR auf `registry.yaml` + Rollenkarte). Prüfmaßstab ist der Abgrenzungs-Abschnitt **aller betroffenen Rollen**: Erweiterung ja, Überschneidung nein. Entscheid: PM (Klasse B); neue Rollen oder Zuschnitt-Änderungen zwischen Rollen: Mensch (Klasse A).

## 7. Governance: Wer konfiguriert Rollen

| Vorgang | Entscheider | Klasse |
|---|---|---|
| Besetzung anlegen/ändern (Motor, Modell, Takt), Staffing | PM-Team | B (geloggt, Einspruch jederzeit) |
| Rollenkarte allgemein ändern, Aufgaben-Typ ergänzen | PM-Team per Prozess-CR (Review betroffene Rolle + QM) | B |
| Neue Rolle, Rollen-Zuschnitt, Team-Gründung/-Archivierung | Mensch | A |
| Motor `api` aktivieren (Budget) oder `ollama`-Autopilot (Aufgabenplanung) einrichten | Mensch | A |
| Rollen hinzufügen/bearbeiten/entfernen durch den Menschen | Mensch direkt (HMI/Chat); PM zieht die Registry nach | — |

## 8. Organigramme

**Quelle ist die Registry, nie Handpflege.** Neuer Generator `platform/scripts/organigramm.py` liest `teams/registry.yaml`, `roles/registry.yaml`, `roles/besetzungen.yaml` und die Projekt-Steckbriefe und erzeugt deterministisch:

1. **`ORGANIGRAMM.md`** je Team- und Projekt-Repo — Mermaid-Diagramm: Mensch → PM-Team → Einheit → Rollen-Instanzen (mit Motor-Kennzeichen), versioniert in Git.
2. **`platform/backend/static/organigramm.json`** — dieselben Daten als eine Datei fürs Frontend.
3. `--check`-Modus nach dem Muster von `arch_diagramm.py` (Gate: Bild passt zur Quelle).

## 9. Live-Nachverfolgung und Zukunftsplanung

Neue HMI-Seite **`/organisation.html`** (eigenständige statische Seite, kein Eingriff in `app.js` — Integration als Tab ist ein Folge-CR ans ASPICE-Team):

1. **Organisation:** Organigramm der Gesamtorganisation und je Einheit (aus `organigramm.json`), mit Besetzung, Motor und Status je Instanz.
2. **Live:** wer arbeitet gerade woran — aus `/api/uebersicht` + `/api/board` je Einheit (Tickets in `doing`, Rolle, Altersampel), automatisch aktualisiert.
3. **Planung (MS-Project-artig):** Zeitachse aus dem Sprint-Register (Takt 60 min) und den Takt-Tickets; je Einheit eine Zeile, Tickets als Balken nach Sprint/Fälligkeit, `blocked_by` als Abhängigkeitspfeile; Zukunft = geplante Sprints + fällige Takte.

## 10. Migration und Umsetzungsstand

| Schritt | Inhalt | Stand |
|---|---|---|
| 1 | Dieses Konzept | **erledigt** (dieses Dokument) |
| 2 | `besetzungen.yaml` + drei Templates | **erledigt** |
| 3 | Projektspezifische Rollenkarten + `docs/historie.md` für p11, p12; PL-Koordination in PM-Charter | **erledigt** (Muster; weitere Einheiten folgen im Betrieb) |
| 4 | `organigramm.py` + generierte ORGANIGRAMM.md + `organigramm.json` | **erledigt** |
| 5 | `/organisation.html` (Organisation + Live + Planung) | **erledigt** (v1; Tab-Integration in app.js als Folge-CR) |
| 6 | Rollenkarten v1 → v2-Template heben (alle 10 Rollen) | offen — je Rolle ein kleines Ticket, im Betrieb |
| 7 | Historie für Alt-Projekte (p0–p10) nacherfassen | offen — niedrige Prio, aus Abschlussberichten generierbar |
| 8 | Ollama-Schnelltakt (Aufgabenplanung + `tick.py`) aktivieren | **Skripte fertig** (`ollama-schnelltakt*.cmd`); einmalige Registrierung durch den Menschen offen (pm/T-0071) |

## 11. F-Fragen — ENTSCHIEDEN (Auftraggeber, 2026-08-20, pm/D010–D012)

- **F18 Ollama-Schnelltakt → JA, auf dem Team-Node-PC** (pm/D010): Windows-Aufgabenplanung ruft alle 15 min `ollama-schnelltakt.cmd` (`tick.py --provider ollama`, sequenziell über die Einheiten mit `takt: schnell`; Modell `gemma3:27b`). Das erweitert F14 kontrolliert: nur Aufgaben-Typen mit `ollama` in der Kette, gate-relevantes nie. Serialisierung über das Nicht-Parallel-Verhalten der Aufgabenplanung. Einrichtung: `ollama-schnelltakt-einrichten.cmd` (einmalig, Mensch — pm/T-0071).
- **F19 Motor `api` → NEIN** (pm/D011): 0-€-Politik bestätigt; `api` bleibt als geplanter Motor im Schema, wird nicht aktiviert.
- **F20 Mehrfach-Instanzen → vorerst EINE je Rolle und Repo** (pm/D012): Mehrfach (`ROLLE-n@einheit`) erst, wenn ein Projekt real am Durchsatz einer Instanz scheitert — dann Klasse-B-Entscheid des PM.

# Genesis 2.0 — Organisationskonzept (v1.0 — F14–F17 entschieden, Input für P5-G0)

*2026-08-15, PL/ARCH/COACH gemeinsam. Anlass: Auftraggeber-Update (Diagramm + Beschreibungen Projektmanagement/Projekt-Teams). Ziel: aus dem Ein-Team-Betrieb (ASPICE-Team + Projekte) wird eine Organisation mit drei Team-Arten. v0.1 war Entwurf; die vier Kernfragen F14–F17 sind am 2026-08-15 vom Auftraggeber entschieden (p0/D027) — siehe Abschnitt 5.*

## 1. Zielbild (aus dem Diagramm übersetzt)

**Mensch** ↔ **HMI** (Mission Control: Cockpit, Inbox, Briefkasten, E-Mail) ↔ **Projektmanagement-Team** (Drehscheibe) → **Projekt-Teams** (beliebige Domänen: Steuer, Mail-Zusammenfassung, Trading-Analyse, wissenschaftliche Auswertung …) und **ASPICE-Team** (baut System/SW/Tools für alle). Der Mensch wird nur bei Unklarheit, Gates und allem, was Geld/Recht/Außenwirkung hat, hinzugezogen.

## 2. Leitidee der Struktur: Generalisieren, nicht neu erfinden

Fast alles, was die Organisation braucht, existiert als Mechanik und muss nur **verallgemeinert** werden:

| Bild-Element | Vorhandene Mechanik | Nötige Verallgemeinerung |
|---|---|---|
| Projekt-Teams mit eigenem „Jira" | Repo-Konvention `tickets/` + `.git` → Discovery, Board, Inbox, Chat, Cockpit **gratis** | Team-Repo je Team (`team-<name>`), Prozessprofil statt Voll-ASPICE |
| Rollen je Team | `process/roles/` + Registry | Team-Zuordnung + Domänen-Rollen (z. B. STEUER-SB, MAIL-RED) |
| Wissen je Team | `process/knowledge/`, Skills | Wissensbasis-Namespace je Team-Typ |
| Tools vom ASPICE-Team | Produktkatalog + feedback_route + CR-Prozess | Routing-Ziel „Team" ergänzen; Katalog = Schnittstelle |
| PM koordiniert | Playbook, Gates, Decision-Logs, Briefkasten | PM-Team als **Dauerbetrieb-Repo** `pm` (Kanban, keine Sprints) |
| Mensch nur bei Bedarf | Inbox mit Default-Fristen, Frist-Warnung, Entscheider-Registry | **Entscheidungsklassen** (was darf PM allein?) |

### 2.1 Drei Team-Arten mit Prozessprofilen

Der wichtigste neue Baustein. Voll-ASPICE für „Mails zusammenfassen" wäre Bürokratie-Theater; gar kein Prozess wäre Genesis-Verrat. Vorschlag: **drei Profile im Playbook**, jedes Team bekommt bei Gründung genau eines:

- **Profil `entwicklung`** (heute gelebt): volle SWE.1–6 + SUP + Gates G0–G4, Sprints, Baselines. Für ASPICE-Team und SW-Produkte.
- **Profil `dienstleistung`**: MAN.3 + SUP.1/8/9 light, Gates nur G0 (Auftrag) und G4 (Abnahme je Lieferung), Tickets + DoD je Aufgabentyp, keine SWRs/Matrix. Für einmalige/projektartige Lieferungen (wissenschaftliche Analyse, Steuererklärung *eines Jahres*).
- **Profil `wiederkehrend`**: Kanban ohne Sprints, wiederkehrende Tickets mit Takt (täglich/wöchentlich), SLA statt G4 (z. B. „Mail-Digest bis 08:00"), Qualitäts-Stichproben durch PM. Für Daueraufgaben (Mail-Zusammenfassung, Markt-Monitoring).

### 2.2 Das Projektmanagement-Team

Eigenes Repo **`pm`** (Discovery greift sofort → eigenes Board/Chat/Cockpit), Profil `wiederkehrend` + Governance-Sonderrechte. Aufgaben exakt wie in deiner Beschreibung, übersetzt in Mechanik: **Intake** (nimmt Aufträge aus Briefkasten/Chat an, bricht sie herunter), **Staffing** (stellt Team-Profil + Rollen + Skills zusammen, legt Team-Repo aus Template an), **Koordination** (Session-Agenda: welche Teams werden heute getickt — wichtig, siehe Lücke L1), **Wissensversorgung**, **Tool-Bedarf** als CR ans ASPICE-Team, **LeLe** quartalsweise über alle Teams, **Eskalation** an den Menschen nach Entscheidungsklassen. PM übernimmt Problem-/Change-/Qualitäts-/Release-/Config-Management *organisatorisch*; die *Werkzeuge* dafür (board.py, feedback_route, Katalog, Baselines) bleiben ASPICE-Lieferungen.

### 2.3 Abgrenzung PM-Team ↔ ASPICE-Team

ASPICE-Team = einziges Team mit Profil `entwicklung` und Schreibrecht auf `platform`/Produkt-Repos. PM besitzt den Prozessrahmen (process-Repo, Profile, Registry) und bestellt Werkzeuge per CR — baut sie aber nie selbst. Der Kreislauf aus dem Bild: Projekt-Team meldet Bug/CR → feedback_route klassifiziert → PM priorisiert → ASPICE liefert → Katalog → alle Teams nutzen.

### 2.4 Struktur auf der Platte (Zielbild)

```
process/   Governance: Playbook + Profile, roles/, teams/registry.yaml, knowledge/, skills/, catalog/
platform/  Mission Control, Orchestrator, Gateway, Skripte (ASPICE-Eigentum)
pm/        PM-Team (Kanban-Dauerbetrieb, Intake-Queue, Session-Agenda, LeLe)
team-*/    je Projekt-Team ein Repo (Profil, Rollen, Tickets, Briefkasten, Wissensverweise)
p*/        klassische Entwicklungsprojekte (wie bisher)
produkt-*/ Produkte (wie bisher)
```

## 3. Lücken der Aufgabenstellung (ehrlich, priorisiert)

**L1 — Ausführungsmodell (die größte Lücke).** „KI-Agenten erledigen selbständig" — womit? Heute gibt es drei Motoren: Cowork-Session (stark, aber nur wenn sie läuft), Ollama (lokal, klein, kostenlos), Skript-Routen (deterministisch). Daueraufgaben („jeden Morgen Mail-Digest") brauchen einen **Takt ohne Menschen**: Windows-Aufgabenplanung + `tick.py` + Ollama/Skripte ginge heute für einfache Aufgaben; anspruchsvolle Arbeit braucht die Session oder — kostenpflichtig — die Claude-API (Budget-Entscheidung, D012/D026-Kontext). **Ohne diese Entscheidung bleibt „selbständig" ein Wunsch.**

**L2 — Externe Daten und Zugänge.** Steuer braucht Belege, Mail-Digest braucht IMAP-Zugriff aufs Postfach, Trading braucht Marktdaten. Wer gibt welche Zugangsdaten frei, wo liegen sie (Env-Muster existiert), welche Dienste sind erlaubt? Braucht ein **Zugangs-Register + Freigabe-Gate je Dienst** (Runbook-Kap.-8-Muster skaliert).

**L3 — Geld, Recht, Außenwirkung.** Daytrading bewegt echtes Geld; eine Steuererklärung ist eine rechtsverbindliche Abgabe. Klare Team-Position: **KI-Teams analysieren und bereiten vor — handeln (Order ausführen, Erklärung abgeben, Mails versenden) tut ausschließlich der Mensch.** Das gehört als harte Guardrail-Klasse ins Playbook (wie „Sandbox pusht nie"). Zusätzlich ehrlich: Trading-Empfehlungen und Steuerentwürfe sind keine Finanz-/Steuerberatung; Verlustrisiken und Haftung liegen beim Menschen.

**L4 — Datenschutz/Vertraulichkeit.** Private Mails und Steuerdaten in Git-Repos, die zu GitHub gespiegelt werden? F10 („alles privat") reicht nicht mehr. Braucht **Datenklassen**: `oeffentlich-intern` (heutige Repos), `sensibel` (nur lokales Repo, nie gepusht — technisch: .gitignore-Zonen oder Repo ohne Remote), `geheim` (nie in Git, nur Env/lokale Ablage). Je Team-Typ festlegen.

**L5 — Entscheidungsklassen.** Heute entscheidet der Mensch jedes Gate. Mit 5+ Teams wird das der Engpass. Vorschlag: Klasse A (Geld/Recht/Außenwirkung/Team-Gründung) = immer Mensch; Klasse B (Priorisierung, Staffing, Routine-Abnahmen im Profil `wiederkehrend`) = PM entscheidet, Mensch sieht es im Cockpit und kann einsprechen; Klasse C (Arbeitsebene) = Teams selbst. Muss der Auftraggeber absegnen.

**L6 — Wiederkehrende Aufgaben in der Mechanik.** Boards kennen Sprints, keine Takte. Braucht: wiederkehrende Tickets (Vorlage + Fälligkeit), Takt-Auslöser (Aufgabenplanung), SLA-Ampel im Cockpit — ein sauberes ASPICE-Arbeitspaket.

**L7 — Team-Lebenszyklus.** Gründung (wer beschließt? → Klasse A), Template, Wissensübergabe, Pausieren, Archivierung. Intake-Prozess existiert für Projekte, nicht für Teams.

**L8 — Erfolgsmessung je Profil.** Heute: Tests/Matrix/Kosten. Ein Mail-Team braucht Pünktlichkeit/Nützlichkeit, ein Analyse-Team Qualitätsurteile des Menschen. KPI-Modell je Profil fehlt.

**L9 — HMI-Skalierung.** Cockpit/Inbox skalieren technisch (Discovery), aber die *Aufmerksamkeit* des Menschen nicht — Priorisierung, Digest („was heute wichtig war" per Mail), Klasse-B-Ausblendung gehören ins HMI.

## 4. Vorgehensvorschlag (Phasen, jeweils eigenes Projekt mit Gates)

1. **P5 „Genesis 2.0 — Organisationsrahmen":** dieses Konzept → beschlossene Fassung (F-Fragen unten), Prozessprofile + Entscheidungsklassen ins Playbook, Team-Registry + Team-Template, PM-Repo-Hülle mit Kanban. *Reine Governance + kleine Mechanik, 0 €.*
2. **P6 „Pilot-Team":** EIN risikoarmes Team real gründen und 2 Wochen betreiben — Empfehlung: **Mail-Zusammenfassung** (klarer Nutzen, L2 klein, L3 unkritisch, Profil `wiederkehrend` wird real erprobt inkl. L6-Mechanik durch ASPICE).
3. **P7+:** weitere Teams nach Pilot-Lehren; Trading/Steuer erst nach L2–L4-Guardrails (Klasse-A-Beschlüsse).

## 5. F-Fragen — ENTSCHIEDEN (Auftraggeber, 2026-08-15, p0/D027)

- **F14 (L1) → Session-Takt (0 €):** Daueraufgaben laufen im Session-Rhythmus („Briefkasten zuerst"-Muster, Session-Agenda durch PM). Kein Autopilot; Aufgabenplanung/Ollama oder Claude-API später per CR mit eigener Freigabe.
- **F15 (L5) → Klasse B an PM:** PM entscheidet Priorisierung, Staffing und Routine-Abnahmen allein — jede Klasse-B-Entscheidung landet im PM-Decision-Log und ist im Cockpit sichtbar, Einspruch jederzeit. Klasse A (Geld, Recht, Außenwirkung, Team-Gründung, Projektabnahmen) bleibt immer beim Menschen.
- **F16 (P6) → Pilot-Team = Mail-Zusammenfassung** (Profil `wiederkehrend`; IMAP-Lesezugriff wird als L2-Freigabe-Gate im Pilot geklärt).
- **F17 (L3/L4) → beide Guardrails bestätigt (hart):** (1) KI-Teams handeln nie selbst mit Außenwirkung — keine Order, keine Steuerabgabe, kein Mailversand an Dritte; sie bereiten vor, der Mensch führt aus. (2) Sensible private Daten (Mails, Steuerunterlagen) landen nie in Repos mit GitHub-Remote. Verankerung im Playbook mit gleicher Härte wie „Sandbox pusht nie".

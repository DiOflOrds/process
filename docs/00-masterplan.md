# Umsetzungsplan: Halb-autonomes ASPICE-Projektteam („Virtual Engineering Team")

**Version:** 0.6 (G0 erteilt; F1–F4 entschieden; +LLM-Provider-Modell Claude/Copilot/Ollama) · **Datum:** 2026-08-05 · **Autor:** Claude (Cowork) · **Auftraggeber:** Engelchen John
**Status:** Zur Review durch den Auftraggeber

---

## 1. Vision und Zielbild

Es entsteht ein virtuelles Entwicklungsteam aus KI-Agenten und Skripten, das Software-Projekte (perspektivisch System/SW-Projekte) nach Automotive SPICE 4.0 halb-autonom abwickelt. Der Mensch übergibt eine Erwartungshaltung bzw. Anforderungen, präzisiert diese im Dialog mit dem Team und wird danach nur noch an definierten Entscheidungspunkten (Gates) oder bei Problemen, Unstimmigkeiten und kritischen Entscheidungen einbezogen. Alles andere — Organisation, Verwaltung, Umsetzung, Qualitätssicherung, Dokumentation — erledigt das Team selbst.

Zentrale Eigenschaften des Zielbilds:

1. **Rollenbasiert:** Jedes relevante ASPICE-Prozessgebiet hat eine verantwortliche Rolle (Agent), Rollen interagieren über definierte Schnittstellen.
2. **Evidenzbasiert:** Jede Tätigkeit hinterlässt nachvollziehbare Artefakte (Tickets, Commits, Reviews, Baselines) — das ist zugleich die ASPICE-Evidenz.
3. **Halb-autonom:** Autonomie innerhalb definierter Leitplanken; Human-in-the-Loop (HITL) an Gates und bei Eskalationen.
4. **Selbstverbessernd:** Jeder Sprint endet mit einer Retrospektive; Prozesse, Workflows, Tools und Prompts werden über das eigene Change Management verbessert.
5. **Transparent und live:** Ein Frontend (Dashboard/HMI) macht Status, Aufgaben, Requirements, Traceability, Entscheidungen und Baselines für den Menschen jederzeit **in Echtzeit** einsehbar — jede Aufgabe ist ein Ticket, jeder Bearbeitungsschritt ist live sichtbar.
6. **Automatisierung zuerst:** Es gilt die Automatisierungspyramide **Skript → KI-Rolle → Mensch**. Wiederkehrende, deterministische Arbeit erledigen Skripte; KI-Rollen werden nur eingesetzt, wo Urteilsvermögen nötig ist; der Mensch nur, wo Verantwortung, Freigabe oder fachliche Klärung es erfordern. Jede Retrospektive prüft, welche KI-Tätigkeiten inzwischen skriptfähig sind.
7. **Besetzungs-agnostische Rollen:** Eine Rolle ist eine definierte Schnittstelle (Auftrag, Inputs/Outputs, Tickets, Reviews) — besetzt durch Skript, KI-Agent oder Mensch. Dieselbe Plattform trägt damit später auch das **Arbeitsteam**: gemischte Teams aus menschlichen und KI-Fachexperten, die zur Erledigung ihrer Aufgaben auf die vom ASPICE-Team entwickelten SW/Produkte zurückgreifen (Produktkatalog, Kap. 5.5) und diese kombinieren.
8. **Lernende Rollen:** Jede KI-Rolle (im ASPICE-Team wie im späteren Arbeitsteam) besitzt eine versionierte Wissensbasis und verbessert sich kontinuierlich und kontrolliert über einen definierten Lernzyklus (Kap. 5.4).
9. **Geschlossene Feedbackschleifen:** Zwischen Nutzung und Entwicklung besteht ein ständiger Rückkanal — jede Produktnutzung kann Probleme, Änderungswünsche und Nutzungsdaten erzeugen, die automatisch als SUP.9-/SUP.10-Tickets beim entwickelnden Team landen; neue Releases fließen in den Produktkatalog zurück, und der Melder sieht den Status seines Feedbacks live (Kap. 5.5).
10. **Verteilt und ortsunabhängig:** Teams und Rollen agieren in einem Netzwerk über verschiedene Geräte (Laptops, PCs, Smartphones). Möglich wird das, weil der gesamte Projektzustand zentral im Backbone lebt und nie auf einem Gerät: Rollen laufen dort, wo gerade Rechenleistung, Werkzeuge oder der Mensch verfügbar sind (Kap. 5.7).

## 2. Getroffene Grundsatzentscheidungen

| # | Entscheidung | Gewählt | Begründung |
|---|---|---|---|
| E1 | Agent-Runtime | **Claude Agent SDK** als Orchestrierungs-Backbone, darunter ein **LLM-Gateway mit drei Providern**: (1) Claude/Claude Code, (2) GitHub Copilot CLI, (3) Ollama (lokales LLM) — Details Kap. 5.8 | Rollen als Agent-Definitionen mit System-Prompt, Skills und Tools; die Ausführung je Aufgabe ist provider-agnostisch und wird nach Qualität, Kosten, Vertraulichkeit und Verfügbarkeit geroutet. |
| E2 | Daten-/Tool-Backbone | **Bestehende Tools: Git + GitLab** (Repos, Issues, Merge Requests, CI/CD, Releases) | Baselines, Reviews, Traceability und Audit-Trail über bewährte Mechanismen; das Team baut Orchestrierung und Visualisierung *darüber*, nicht alles neu. |
| E3 | ASPICE-Scope Stufe 1 | **SWE.1–SWE.6 + MAN.3, SUP.1, SUP.8, SUP.9, SUP.10** (+ SPL.2 leichtgewichtig); SYS.1/SYS.2 als Stub über die Systemrolle | Fokus Software-Entwicklung; System-Ebene wird als schlanke Durchreiche realisiert und später voll ausgeprägt. |
| E4 | Dokumentationsform | Markdown im Claude-Projekt „Product Developer" + Git | Maschinen- und menschenlesbar, versionierbar, baseline-fähig. |
| E5 | Betriebsumgebung Hub (F1) | **Cloud-VM** (z.B. Hetzner, Docker Compose) — Beschaffung per D014 auf **Sprint 4** verschoben; bis dahin Betrieb auf dem Team-Node (Backend/Ticks), Cowork-Sessions für Engineering | Immer erreichbar, kein Heimnetz-Aufwand; entschieden 2026-08-05 (G0); Zeitplan angepasst 2026-08-06 (D014, via Inbox). |
| E6 | Git-Backbone (F2) | ~~gitlab.com~~ → **GitHub** (kostenloser Account; Repos, Issues, Labels, Milestones, Actions-CI, Releases) | Revidiert per D005: gitlab.com ist aus der Cowork-Cloud-Sandbox nicht erreichbar, GitHub verifiziert voll erreichbar — damit können Agenten-Ticks ab Sprint 1 direkt aufs Board. Umzug auf self-hosted GitLab später möglich; entschieden 2026-08-05 (DR-001/D005). |
| E7 | API-Budget (F3) | **Testphase zuerst:** Sprint 0–1 mit Mini-Budget (~20 €), Ist-Kosten messen, dann Budget datenbasiert festlegen (Review in Sprint 2) | Datenbasierte Entscheidung statt Schätzung; hartes Limit mit Abschaltung gilt ab dem ersten Tick; entschieden 2026-08-05 (G0). |
| E8 | Benachrichtigung (F4) | **E-Mail** (geraldine.john90@gmail.com) + Frontend-Decision-Inbox | E-Mail ab sofort (übergangsweise via Sprint-Report), Inbox ab Sprint 3; entschieden 2026-08-05 (G0). |

F1–F4 sind damit entschieden; noch offene Grundsatzentscheidungen: siehe Kapitel 10 (F5–F12).

## 3. ASPICE-4.0-Grundlage (relevanter Auszug)

Automotive SPICE 4.0 (PAM 4.0, veröffentlicht Dezember 2023) umfasst 32 Prozesse. Für dieses Vorhaben relevant:

**Gewählter Scope (Stufe 1 — Software-Fokus):**

| Prozess | Titel (PAM 4.0) | Abdeckung Stufe 1 |
|---|---|---|
| MAN.3 | Project Management | voll |
| SUP.1 | Quality Assurance | voll |
| SUP.8 | Configuration Management | voll |
| SUP.9 | Problem Resolution Management | voll |
| SUP.10 | Change Request Management | voll |
| SWE.1 | Software Requirements Analysis | voll |
| SWE.2 | Software Architectural Design | voll |
| SWE.3 | Software Detailed Design and Unit Construction | voll |
| SWE.4 | Software Unit Verification | voll |
| SWE.5 | Software Component Verification and Integration Verification | voll |
| SWE.6 | Software Verification | voll |
| SPL.2 | Product Release | leichtgewichtig |
| SYS.1/SYS.2 | Requirements Elicitation / System Requirements Analysis | Stub (Systemrolle nimmt Kundenerwartung auf, leitet SW-Anforderungen ab) |

**Spätere Ausbaustufen:** SYS.3–SYS.5 (Systemarchitektur, -integration, -verifikation), VAL.1 (Validation), MAN.5 (Risk Management, zunächst als Teil von MAN.3), MAN.6 (Measurement, zunächst als Teil der Retrospektive/KPIs), PIM.3 (Process Improvement — de facto von Beginn an gelebt über Retrospektiven), REU.2, sowie perspektivisch HWE.1–4 und MLE.1–4/SUP.11, falls Hardware- oder ML-Anteile dazukommen.

**Für die Umsetzung wichtige 4.0-Neuerungen** (Abweichungen zu 3.1, die das Team von Anfang an richtig leben soll):

- Aus „Work Products" wurden **Information Items** (mit Information Item Characteristics als Prüfmaßstab für den QM).
- **Traceability und Konsistenz** sind je Prozess in einer Basispraktik zusammengeführt; bidirektionale Nachverfolgbarkeit über die gesamte Kette (Stakeholder-Anforderung → SW-Anforderung → Architektur → Detailed Design/Unit → Verifikationsmaßnahmen → Ergebnisse) bleibt Pflicht.
- Der Begriff **„Verification"** ersetzt „Testing" weitgehend; SWE.4–SWE.6 sind neu geschnitten (Unit-Verifikation / Komponenten- und Integrationsverifikation / SW-Verifikation).
- **Strategien** (z.B. CM-Strategie, Verifikationsstrategie) sind auf Capability Level 2 über GP 2.1.1 verankert — wir erstellen sie trotzdem von Beginn an als leichtgewichtige Dokumente, weil sie den Agenten als Arbeitsanweisung dienen.
- Ziel Stufe 1: Arbeitsweise **konform zu CL1** (Basispraktiken werden ausgeführt, Information Items entstehen); CL2-Elemente (Planung, Überwachung, Verantwortlichkeiten) sind durch die Team-Mechanik ohnehin weitgehend abgedeckt und werden in einer späteren Stufe formal nachgezogen.

## 4. Rollenmodell

### 4.1 Rollen und Prozess-Zuordnung

Deine vorgeschlagenen Rollen sind übernommen und um fehlende ergänzt (fett = ergänzt; Begründung in Kap. 11). Jede Rolle ist ein Agent mit eigenem System-Prompt („Rollenkarte"), Prozess-Skills und Tool-Rechten.

| Rolle (Agent) | ASPICE-Prozesse | Kernauftrag |
|---|---|---|
| Projektleiter (PL) | MAN.3, (MAN.5) | Orchestriert alles: Planung, Priorisierung, Aufgabenverteilung, Fortschritts-/Risikoüberwachung, Sprint-Steuerung, Berichte an den Menschen, Eskalationsentscheidung |
| Requirements-Manager (RM) | SYS.1, SWE.1 | Nimmt Kundenerwartung auf, klärt sie mit dem Menschen, verwaltet Stakeholder- und SW-Anforderungen inkl. Attributen, Status, Traceability |
| Systemingenieur (SYS) | SYS.2 (Stub; später SYS.3–SYS.5) | Leitet aus Stakeholder-Anforderungen System-/SW-Anforderungen ab; Schnittstelle Systemebene ↔ SW-Ebene |
| System-/SW-Architekt (ARCH) | SWE.2 (später SYS.3) | Architekturentwurf, Schnittstellen, Design-Entscheidungen mit Begründung (Architecture Decision Records), Bewertung von Alternativen |
| **Entwickler (DEV)** | SWE.3 | Detailed Design, Unit-Konstruktion, Implementierung, Code-Doku — *fehlte im ursprünglichen Konzept* |
| Verifikationsingenieur (TEST) | SWE.4, SWE.5, SWE.6 (später SYS.4/5, VAL.1) | Verifikationsstrategie und -maßnahmen je Ebene (Unit/Komponente+Integration/SW), Testautomatisierung, Ergebnisberichte; unabhängig von DEV |
| Qualitätsmanager (QM) | SUP.1 | Prüft Information Items gegen Checklisten (IIC), Prozess-Compliance, unabhängige Berichtslinie an den Menschen; blockiert Baselines bei Nichterfüllung |
| Konfigurationsmanager (CM) | SUP.8 | CM-Strategie, Repo-/Storage-Struktur, Tool-Übersicht, Baselines, Branching-Regeln, Backup, Zugriffsrechte, IT-Infrastruktur |
| Problemmanager (PROB) | SUP.9 | Bug-/Problem-Tickets: Erfassung, Analyse, Ursache, Priorisierung, Verfolgung bis Abschluss, Trend-Auswertung |
| Change-Manager (CHG) | SUP.10 | Change Requests: Aufnahme, Impact-Analyse (mit ARCH/RM/TEST), Entscheidungsvorlage, Umsetzungsverfolgung — auch für Prozess-Änderungen des Teams selbst |
| **Release-Manager (REL)** | SPL.2 | Stellt Releases zusammen, prüft Freigabevoraussetzungen (alle Tests grün, QM-Freigabe, Baseline), erstellt Release Notes — *fehlte* (kann anfangs der CM mitübernehmen) |
| **Prozess-Coach (COACH)** | PIM.3, MAN.6, Scrum-Master-Funktion | Pflegt Prozessbeschreibungen (Skills/Templates), moderiert Retrospektiven, misst Prozess-KPIs, treibt Verbesserungen — *fehlte* |

**Hinweis Mensch:** Der Mensch hat die Rollen *Auftraggeber/Product Owner* (präzisiert Anforderungen, priorisiert fachlich, nimmt ab) und *Eskalationsinstanz* (entscheidet an Gates und bei Konflikten). Er ist bewusst **keine** operative Rolle.

### 4.2 Unabhängigkeitsregeln (ASPICE-relevant)

- TEST ist organisatorisch von DEV getrennt (eigener Agent, eigener Kontext) — Verifikation prüft gegen Anforderungen, nicht gegen die Implementierungsidee.
- QM ist von allen produktiven Rollen unabhängig und berichtet direkt an den Menschen (eigener Abschnitt im Sprint-Report, ungefiltert durch PL).
- Reviews laufen grundsätzlich als Vier-Augen-Prinzip zwischen *verschiedenen* Agenten (z.B. RM erstellt Anforderung → SYS/ARCH/TEST reviewen; DEV erstellt Code → zweiter DEV-Kontext oder ARCH reviewt via Merge Request).

### 4.3 Interaktionsmodell

Rollen kommunizieren **nicht über freien Chat**, sondern ticket- und artefaktbasiert über das Backbone — jede Interaktion ist damit automatisch dokumentiert und ASPICE-Evidenz:

1. **Aufgaben:** GitLab-Issues mit Prozessgebiet-Label, Rolle (Assignee), Status-Workflow, Verknüpfung zu Anforderungen/MRs.
2. **Übergaben:** Definierte Input/Output-Beziehungen (z.B. RM liefert freigegebene SW-Anforderungs-Baseline → ARCH beginnt; ARCH liefert Architektur + Schnittstellen → DEV und TEST parallel).
3. **Reviews:** Merge Requests (Code, Doku, Prozessänderungen) mit Pflicht-Reviewern nach Rollenmatrix.
4. **Konflikte:** Kann ein Konflikt zwischen Rollen nicht durch Daten/Kriterien aufgelöst werden, entscheidet PL; betrifft es Qualität/Scope/Budget/Architektur-Grundsätze → Decision Request an den Menschen (siehe Kap. 7).

## 5. Systemarchitektur

### 5.1 Komponentenübersicht

```
┌────────────────────────────────────────────────────────────────┐
│  Mensch (Browser)                                              │
│  Frontend „Mission Control": Dashboard, Board, Requirements,   │
│  Traceability, Decision-Inbox (HITL), KPIs, Baselines, Logs    │
└───────────────▲────────────────────────────────────────────────┘
                │ REST/WebSocket
┌───────────────┴────────────────────────────────────────────────┐
│  Backend (FastAPI, PostgreSQL)                                 │
│  - Aggregation GitLab-API (Issues, MRs, Pipelines, Releases)   │
│  - Traceability-Service (Anforderungs-Graph, Matrix)           │
│  - HITL-Queue (Decision Requests, Benachrichtigungen)          │
│  - Agent-Run-Registry (wer lief wann, Kosten, Ergebnis)        │
│  - KPI-/Metrik-Service (MAN.6 light)                           │
└───────────────▲────────────────────────────────────────────────┘
                │
┌───────────────┴────────────────────────────────────────────────┐
│  Agent-Layer (Claude Agent SDK, Python)                        │
│  - Orchestrator = PL-Agent (zyklisch + eventgetrieben)         │
│  - Rollen-Agents als Subagents/Worker (RM, ARCH, DEV, TEST,    │
│    QM, CM, PROB, CHG, REL, COACH)                              │
│  - Rollen = System-Prompt („Rollenkarte") + Prozess-Skills     │
│    (SKILL.md je Prozessgebiet) + Tool-Rechte                   │
│  - Guardrails: Kosten-Limits, Schreibrechte, Gate-Checks       │
└───────────────▲────────────────────────────────────────────────┘
                │ Git/API
┌───────────────┴────────────────────────────────────────────────┐
│  Backbone: GitLab                                              │
│  - Repos: process/ (Prozesse, Skills, Templates, Checklisten)  │
│           platform/ (Orchestrator, Backend, Frontend)          │
│           <projekt>/ (Anforderungen, Architektur, Code, Tests) │
│  - Issues (Tasks, Probleme SUP.9, CRs SUP.10, Decisions)       │
│  - Merge Requests (Reviews), CI/CD (Build, Test, Checks)       │
│  - Tags/Releases (Baselines SUP.8, Releases SPL.2)             │
└────────────────────────────────────────────────────────────────┘
```

### 5.2 Wichtige Architektur-Festlegungen

**Requirements als Code:** Anforderungen liegen als strukturierte Dateien (Markdown/YAML mit stabilen IDs, Attributen wie Status/Priorität/Quelle/Verifikationskriterium und expliziten Links) im Git — pro Ebene ein Verzeichnis (stakeholder/, sw-requirements/). Traceability wird per Skript aus den Links generiert und im CI geprüft (Fehler bei toten Links, unverlinkten Anforderungen, fehlenden Verifikationsmaßnahmen). Optional evaluieren wir in P0 die Open-Source-Tools **Doorstop** oder **StrictDoc** (beide git-basiert), bevor wir Eigenbau festschreiben.

**Baselines:** Eine Baseline ist ein Git-Tag über den relevanten Repos plus ein generiertes Baseline-Manifest (welche Information Items in welcher Version, Prüfstatus des QM). Baseline-Erstellung ist ein CM-Workflow mit QM-Freigabe; Requirements-Baselines zusätzlich mit Mensch-Freigabe (Gate).

**Prozesse als Skills — der Selbstverbesserungs-Trick:** Alle Prozessbeschreibungen, Rollenkarten, Templates und Checklisten liegen im Repo `process/` und werden den Agenten als Skills geladen. Eine Prozessverbesserung aus der Retrospektive ist damit ein ganz normaler Change Request (SUP.10) mit Merge Request auf `process/` — reviewt, versioniert, sofort für alle Agenten wirksam. Prozessverbesserung nutzt also exakt die Mechanik, die das Team ohnehin beherrscht.

**Betriebsmodell (Ticks statt Dauerlauf):** Der Orchestrator läuft nicht permanent, sondern in **Ticks**: zyklisch per Scheduler (z.B. mehrmals täglich) und eventgetrieben (Mensch antwortet auf Decision Request, CI wird rot, neues Ticket). Je Tick: Board lesen → planen/priorisieren → Aufgaben an Rollen-Agents delegieren (parallel, wo unabhängig) → Ergebnisse einsammeln → Status, Board und Bericht aktualisieren. Das begrenzt Kosten, macht Läufe reproduzierbar und passt zum Sprint-Rhythmus.

**Modell-Zuordnung (Kostenhebel):** Anspruchsvolle Rollen (PL-Planung, ARCH, schwierige DEV-Aufgaben, QM-Bewertungen) laufen auf einem starken Modell; mechanische Aufgaben (Statuspflege, Report-Generierung, Link-Checks) auf einem günstigen Modell oder als deterministische Skripte ohne LLM. Grundsatz: **Was ein Skript kann, macht ein Skript** — LLM nur für Urteilsvermögen.

### 5.3 Rollen-Registry: Besetzung durch Skript, KI oder Mensch

Jede Rolle wird in einer **Rollen-Registry** (`process/roles/registry.yaml`) geführt mit: Rollen-Schnittstelle (Auftrag, Inputs/Outputs, Ticket-Typen), Besetzungstyp (**script | ki | mensch**) und je Aufgaben-Typ der Ausführungsweg. Konsequenzen:

- **Eskalationskette je Aufgabe:** Zuerst prüft der Orchestrator, ob ein registriertes Skript die Aufgabe deterministisch lösen kann; erst dann wird die KI-Rolle aufgerufen; diese eskaliert bei Bedarf an den Menschen. Der gewählte Weg wird am Ticket protokolliert (Kosten-/Qualitäts-KPI).
- **Automatisierungs-Backlog:** Erkennt eine KI-Rolle (oder die Retro), dass eine Tätigkeit regelhaft geworden ist, entsteht ein Ticket „Skriptifizierung" — die KI schreibt das Skript, TEST verifiziert es, danach übernimmt das Skript. So sinkt der KI-Anteil pro Projekt systematisch.
- **Arbeitsteam-Fähigkeit:** Menschliche Fachexperten werden wie Agenten an Rollen gebunden (gleiche Tickets, gleiche Reviews, gleiche DoD) — sie erhalten ihre Aufgaben über das Frontend statt über den Orchestrator. Damit trägt dieselbe Mechanik später gemischte Mensch/KI-Expertenteams, die mit dem entwickelten System/SW fachlich arbeiten.

### 5.4 Lernzyklus der KI-Rollen (kontrolliertes Dazulernen)

Jede KI-Rolle lernt ständig dazu — aber nie unkontrolliert, sondern über versionierte Artefakte:

1. **Wissensbasis je Rolle:** `process/knowledge/<rolle>/` enthält kuratierte Lessons Learned, bewährte Muster/Beispiele („Gold-Beispiele"), bekannte Fallstricke und rollenspezifische Heuristiken. Sie wird dem Agenten bei jedem Einsatz zusätzlich zur Rollenkarte geladen.
2. **Feedback-Erfassung:** Review-Findings, QM-Findings, Probleme, Nacharbeit und KPI-Ausreißer werden automatisch der verursachenden Rolle zugeordnet (Skript, nicht LLM).
3. **Kuratierung:** Der COACH destilliert daraus je Sprint Kandidaten für Wissensbasis-Updates (neue Regel, besseres Beispiel, Prompt-Schärfung, neue Checklisten-Zeile — oder ein Skriptifizierungs-Ticket).
4. **Kontrollierte Übernahme:** Updates laufen als Prozess-CR (SUP.10) mit Review durch die betroffene Rolle und QM; Wirkung wird im Folgesprint gemessen (First-Pass-Yield der Rolle). Bei Verschlechterung: Revert per Git.
5. **Schutz vor Drift:** Wissensbasen haben ein Größenbudget (Kuratieren heißt auch Löschen), und Gold-Beispiele werden als Regressionstest genutzt (Rolle bearbeitet Referenzaufgaben, Ergebnis wird mit Soll verglichen), bevor ein Update gemergt wird.

### 5.5 Produktkatalog und Feedbackschleifen (Arbeitsteam ↔ ASPICE-Team)

Die Brücke zwischen Entwicklung und Nutzung besteht aus zwei Mechanismen:

**Produktkatalog (REU.2 light):** Jedes Release des ASPICE-Teams (SPL.2) registriert das Produkt automatisch im Katalog (`catalog/products.yaml` + Detailseite je Produkt): Name, Version, Fähigkeiten („was kann es, wofür geeignet"), Schnittstelle (CLI, API, Bibliothek — bevorzugt zusätzlich als **MCP-Tool/Agent-Tool verpackt**, damit KI-Rollen es direkt aufrufen können), Doku, bekannte Einschränkungen, zuständiges Projekt. Arbeitsteam-Rollen (KI wie Mensch) wählen bei einer Aufgabe passende Produkte aus dem Katalog aus und kombinieren bei Bedarf mehrere Produkte zu einer Lösung; das verwendete Produkt-Set wird am Aufgaben-Ticket protokolliert. Auch das ASPICE-Team selbst und die Skript-Route der Rollen-Registry greifen auf Katalog-Produkte als Werkzeuge zurück — einmal Gebautes wird überall wiederverwendet.

**Ständige Feedbackschleifen (geschlossen, ticket-basiert):**

1. **Erfassung:** Jede Produktnutzung kann Feedback erzeugen — automatisch (Fehler-/Crash-Reports, Nutzungsstatistiken per Skript) und explizit (Rolle oder Mensch meldet per Feedback-Ticket: Bug, Änderungswunsch, Usability-Hinweis, Lob/Bewertung).
2. **Routing (Skript):** Feedback wird automatisch dem Produkt und damit dem zuständigen Entwicklungsprojekt zugeordnet: Bugs → SUP.9-Problem, Wünsche → SUP.10-CR, Sonstiges → Produkt-Backlog. Kein Feedback versandet: jedes bekommt ein Ticket mit Status.
3. **Verarbeitung:** Das ASPICE-Team priorisiert Feedback in seiner normalen Sprint-Mechanik (PL, mit Nutzungshäufigkeit und Schwere als Priorisierungs-Input); Lösungen erreichen den Katalog als neue Release-Version, das Arbeitsteam wird über Release Notes benachrichtigt.
4. **Geschlossene Schleife:** Der Melder (KI-Rolle oder Mensch) sieht den Status seines Feedbacks live im Frontend und wird bei Lösung benachrichtigt; Nutzungserfahrungen fließen zusätzlich in die Wissensbasen der nutzenden Rollen ein („welches Produkt eignet sich wofür").
5. **Produkt-KPIs:** Nutzungshäufigkeit je Produkt, Problemrate, Time-to-Fix, Zufriedenheitsbewertung — sichtbar im Frontend; Produkte ohne Nutzung oder mit hoher Problemrate werden in der Retro thematisiert (weiterentwickeln, überarbeiten oder aussortieren).

### 5.6 Frontend „Mission Control" (HMI)

Sichten für den Menschen: (1) Projektübersicht mit Ampelstatus, Sprint-Fortschritt, Risiken; (2) **Decision-Inbox** — offene Entscheidungen mit Kontext, Optionen, Empfehlung und Ein-Klick-Antwort; (3) Sprint-Board (Tickets je Rolle/Status) — **live**: Statusänderungen und laufende Bearbeitungen erscheinen in Echtzeit via WebSocket, inkl. „wer/was arbeitet gerade woran" (Agent-Tick, Skript-Lauf, wartend auf Mensch); (4) Requirements-Browser mit Status und Herkunft; (5) Traceability-Matrix (Anforderung → Architektur → Code → Test → Ergebnis); (6) Workproduct-/Baseline-Übersicht mit Storage-Locations (CM-Sicht); (7) Problem- und CR-Listen mit Trends; (8) Agent-Aktivitätslog inkl. Kosten und Ausführungsweg (Skript/KI/Mensch); (9) KPI-/Retro-Sicht inkl. Automatisierungsgrad und Lernkurve je Rolle; (10) Aufgaben-Sicht für menschliche Rollen-Inhaber (Arbeitsteam: meine Tickets, meine Reviews); (11) Produktkatalog-Sicht mit Versionen, Produkt-KPIs und Status des eigenen Feedbacks. Benachrichtigung bei neuen Decision Requests per E-Mail (Kanal offen, siehe Fragen).

### 5.7 Verteilte Infrastruktur: Betrieb im Netzwerk über Laptops, PCs und Smartphones

**Grundmodell Hub-and-Spoke:** Es gibt genau eine zentrale, dauerhaft erreichbare Drehscheibe (**Hub**) und beliebig viele Geräte als **Team-Nodes** (Spokes). Der Hub trägt alles, was immer verfügbar sein muss: GitLab (bzw. Anbindung an gitlab.com), Backend inkl. Aufgaben-Queue, Frontend, Datenbank, Scheduler und die Guardrails. Die Team-Nodes führen Arbeit aus oder dienen dem Menschen als Zugang. Empfohlener Hub: eine kleine Cloud-VM oder ein Heimserver mit Docker Compose (GitLab/Runner optional, Backend, Frontend, Queue z.B. Redis oder NATS, PostgreSQL, Reverse-Proxy mit TLS).

**Warum das mit unserer Architektur trivial zusammenpasst:** Es gilt bereits das Prinzip, dass kein Projektzustand nur im Kontext eines Agenten lebt — alles liegt in Git, Tickets und Backend-DB. Rollen sind dadurch **ortstransparent**: Es ist egal, auf welchem Gerät ein Rollen-Agent oder ein Skript läuft; er holt sich Aufgabe und Zustand vom Hub und schreibt Ergebnisse dorthin zurück. Ein Gerät ist damit reine, austauschbare Ausführungskapazität.

**Team-Node (Laptop/PC):** Ein leichtgewichtiger Runner-Dienst (Python-Service oder Docker-Container), der sich beim Hub registriert und dabei seine **Fähigkeiten** meldet: Betriebssystem, CPU/GPU/RAM, installierte Werkzeuge (Compiler, Simulatoren, Testumgebungen), erlaubte Rollen, Verfügbarkeitsfenster (z.B. „Laptop: abends; PC: immer wenn an"). Der Node **zieht** passende Aufgaben aus der Queue (Pull-Prinzip, kein Push): Der Orchestrator plant rollen- und fähigkeitsbezogen („SWE.4-Verifikation braucht Node mit Toolchain X"), die Queue vergibt die Aufgabe an den nächsten passenden freien Node. Spezialgeräte (z.B. ein PC mit angeschlossener Zielhardware für spätere Embedded-Projekte) werden so natürlich eingebunden — als Node mit einzigartiger Fähigkeit.

**Robustheit bei kommenden und gehenden Geräten:** Aufgabenvergabe per **Lease**: Ein Node erhält eine Aufgabe mit Zeit-Lease und sendet Heartbeats; klappt der Laptop zu oder fällt das WLAN aus, läuft die Lease ab und die Aufgabe geht automatisch zurück in die Queue (Ergebnisse werden nur atomar bei Abschluss übernommen — halbe Arbeit hinterlässt keinen inkonsistenten Zustand, unfertige Branches bleiben als solche markiert). Kein Gerät ist ein Single Point of Failure außer dem Hub; der Hub selbst wird per Backup (CM-Runbook) gesichert.

**Kommunikation und Sicherheit:** Nodes verbinden sich ausschließlich **ausgehend** zum Hub (HTTPS/WebSocket) — keine offenen Ports auf Privatgeräten, funktioniert hinter jedem Router/NAT. Für private Netze zusätzlich empfohlen: ein leichtgewichtiges Overlay-VPN (WireGuard/Tailscale), dann ist der Hub nicht öffentlich exponiert. Jedes Gerät hat eine eigene Identität im **Geräteregister** (Teil der Rollen-Registry-Welt): eigenes Token, minimale Rechte (nur seine Rollen/Repos), einzeln widerrufbar; jede Node-Aktion erscheint im Aktivitätslog mit Geräteangabe. Verlorenes Gerät = Token sperren, fertig.

**Smartphone:** Primär **HMI, nicht Rechenknoten**: Das Frontend wird als PWA (progressive Web-App) gebaut — damit sind Live-Board, Decision-Inbox mit Push-Benachrichtigung, Feedback-Tickets, Reviews und Freigaben (Gates!) vom Handy aus möglich. Menschliche Rollen-Inhaber des Arbeitsteams können so unterwegs Entscheidungen treffen, Aufgabenstatus prüfen und Feedback geben; die eigentliche Agent-Rechenarbeit bleibt auf Laptops/PCs/Hub. (KI-Rollen selbst brauchen ohnehin primär API-Zugriff, kaum lokale Rechenleistung — rechenintensiv sind eher Builds, Testläufe und Simulationen, und die gehören auf fähige Nodes.)

**Betriebsregeln (CM verantwortet):** Geräteregister mit Inventar und Rechten ist Teil der CM-Tool-/Storage-Übersicht (SUP.8); neue Geräte werden per dokumentiertem Onboarding-Skript aufgenommen (Freigabe durch den Menschen als Gate — kein Gerät nimmt unbemerkt teil); Nodes patchen/aktualisieren sich über den Runner selbst; die Retro bewertet Auslastung und Engpässe der Nodes (KPI: Wartezeit von Aufgaben auf passende Nodes).

### 5.8 LLM-Provider-Modell: Claude, GitHub Copilot CLI und Ollama

Es gibt drei LLM-Arten im System — jede mit eigener Rolle im Gesamtbild, alle hinter einer gemeinsamen Schnittstelle:

| Provider | Art | Stärken | Typischer Einsatz |
|---|---|---|---|
| **Claude** (Agent SDK / Claude Code) | Cloud-API, voll agentisch (Tools, Subagents, Skills, große Kontexte) | Höchste Qualität und Urteilsvermögen | Standardweg im Hub: PL-Planung, ARCH, QM-Bewertungen, RM, schwierige DEV-/TEST-Aufgaben; alles Gate-/Baseline-Relevante |
| **GitHub Copilot CLI** | Cloud, agentisch im Repo/Terminal, **Abo-Flatrate** | Coding-Arbeit ohne API-Kosten pro Token; entlastet das Claude-Budget | DEV-Routineaufgaben auf Team-Nodes: Implementierung nach klarer Spezifikation, Refactorings, Testcode, Doku-Gerüste |
| **Ollama** (lokales LLM auf Team-Nodes) | Lokal, offline-fähig, kostenlos, Daten verlassen das Gerät nicht | Datenschutz, Null-Kosten, Verfügbarkeit auch ohne Internet/Budget | Mechanische Textarbeit: Ticket-Vorklassifikation, Zusammenfassungen, Entwurfsprosa, Log-Analyse; vertrauliche Inhalte (F10) |

**LLM-Gateway (platform/):** Einheitliche Schnittstelle `execute(rolle, aufgabe, kontext) → ergebnis` mit drei Executor-Plugins: `claude` (Agent SDK, headless), `copilot` (Copilot CLI im programmatischen Modus auf einem Repo-Checkout des Team-Nodes), `ollama` (lokale HTTP-API; text-only oder mit minimalem Tool-Loop). Jeder Executor liefert dasselbe Format zurück (Ergebnis, Artefakte/MR, Log, Kosten) — für Orchestrator, Tickets und Run-Registry ist der Provider nur ein Attribut.

**Capability-Klassen und Routing-Kette:** Die Rollen-Registry gibt je Aufgaben-Typ die nötige Fähigkeitsklasse und eine Provider-Präferenzkette an: *agentic-full* (nur Claude), *agentic-repo* (Copilot oder Claude), *text-only* (Ollama, dann günstiges Claude-Modell). Beispiele: QM-Bewertung → Claude only; DEV-Task nach Spezifikation → Copilot → Claude; Ticket-Vorklassifikation → Ollama → Claude-cheap. Die vollständige Pyramide lautet damit: **Skript → Ollama (lokal, frei) → Copilot (Flatrate) → Claude (API-Budget) → Mensch.** Ist ein Provider nicht verfügbar (Node offline, Budget erschöpft, Abo fehlt), greift die Kette automatisch weiter — ein Budget-Stopp bedeutet keinen Stillstand: mechanische Arbeit läuft lokal weiter, nur urteilsintensive Arbeit wartet.

**Team-Node-Integration:** Nodes melden ihre Provider als Fähigkeiten (Copilot CLI authentifiziert? Ollama installiert, mit welchen Modellen/VRAM?); der Orchestrator plant providerbezogen — eine Copilot-Aufgabe geht nur an einen Node mit gh-Login, eine Ollama-Aufgabe nur an einen Node mit passendem Modell. Der Hub selbst nutzt primär Claude.

**Qualitätssicherung provider-unabhängig:** Es gelten dieselben DoD, Reviews und QM-Checks, egal welcher Provider gearbeitet hat — ein Copilot- oder Ollama-Ergebnis wird wie jedes andere durch eine andere Rolle reviewt (Review-Instanz läuft auf Claude oder beim Menschen). Die Leistung je Provider und Aufgaben-Typ wird gemessen (First-Pass-Yield, Nacharbeit, Kosten) und über Gold-Beispiele verglichen; die Routing-Ketten werden datenbasiert per Prozess-CR nachgeschärft — Provider-Wahl ist Lernzyklus-Gegenstand, kein Dogma.

## 6. Human-in-the-Loop-Konzept

### 6.1 Feste Gates (Mensch muss freigeben)

| Gate | Zeitpunkt | Freigegeben wird |
|---|---|---|
| G0 Projektauftrag | Projektstart | Präzisierter Auftrag, Scope, Budgetrahmen, Randbedingungen |
| G1 Anforderungs-Baseline | Ende Anforderungsphase (und bei relevanten Änderungen) | Stakeholder-/SW-Anforderungen als Baseline |
| G2 Architektur-Grundsatz | Nach SWE.2-Erstentwurf | Technologie-/Architekturentscheidungen mit langfristiger Bindungswirkung |
| G3 Release | Vor jeder Auslieferung (SPL.2) | Release-Inhalt, bekannte Restpunkte, Freigabe |
| G4 Sprint-Review | Je Sprint (asynchron möglich) | Kenntnisnahme Bericht; Einsprüche innerhalb definierter Frist, sonst gilt „genehmigt" |

### 6.2 Ereignisbasierte Eskalation (Team meldet sich)

Eskalationskriterien (initial, vom Team per Retro verfeinerbar): widersprüchliche oder nicht klärbare Anforderungen; Konflikt zwischen Rollen ohne kriterienbasierte Auflösung; prognostizierte Budget-/Termin-Überschreitung > definierter Schwelle; Probleme mit Prioritätsstufe „kritisch"; Sicherheits-/Datenschutz-relevante Sachverhalte; jede Aktion außerhalb der Leitplanken (z.B. externe Veröffentlichung, Kosten über Limit, Löschen von Baselines).

**Mechanik:** Rolle erstellt „Decision Request"-Ticket → PL prüft, ob teamintern lösbar → wenn nein: DR wird qualifiziert (Kontext, Optionen mit Konsequenzen, Team-Empfehlung, Antwortfrist, Default-Verhalten bei Fristablauf) → Decision-Inbox + Benachrichtigung → Antwort des Menschen wird als Entscheidung protokolliert (Decision Log) und entblockt die Arbeit. Nicht-blockierende Arbeit läuft währenddessen weiter.

### 6.3 Leitplanken (hart, technisch durchgesetzt)

Kosten-Limit pro Tick/Sprint/Projekt mit Abschaltung und Meldung; Schreibrechte der Agenten nur auf eigene Repos/Bereiche; keine externen Kommunikationskanäle außer der Decision-Inbox; destruktive Operationen (Force-Push, Löschen von Tags/Baselines) technisch gesperrt; jede Agent-Aktion wird geloggt (Run-Registry).

## 7. Arbeitsweise (Kurzfassung — Details im Team-Playbook)

Das Team arbeitet agil in Sprints (empfohlen: 1 Woche Kadenz zum Start, primär getaktet durch die Feedback-Geschwindigkeit des Menschen, nicht durch Agent-Geschwindigkeit). Zeremonien: Sprint Planning (PL mit Rollen), tägliche Sync-Ticks (Statuspflege, Blocker-Erkennung), Sprint Review (Bericht + Demo-Artefakte an den Menschen), Retrospektive (datenbasiert: KPIs, Probleme, Prozess-Verbesserungs-CRs). Definition of Done je Information-Item-Typ mit QM-Checklisten. Alle Details: `01-team-playbook.md`.

## 8. Roadmap

| Phase | Inhalt | Ergebnis |
|---|---|---|
| **P0 „Genesis"** (initiales Projekt, 7 Sprints: 0–6) | Team befähigt sich selbst: Infrastruktur, Rollen, Prozesse, Backend/Frontend-MVP, End-to-End-Durchstich an einem Mini-Übungsprodukt | Arbeitsfähiges Team, das ein kleines SW-Projekt nach ASPICE-Scope Stufe 1 abwickeln kann; Abnahme durch Mensch |
| **P1 Pilot** | Erstes echtes kleines SW-Produkt deiner Wahl durch das Team | Validierung der Arbeitsweise am Ernstfall; Härtung via Retros |
| **P2 Ausbau** | SYS.3–SYS.5, VAL.1 voll; MAN.5/MAN.6 formalisieren; CL2-Lücken schließen; Multi-Projekt-Fähigkeit; **Multi-Node-Betrieb produktiv** (Team-Node-Runner auf mehreren Laptops/PCs, Geräteregister, Lease-Queue, PWA mit Push) | System/SW-Projekte; mehrere Projekte parallel auf verteilter Infrastruktur |
| **P3 Arbeitsteam** | Gemischte Mensch/KI-Expertenteams auf der Plattform: menschliche Rollen-Inhaber (Frontend-Aufgabensicht, Reviews, Übergaben), KI-Fachexperten-Rollen mit eigenen Wissensbasen; voller Produktkatalog (Produkte als Agent-Tools/MCP nutzbar, auch kombiniert für eine Aufgabe); ständige Feedbackschleifen Nutzung → Entwicklung (automatisches Routing in SUP.9/SUP.10, Produkt-KPIs, geschlossener Status-Rückkanal) | Plattform trägt Entwicklungs- **und** Arbeitsteams; Produkte werden genutzt, bewertet und dadurch besser |
| **P4 Optional** | HWE-/MLE-Plug-ins, formales Self-Assessment, externe Integrationen | Nach Bedarf |

Detaillierte P0-Beschreibung: `02-initialprojekt-p0.md`.

## 9. Risiken und Gegenmaßnahmen

| Risiko | Wirkung | Gegenmaßnahme |
|---|---|---|
| LLM-Halluzination in Anforderungen/Doku | Falsche Inhalte pflanzen sich durch Traceability fort | Vier-Augen-Reviews zwischen Rollen, QM-Checklisten, CI-Konsistenzprüfungen, kritische Artefakte nur mit Mensch-Gate |
| Kostenexplosion (Token) | Budget weg, Vertrauen weg | Harte Limits je Tick/Sprint, Modell-Staffelung, Skripte statt LLM wo möglich, Kosten-KPI im Dashboard |
| Kontextverlust zwischen Ticks | Agenten „vergessen" Projektstand | Zustand liegt vollständig im Backbone (Tickets, Dateien, Logs), nie nur im Agenten-Kontext; jeder Tick startet mit Board-Lektüre |
| Scheinkonformität („Papier-ASPICE") | Prozesse existieren, werden aber nicht gelebt | Evidenz = Arbeitsartefakt (gleiche Quelle), QM prüft stichprobenartig gegen echte Tätigkeit, Self-Assessment in P0-Sprint 6 |
| Überkomplexität des Eigenbaus | P0 wird nie fertig | MVP-Disziplin: GitLab-Bordmittel zuerst, Frontend nur lesend + Decision-Inbox zuerst, Ausbau iterativ |
| Mensch wird Flaschenhals | Team wartet ständig | Klare Gate-Liste, Default-Verhalten bei Fristablauf, asynchrone Reviews, nicht-blockierendes Weiterarbeiten |
| Ein Agent „überschreibt" Arbeit anderer | Inkonsistenzen | Schreibrechte je Rolle, alles über MRs mit Pflicht-Review, Branch-Schutz |

## 10. Offene Fragen an dich

~~F1–F4 wurden am 2026-08-05 im Rahmen von Gate G0 entschieden — siehe E5–E8 in Kapitel 2 und Decision Log D001–D004 im P0-Repo.~~ Offen bleiben (Antworten bis Ende Sprint 2 erbeten):

5. **F5 Sprache der Workproducts:** Deutsch, Englisch oder gemischt (Empfehlung: Englisch für Requirements/Code-Artefakte — assessment- und tool-üblich; Deutsch für Berichte an dich)?
6. **F6 Zielprodukte:** Reine Software (Libraries, Apps, Services) oder auch Embedded mit Cross-Compiler/Target-Hardware? (Beeinflusst CI und Test-Infrastruktur erheblich.)
7. **F7 Anspruch an Konformität:** „ASPICE-angelehnt, pragmatisch" oder Ziel eines belastbaren Self-Assessments CL1 (später CL2)?
8. **F8 Parallele Projekte:** Wie viele Projekte soll das Team mittelfristig parallel stemmen? (Beeinflusst Mandanten-Design von Backend/Frontend.)
9. **F9 Weitere Nutzer:** Bleibt es bei dir als einzigem Menschen, oder sollen später mehrere Personen (Rollen: Auftraggeber, Beobachter) zugreifen?
10. **F10 Vertraulichkeit:** Dürfen Projektinhalte in Cloud-Dienste (GitLab.com, Anthropic API), oder gibt es Inhalte, die lokal bleiben müssen?
11. **F11 Arbeitsteam:** Für welche Fachdomäne(n) ist das spätere Mensch/KI-Arbeitsteam gedacht, und in welchem Verhältnis steht es zum ASPICE-Team — nutzt es (a) die entwickelten Produkte fachlich, (b) dieselbe Plattform für eigene (Nicht-Entwicklungs-)Projekte, oder (c) beides? Und welche menschlichen Experten außer dir sollen Rollen übernehmen können?
12. **F12 Geräte-Landschaft:** Welche Geräte sollen als Team-Nodes mitwirken (wie viele Laptops/PCs, Betriebssysteme, besondere Fähigkeiten wie GPU oder angeschlossene Hardware)? Gibt es ein Gerät, das als dauerhaft laufender Hub dienen kann (Heimserver/NAS), oder soll der Hub in die Cloud (vgl. F1)? Und sollen die Geräte über ein VPN (z.B. Tailscale/WireGuard) verbunden werden?
13. **F13 Provider-Zugänge:** Besteht ein GitHub-Copilot-Abo (welcher Plan — Pro/Business?), und auf welchem Gerät ist die Copilot CLI eingeloggt? Für Ollama: Welches Gerät hat wie viel RAM/VRAM (bestimmt die nutzbaren Modellgrößen), und gibt es Modell-Präferenzen (z.B. Llama, Qwen, Mistral)?

## 11. Was im ursprünglichen Konzept fehlte (Lücken-Analyse)

1. **Die Entwicklerrolle (SWE.3):** Es waren Rollen für Anforderungen, Architektur, Test, QM etc. vorgesehen — aber niemand, der entwirft und implementiert. Ergänzt als DEV.
2. **Differenzierung der Verifikation:** „Testing" ist in ASPICE 4.0 drei Prozesse (SWE.4/5/6) mit unterschiedlichen Bezugsobjekten; zudem ist Unabhängigkeit vom Entwickler nötig. Ergänzt als TEST-Rolle mit drei Ebenen.
3. **Release-Management (SPL.2):** Wer entscheidet, was wann in welchem Zustand ausgeliefert wird, fehlte. Ergänzt als REL (anfangs bei CM).
4. **Prozess-Owner/Verbesserungsrolle (PIM.3):** „Das Team verbessert sich ständig" braucht einen Kümmerer mit Mechanik (Retro → Improvement-Backlog → Prozess-CR). Ergänzt als COACH.
5. **Messung (MAN.6):** Verbesserung ohne Metriken ist Bauchgefühl. KPIs (Durchlaufzeit, Fehlerdichte, Review-Findings, Kosten je Feature, Nacharbeitsquote) von Beginn an.
6. **Risikomanagement (MAN.5):** Zunächst als MAN.3-Bestandteil (Risikoliste des PL), später eigenständig.
7. **Decision Log:** Für Halb-Autonomie zentral — jede Mensch- und jede wichtige Team-Entscheidung wird mit Kontext und Begründung protokolliert (auch Assessment-Evidenz GP-Ebene).
8. **Leitplanken/Autonomiegrenzen:** Explizite technische Grenzen (Kosten, Schreibrechte, verbotene Aktionen) statt nur „bei Bedarf fragt das Team".
9. **Abnahmeprozess:** Nicht nur Anforderungs-Eingang, auch Abnahme des Ergebnisses durch den Menschen (G3/G4) inkl. Kriterien.
10. **Wissens-Persistenz über Projekte hinweg (REU.2 light):** Lessons Learned, wiederverwendbare Komponenten, Prompt-/Prozessbibliothek — sonst lernt jedes Projekt von vorn.
11. **Safety/Security:** ISO 26262 / ISO 21434 / Datenschutz sind in Stufe 1 bewusst out of scope — muss aber als explizite Projektauftrags-Randbedingung je Projekt festgehalten werden (kein sicherheitsrelevanter Einsatz der Ergebnisse).
12. **Betrieb der eigenen Plattform:** Das Team baut SW (Backend/Frontend/Orchestrator) — jemand muss sie betreiben, überwachen, updaten (bei CM verankert, „Plattform-Betrieb").

## 12. Nächste Schritte

1. Du beantwortest F1–F4 (Rest kann nachlaufen).
2. Freigabe dieses Plans (= Gate G0 für Projekt P0).
3. Start P0 Sprint 0 gemäß `02-initialprojekt-p0.md`.

---

*Referenzen: Automotive SPICE PAM 4.0 (VDA QMC, Dez. 2023); Überblick der 4.0-Änderungen u.a. UL Solutions / Kugler Maag („Automotive SPICE 4.0 — What is new").*

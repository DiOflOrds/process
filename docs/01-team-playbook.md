# Team-Playbook: Arbeitsweise des virtuellen ASPICE-Teams

**Version:** 0.6 (Entwurf) · **Datum:** 2026-08-06 · **Gültig für:** alle Projekte des Teams
**Pflege:** Dieses Dokument gehört dem Prozess-Coach (COACH); Änderungen laufen als Change Request (SUP.10) über Review.

---

## 1. Grundprinzipien

1. **Ticket-getrieben:** Keine Arbeit ohne Ticket. Jede Tätigkeit eines Agenten referenziert ein Issue mit Prozessgebiet-Label. Damit ist jede Tätigkeit geplant (MAN.3), nachvollziehbar und Evidenz.
2. **Alles versioniert:** Anforderungen, Architektur, Code, Tests, Prozesse, Prompts, Checklisten — alles liegt in Git. Es gibt keinen Projektzustand, der nur im „Kopf" (Kontext) eines Agenten existiert.
3. **Artefakt = Evidenz:** Es werden keine separaten „Assessment-Dokumente" gepflegt. Das Arbeitsartefakt selbst (Issue, MR, Report, Baseline-Manifest) ist die ASPICE-Evidenz.
4. **Vier-Augen-Prinzip zwischen Rollen:** Kein Information Item wird von der Rolle freigegeben, die es erstellt hat.
5. **Automatisierungspyramide — Skript vor KI vor Mensch:** Deterministische Aufgaben (Link-Checks, Matrix-Generierung, Statuslisten, Builds, Reports, Feedback-Zuordnung) erledigen Skripte in CI; KI-Rollen nur, wo Urteilsvermögen nötig ist; der Mensch nur für Verantwortung, Freigaben und fachliche Klärung. Der Orchestrator prüft je Aufgabe zuerst die Skript-Route (Rollen-Registry); jede Retro sucht aktiv nach „skriptifizierbaren" KI-Tätigkeiten und macht daraus Automatisierungs-Tickets. Der Automatisierungsgrad ist ein KPI.
6. **Mensch als knappe Ressource:** Der Mensch wird gezielt und gebündelt einbezogen (Gates, Decision-Inbox, Review-Fristen mit Default-Verhalten) — niemals beiläufig.
7. **Transparenz — live:** Jederzeit und in Echtzeit beantwortbar über das Frontend: Woran arbeitet das Team gerade (laufender Agent-Tick, Skript-Lauf, wartend)? Was ist der Status jedes Tickets und jeder Anforderung? Welche Entscheidungen stehen an? Was hat es gekostet? Alle Aufgaben leben ausnahmslos im Ticketsystem; Statusänderungen erscheinen ohne Verzögerung in der Live-Ansicht.
8. **Besetzungs-agnostische Rollen:** Jede Rolle ist über ihre Schnittstelle definiert (Auftrag, Inputs/Outputs, Ticket-Typen, Reviews) und kann durch Skript, KI-Agent oder Mensch besetzt sein — auch gemischt je Aufgaben-Typ. Menschliche Rollen-Inhaber (späteres Arbeitsteam aus Fachexperten) arbeiten nach denselben Regeln: gleiche Tickets, gleiche DoD, gleiche Reviews; sie erhalten ihre Aufgaben über die Frontend-Aufgabensicht.
9. **Lernende Rollen:** Jede KI-Rolle lernt kontinuierlich dazu — ausschließlich über ihre versionierte Wissensbasis und den kontrollierten Lernzyklus (Kap. 11), nie durch unkontrollierte Ad-hoc-Änderungen.
10. **Ortstransparenz:** Rollen und Aufgaben sind an kein Gerät gebunden. Der gesamte Zustand lebt im Hub (Git, Tickets, Backend) — Team-Nodes (Laptops, PCs) sind austauschbare Ausführungskapazität, die sich Aufgaben per Lease aus der Queue ziehen; das Smartphone ist HMI (PWA: Live-Board, Decision-Inbox, Reviews, Feedback). Fällt ein Gerät aus, läuft die Lease ab und die Aufgabe geht automatisch zurück in die Queue — Arbeit geht nie verloren, sie wartet höchstens.

## 2. Rollen im Detail

Jede Rolle existiert als Rollenkarte (`process/roles/<rolle>.md` = System-Prompt) plus zugeordnete Prozess-Skills (`process/skills/<prozess>/SKILL.md`). Hier die operative Kurzbeschreibung:

### PL — Projektleiter (MAN.3)
**Auftrag:** Projekt planen, steuern, überwachen; Sprint-Zyklus fahren; Prioritäten setzen; Risiken führen; berichten; Eskalationen qualifizieren.
**Erstellt/pflegt:** Projektplan (leichtgewichtig: Ziele, Meilensteine, Sprint-Backlog), Risikoliste, Sprint-Reports, Decision Requests (Qualifizierung).
**Trigger:** Scheduler-Tick, Events (CI rot, DR beantwortet, neues Ticket vom Menschen).
**Besonderheit:** Einzige Rolle, die anderen Rollen Aufgaben zuweist. Löst Rollen-Konflikte kriterienbasiert; sonst Eskalation an Mensch.

### RM — Requirements-Manager (SYS.1, SWE.1)
**Auftrag:** Kundenerwartung aufnehmen und mit dem Menschen präzisieren (strukturierte Rückfragen!); Stakeholder-Anforderungen und SW-Anforderungen verwalten: eindeutige IDs, Attribute (Status, Priorität, Quelle, Rationale, Verifikationskriterium), Analyse auf Machbarkeit/Widerspruchsfreiheit/Testbarkeit, Traceability-Links.
**Erstellt/pflegt:** `requirements/stakeholder/*.md`, `requirements/software/*.md`, Klärungsfragen-Tickets, Anforderungs-Statusreport.
**Review durch:** SYS/ARCH (Machbarkeit), TEST (Testbarkeit), QM (IIC-Checkliste).

### SYS — Systemingenieur (SYS.2, Stub)
**Auftrag (Stufe 1):** Aus Stakeholder-Anforderungen die System-Sicht ableiten und die Grenze System↔Software sauber ziehen; SW-Anforderungen mit RM konsistent halten. In späteren Stufen: SYS.3–SYS.5 voll.

### ARCH — Architekt (SWE.2)
**Auftrag:** SW-Architektur entwerfen und pflegen: Komponenten, Schnittstellen, dynamisches Verhalten, Ressourcen; Bewertung von Alternativen; Architecture Decision Records (ADRs) mit Begründung; Zuordnung Anforderungen → Komponenten (Traceability).
**Erstellt/pflegt:** `architecture/` (Beschreibung, Diagramme als Mermaid/PlantUML im Repo, ADRs).
**Review durch:** DEV (Umsetzbarkeit), TEST (Prüfbarkeit/Integrationsstrategie), QM.

### DEV — Entwickler (SWE.3)
**Auftrag:** Detailed Design und Implementierung der Units gemäß Architektur; Code-Dokumentation; Unit-nahe Absicherung im Rahmen der Konstruktion (statische Analyse, Linting laufen in CI).
**Erstellt/pflegt:** `src/`, `design/` (Detailed Design dort, wo es über selbsterklärenden Code hinausgeht), Merge Requests je Aufgabe.
**Review durch:** zweiter DEV-Kontext oder ARCH (Code-Review via MR-Pflicht).
**Regel:** DEV schreibt keine Verifikationsmaßnahmen für die eigene Implementierung auf Komponenten-/SW-Ebene — das ist TEST.

### TEST — Verifikationsingenieur (SWE.4, SWE.5, SWE.6)
**Auftrag:** Verifikationsstrategie je Ebene; Maßnahmen spezifizieren und automatisieren: Unit-Verifikation (SWE.4), Komponenten-/Integrationsverifikation (SWE.5), SW-Verifikation gegen die SW-Anforderungen (SWE.6); Ergebnisse bewerten und berichten; Regressionsauswahl.
**Erstellt/pflegt:** `verification/` (Strategie, Spezifikationen, Testcode), Verifikationsreports je Sprint/Baseline, Findings als SUP.9-Tickets.
**Unabhängigkeit:** Arbeitet gegen Anforderungen und Architektur, nicht gegen Implementierungsdetails; eigener Agent-Kontext getrennt von DEV.

### QM — Qualitätsmanager (SUP.1)
**Auftrag:** Qualität von Information Items (gegen Checklisten/IIC) und Prozess-Compliance (Stichproben: „Wurde der Workflow wirklich gelebt?") unabhängig prüfen; Abweichungen als Findings mit Nachverfolgung; Freigabe-Mitzeichnung bei Baselines/Releases; **ungefilterter QM-Abschnitt im Sprint-Report an den Menschen**.
**Erstellt/pflegt:** `quality/` (QA-Plan light, Checklisten, Prüfprotokolle, Findings-Liste).
**Vetorecht:** Kann Baseline/Release blockieren; Überstimmen nur durch den Menschen (dokumentiert im Decision Log).

### CM — Konfigurationsmanager (SUP.8) + Plattform-Betrieb
**Auftrag:** CM-Strategie (Repos, Branching, Namenskonventionen, Storage-Locations, Tool-Liste, Backup); Baselines erstellen und verwalten (Tag + Baseline-Manifest); Zugriffsrechte; Infrastruktur (GitLab, CI-Runner, Backend/Frontend-Betrieb, Monitoring, Updates).
**Erstellt/pflegt:** `process/cm/` (CM-Strategie, Tool- und Storage-Übersicht), Baseline-Manifeste, Infra-as-Code.

### PROB — Problemmanager (SUP.9)
**Auftrag:** Alle Probleme (Bugs, CI-Ausfälle, Prozess-Pannen) als Tickets führen: Erfassung mit Reproduktion, Klassifikation (Schwere/Priorität), Ursachenanalyse koordinieren, Lösung als Aufgabe/CR auslösen, Abschluss verifizieren (durch TEST), Trends auswerten (Input für Retro).
**Regel:** Kritische Probleme → sofortige Meldung an PL, ggf. Decision Request an Mensch.

### CHG — Change-Manager (SUP.10)
**Auftrag:** Change Requests aufnehmen (aus Mensch-Wünschen, Problemen, Retros); Impact-Analyse orchestrieren (RM: Anforderungen, ARCH: Architektur, TEST: Verifikation, PL: Termin/Budget); Entscheidungsvorlage; nach Genehmigung Umsetzung als Tickets verfolgen; Status berichten.
**Genehmigung:** Kleine CRs innerhalb genehmigten Scopes → PL; scope-/budget-/baseline-relevante CRs → Mensch (Gate).

### REL — Release-Manager (SPL.2) *(Stufe 1: vom CM wahrgenommen)*
**Auftrag:** Release-Inhalt zusammenstellen, Freigabevoraussetzungen prüfen (Verifikation vollständig und bewertet, offene Probleme klassifiziert und akzeptiert, QM-Mitzeichnung, Baseline vorhanden), Release Notes, Auslieferungspaket, Mensch-Gate G3 einholen.

### COACH — Prozess-Coach (PIM.3, MAN.6, Moderation)
**Auftrag:** Prozessbeschreibungen/Skills/Templates pflegen; Retrospektive vorbereiten und moderieren (datenbasiert); Improvement-Backlog führen; KPIs erheben und im Frontend bereitstellen; Prozess-CRs formulieren; Wissensbasis/Lessons Learned über Projekte hinweg pflegen.

### Mensch — Auftraggeber & Eskalationsinstanz
Präzisiert Anforderungen (Dialog mit RM), entscheidet an Gates G0–G4, beantwortet Decision Requests, nimmt Ergebnisse ab. Antwortfristen und Default-Verhalten siehe Kap. 7.

## 3. Artefakt-Landkarte und Storage-Locations

| Information Item | Ort (Single Source of Truth) | Eigentümer | Reviewer |
|---|---|---|---|
| Projektauftrag, Projektplan, Risikoliste | `<projekt>/management/` | PL | QM, Mensch (G0) |
| Stakeholder-Anforderungen | `<projekt>/requirements/stakeholder/` | RM | SYS, Mensch (G1) |
| SW-Anforderungen | `<projekt>/requirements/software/` | RM | ARCH, TEST, QM |
| SW-Architektur, ADRs | `<projekt>/architecture/` | ARCH | DEV, TEST, QM, Mensch (G2 bei Grundsatz) |
| Detailed Design | `<projekt>/design/` + Code-Doku | DEV | ARCH |
| Quellcode | `<projekt>/src/` | DEV | DEV2/ARCH (MR) |
| Verifikationsstrategie/-spezifikationen/-code | `<projekt>/verification/` | TEST | ARCH, QM |
| Verifikationsreports | `<projekt>/verification/reports/` (CI-generiert) | TEST | QM |
| Probleme, CRs, Aufgaben, Decisions | GitLab Issues (typisierte Templates + Labels) | PROB/CHG/PL | — |
| Traceability-Matrix | CI-generiert aus Links, publiziert ins Frontend | RM (Datenqualität) | QM |
| Baselines | Git-Tags + `baselines/<id>-manifest.md` | CM | QM (+ Mensch bei G1/G3) |
| Prozesse, Rollen, Skills, Templates, Checklisten | Repo `process/` | COACH | betroffene Rollen, QM |
| Produktkatalog (Produkte, Versionen, Fähigkeiten, Schnittstellen) | `catalog/` (Einträge CI-generiert bei Release) | REL/CM | QM |
| Feedback-Tickets und Produkt-KPIs | GitLab Issues (Typ Feedback) + Backend-DB | PROB (Routing-Datenqualität) | — |
| Decision Log | `<projekt>/management/decisions/` | PL | — (append-only) |
| KPIs/Metriken | Backend-DB, Sicht im Frontend | COACH | — |

## 4. Sprint-Zyklus und Zeremonien

**Kadenz:** 1 Woche (Start-Empfehlung; per Retro änderbar). Agent-Arbeit erfolgt in Ticks (mehrere pro Tag, scheduler- und eventgetrieben); der Sprint gibt den Planungs- und Review-Rhythmus mit dem Menschen vor.

**Sprint Planning (Sprint-Tag 1, autonom):** PL zieht priorisierte Items aus dem Backlog (Kapazität = Kosten-Budget des Sprints), bricht sie mit den Rollen in Tickets herunter (jede Rolle schätzt/verfeinert ihren Anteil), löst erkennbare Klärungsfragen als gebündelten Decision Request aus. Ergebnis: Sprint-Backlog + Sprint-Ziel im Frontend.

**Sync-Tick (mehrmals täglich, autonom):** Jeder aktive Agent aktualisiert Ticket-Status; PL erkennt Blocker, verteilt neu, aggregiert Ampelstatus. Kein „Meeting", nur Board-Hygiene und Steuerung.

**Sprint Review (letzter Sprint-Tag, mit Mensch, asynchron möglich):** PL erstellt Review-Report: erreichte Ziele, Demo-Artefakte (Links auf lauffähige Ergebnisse, Reports), Anforderungs-/Verifikationsstatus, Kosten, Risiken, QM-Abschnitt (ungefiltert), anstehende Entscheidungen. Mensch hat eine definierte Einspruchsfrist (Vorschlag: 48 h), danach gilt der Sprint als abgenommen (G4) — Einsprüche werden CRs oder Probleme.

**Retrospektive (nach Review, autonom, Ergebnis transparent):** COACH moderiert datenbasiert: KPIs des Sprints, Problem-/Finding-Trends, Kosten je Ergebnis, „was hat Ticks verschwendet?". Jede Rolle liefert strukturiert Verbesserungsvorschläge. Ergebnis: max. 3 priorisierte Verbesserungen als Prozess-CRs (SUP.10) mit messbarem Erwartungswert — umgesetzt im nächsten Sprint. So verbessert sich Workflow, Prozess, IT und SW kontinuierlich und *kontrolliert*.

## 5. Ticket-Workflow und Status-Modell

Typisierte Issue-Templates: **Task** (Arbeitsauftrag, Label nach Prozessgebiet), **Problem** (SUP.9: Schwere, Repro, Ursache, Lösung, Verifikation), **Change Request** (SUP.10: Anlass, Impact-Analyse, Entscheidung, Umsetzung), **Decision Request** (HITL: Kontext, Optionen, Empfehlung, Frist, Default), **Clarification** (RM-Rückfrage an Mensch), **Finding** (QM/SUP.1), **Feedback** (Produktnutzung: Produkt+Version, Anlass, Typ Bug/Wunsch/Hinweis/Bewertung — wird per Skript zu Problem/CR/Backlog-Item geroutet, Kap. 12).

Status-Workflow (einheitlich): `open → in_analysis → in_progress → in_review → done` (+ `blocked`, `rejected`). Regeln: `in_review` erfordert benannten Reviewer ≠ Autor; `done` bei Problem/CR nur nach Verifikation; `blocked` erfordert Verweis auf Blocker (Ticket oder DR).

**Sichtbarkeitsregel (pm/N-0011, 2026-08-16):** `in_progress` wird **beim Beginn der Arbeit gesetzt, nicht erst beim Abschluss nachgezogen** — die Spalte im Board ist der Live-Blick des Menschen auf „woran wird gerade gearbeitet". Sessions, die ein Ticket in einem Zug erledigen, durchlaufen die Stufen trotzdem einzeln, damit ein Blick ins HMI während der Session etwas zeigt.

**Requirements-first (ab Sprint 3, T-0025):** Jedes neue Ticket, das Plattform- oder Produkt-Code erzeugt oder ändert, referenziert im Ticket-Body mindestens eine SWR-ID (Feld „SWR-Bezug") oder den auslösenden CR. Implementierung ohne Anforderungsbezug ist ein QM-Finding (SUP.1). Reine Prozess-/Doku-Tickets sind ausgenommen. Fehlt die passende Anforderung, entsteht sie zuerst (SWE.1, RM) — nicht der Code.

## 6. Definition of Done und QM-Checklisten

Je Information-Item-Typ existiert eine DoD-Checkliste in `process/checklists/` (abgeleitet aus den Information Item Characteristics des PAM 4.0). Beispiele:

- **SW-Anforderung done:** eindeutige ID; eindeutig, atomar, testbar formuliert; Attribute vollständig; Link zur Stakeholder-Anforderung; Verifikationskriterium benannt; Reviews (ARCH: machbar, TEST: prüfbar) bestanden; QM-Check ok.
- **Code/Unit done:** MR gemergt nach Review; CI grün (Build, Lint, statische Analyse, Unit-Verifikation); Coverage-Schwelle erfüllt; Traceability-Kommentar/Referenz auf Anforderung/Design; Doku aktualisiert.
- **Sprint done:** alle Sprint-Tickets `done` oder begründet zurückgegeben; Reports erstellt; KPIs erfasst; Retro durchgeführt; Review an Mensch versendet.

CI erzwingt die maschinenprüfbaren Teile (Links, Status, Coverage, Formate); QM prüft die urteilsbedürftigen Teile.

## 7. Human-in-the-Loop im Betrieb

**Bündelung:** Klärungsfragen und Entscheidungen werden — außer bei Kritikalität — gesammelt und dem Menschen gebündelt vorgelegt (Decision-Inbox, Benachrichtigung), nicht einzeln getröpfelt.

**Qualifizierte Vorlage:** Jeder DR enthält: Sachverhalt in 5 Sätzen, Optionen mit Konsequenzen (Aufwand, Risiko, ASPICE-Wirkung), Team-Empfehlung mit Begründung, Antwortfrist, Default bei Fristablauf (nur bei risikoarmen DRs zulässig; kritische DRs haben keinen Default und blockieren den betroffenen Strang).

**Eskalationsmatrix (initial):**

| Situation | Entscheider |
|---|---|
| Priorisierung innerhalb genehmigten Sprint-Scopes | PL |
| Technischer Rollen-Konflikt mit objektivierbaren Kriterien | PL (dokumentiert) |
| Anforderungswiderspruch, fachliche Unklarheit | Mensch (via RM-Clarification) |
| Scope-, Budget-, Termin-relevante Änderung | Mensch (via CR) |
| Architektur-Grundsatz, Technologie-Festlegung | Mensch (G2) |
| Baseline Anforderungen / Release | Mensch (G1/G3) |
| QM-Veto überstimmen | nur Mensch |
| Kritisches Problem (Datenverlust, Sicherheitsverdacht, Kosten-Anomalie) | sofortige Meldung an Mensch, Arbeit am Strang stoppt |

**Decision Log:** Jede Entscheidung (Mensch und relevante Team-Entscheidungen) wird append-only protokolliert: Datum, Entscheider, Optionen, Wahl, Begründung, betroffene Artefakte.

## 8. Kontinuierliche Verbesserung und KPIs

Start-KPI-Set (COACH erhebt automatisch, Frontend zeigt Trends):

1. Durchlaufzeit Ticket (nach Typ) und Anteil `blocked`-Zeit
2. First-Pass-Yield von Reviews (Anteil ohne Major-Findings)
3. Fehlerdichte (Probleme je Umfang) und Wiederöffnungsquote
4. Verifikationsabdeckung (Anforderungen mit bestandener Verifikation / alle)
5. Traceability-Vollständigkeit (CI-gemessen)
6. Kosten je Sprint und je abgeschlossenem Backlog-Item (Token + Infrastruktur)
7. HITL-Last: Anzahl DRs/Clarifications je Sprint, Antwortlatenz Mensch
8. Retro-Wirksamkeit: umgesetzte Verbesserungen und deren gemessener Effekt

Verbesserungen ändern ausschließlich versionierte Artefakte (Skills, Templates, Checklisten, CI-Regeln, Prompts) über Prozess-CRs — nie ad hoc. Wirkung wird im Folgesprint gegen den Erwartungswert geprüft (MAN.6/PIM.3 light).

**LeLe-Takt (pm/D005, 2026-08-16):** Lessons Learned sind Pflichtteil JEDES Sprint-Abschlusses und jedes Takt-Durchlaufs — nicht quartalsweise. Jede Lehre wird noch in derselben Session verankert (Runbook-Regel, Gold-Beispiel oder Prozess-CR); das PM-Takt-Ticket prüft die Einhaltung.

## 9. Baselines und Releases

**Baseline-Anlässe:** Anforderungs-Baseline vor Architekturstart und je Release (G1); Produkt-Baseline je Release (G3); Prozess-Baseline je P0-Meilenstein und danach quartalsweise. **Inhalt:** Git-Tag(s) + Manifest (Item-Versionen, Prüfstatus, offene Punkte). **Regel:** Nur aus Baselines wird released; Änderungen an einer Baseline nur über CR.

## 10. Problemlösungs-Workflow (SUP.9, Kurzform)

Erfassung (jeder Agent oder Mensch kann melden; CI meldet automatisch) → PROB klassifiziert (Schwere × Dringlichkeit; kritisch → Sofortmeldung) → Ursachenanalyse (PROB koordiniert, betroffene Rolle analysiert) → Lösungsweg: direkter Fix (Task) oder CR (wenn Baseline/Scope betroffen) → Umsetzung → Verifikation durch TEST (nicht durch den Fixenden) → Abschluss + ggf. Lessons-Learned-Notiz für Retro.

## 11. Lernzyklus der KI-Rollen

Ziel: Jede KI-Rolle (ASPICE-Team wie Arbeitsteam) wird messbar besser in ihrer Aufgabe — kontrolliert, nachvollziehbar, umkehrbar.

**Wissensbasis:** Je Rolle existiert `process/knowledge/<rolle>/` mit kuratierten Lessons Learned, Gold-Beispielen (Referenz-Ein-/Ausgaben), bekannten Fallstricken und Heuristiken. Sie wird dem Agenten bei jedem Einsatz zusätzlich zur Rollenkarte geladen und unterliegt einem Größenbudget (Kuratieren heißt auch Löschen).

**Zyklus (je Sprint):**

1. *Erfassen (Skript):* Review-/QM-Findings, Probleme, Nacharbeit und KPI-Ausreißer werden automatisch der verursachenden Rolle und dem Aufgaben-Typ zugeordnet.
2. *Destillieren (COACH):* Aus den Daten entstehen Update-Kandidaten: neue Regel, besseres Gold-Beispiel, Prompt-/Checklisten-Schärfung — oder ein Skriptifizierungs-Ticket, wenn die Tätigkeit regelhaft geworden ist.
3. *Prüfen:* Vor dem Merge bearbeitet die Rolle ihre Gold-Beispiele mit der neuen Wissensbasis (Regressionstest); betroffene Rolle und QM reviewen den Prozess-CR (SUP.10).
4. *Messen:* Im Folgesprint wird die Wirkung an den Rollen-KPIs geprüft (v.a. First-Pass-Yield, Nacharbeitsquote); bei Verschlechterung Revert per Git.

**Rollen-KPIs für Lernen und Automatisierung:** First-Pass-Yield je Rolle, Nacharbeitsquote je Rolle, Automatisierungsgrad (Anteil Aufgaben via Skript-Route), Kosten je erledigtem Aufgaben-Typ — Trends sichtbar im Frontend („Lernkurve je Rolle").

**Für menschliche Rollen-Inhaber** gilt der Zyklus sinngemäß: ihre Lessons Learned fließen in dieselbe Wissensbasis der Rolle ein — so profitieren KI- und Mensch-Besetzung derselben Rolle voneinander.

## 12. Produktnutzung und Feedbackschleifen (Arbeitsteam ↔ ASPICE-Team)

**Produkte nutzen:** Released Produkte des ASPICE-Teams stehen im Produktkatalog (`catalog/`) und sind — wo möglich — als Agent-Tools (CLI/API/MCP) verpackt. Eine Arbeitsteam-Rolle (KI oder Mensch), die eine Aufgabe erhält, prüft zuerst den Katalog: Welche vorhandenen Produkte lösen die Aufgabe ganz oder in Kombination? Das verwendete Produkt-Set inkl. Version wird am Ticket protokolliert (Nachvollziehbarkeit + Nutzungs-KPI). Erst wenn kein Produkt passt, entsteht neuer Lösungsaufwand — ggf. als Produktauftrag an das ASPICE-Team.

**Feedback geben (Pflichtkultur, geringe Hürde):** Jede Nutzung kann Feedback erzeugen — automatisch (Fehler-Reports, Nutzungsstatistik per Skript) oder explizit als Feedback-Ticket in unter einer Minute. Regel: Wer ein Produkt-Problem erlebt und kein Ticket erzeugt, hat den Prozess verletzt — nicht, wer „zu viel" meldet.

**Routing und Verarbeitung (ständig, nicht sprint-gebunden in der Erfassung):** Ein Skript ordnet jedes Feedback dem Produkt und dessen Entwicklungsprojekt zu und typisiert es: Bug → SUP.9-Problem (kritische sofort an PL), Änderungswunsch → SUP.10-CR, Hinweis/Bewertung → Produkt-Backlog. Das ASPICE-Team priorisiert im normalen Sprint-Rhythmus; Nutzungshäufigkeit und Schwere sind Priorisierungs-Input. Lösungen erreichen den Katalog als neues Release; das Arbeitsteam wird benachrichtigt.

**Schleife schließen:** Der Melder sieht den Status seines Feedbacks live im Frontend und erhält bei Lösung eine Nachricht. Nutzungserfahrungen („Produkt X eignet sich gut/schlecht für Aufgabentyp Y") fließen als Lessons Learned in die Wissensbasen der nutzenden Rollen (Kap. 11) — die Produktauswahl wird dadurch mit jeder Aufgabe besser.

**Produkt-KPIs (COACH/Backend):** Nutzungen je Produkt und Aufgabentyp, Problemrate, Time-to-Fix, Bewertung. Retro-Regel: Produkte ohne Nutzung oder mit hoher Problemrate kommen auf die Agenda — weiterentwickeln, überarbeiten oder aussortieren.

## 13. Verteilter Betrieb (Team-Nodes)

**Geräteregister (SUP.8, CM):** Jedes teilnehmende Gerät (Laptop, PC) steht im Geräteregister: Identität/Token, Fähigkeiten (OS, Toolchains, GPU, angeschlossene Hardware), erlaubte Rollen, Verfügbarkeitsfenster, Rechteumfang. Aufnahme neuer Geräte nur per Onboarding-Workflow mit Freigabe durch den Menschen; Tokens sind einzeln widerrufbar.

**Aufgabenvergabe:** Der Orchestrator plant fähigkeitsbezogen; die Hub-Queue vergibt Aufgaben im Pull-Prinzip an den nächsten passenden freien Node — mit Zeit-Lease und Heartbeat. Ergebnisse werden nur atomar bei Abschluss übernommen; abgelaufene Leases requeuen die Aufgabe automatisch. Der Ausführungsort (Gerät) wird am Ticket protokolliert und ist im Live-Board sichtbar („läuft auf PC-Werkstatt").

**Regeln:** Kein Projektzustand auf Geräten (lokale Arbeitskopien sind Wegwerf-Material); Nodes kommunizieren nur ausgehend zum Hub (HTTPS/WebSocket, optional VPN); rechenintensive Arbeit (Builds, Testläufe, Simulationen) gehört auf fähige Nodes, nicht auf den Hub; Smartphone-Nutzung ist auf HMI-Funktionen beschränkt (Entscheidungen, Reviews, Feedback, Statuseinsicht — auch das Beantworten eines Gates von unterwegs ist ein vollwertiger Beitrag). KPI: Wartezeit von Aufgaben auf passende Nodes; Engpässe sind Retro-Thema.

## 14. LLM-Provider-Einsatz (Claude, Copilot CLI, Ollama)

Drei LLM-Arten hinter einem Gateway (Masterplan Kap. 5.8); für die tägliche Arbeit gelten diese Regeln:

**Erweiterte Automatisierungspyramide:** Skript → **Ollama** (lokal, kostenlos) → **Copilot CLI** (Abo-Flatrate, Coding auf Team-Nodes) → **Claude** (API-Budget, höchste Qualität) → Mensch. Jede Aufgabe läuft auf der günstigsten Stufe, die ihre Fähigkeitsklasse erfüllt; die Rollen-Registry definiert je Aufgaben-Typ die Präferenzkette, der Orchestrator wendet sie an und protokolliert den gewählten Provider am Ticket.

**Feste Zuordnungen:** Gate-/Baseline-relevante Bewertungen (QM, Architektur-Entscheidungen, DR-Qualifizierung) laufen ausschließlich auf Claude. DEV-Routineaufgaben mit klarer Spezifikation bevorzugt auf Copilot CLI. Vorklassifikation, Zusammenfassungen, Entwürfe und als vertraulich markierte Inhalte bevorzugt auf Ollama.

**Provider-unabhängige Qualität:** DoD, Reviews und QM-Checks sind für alle Provider identisch — wer geliefert hat, ist für die Prüfschärfe irrelevant. Fällt ein Provider aus (offline, Budget, Abo), greift automatisch die nächste Stufe der Kette; nur wenn keine Stufe die Fähigkeitsklasse erfüllt, wartet die Aufgabe (blocked mit Grund).

**Messen statt glauben:** First-Pass-Yield, Nacharbeitsquote und Kosten je Provider und Aufgaben-Typ sind KPIs; Gold-Beispiele werden periodisch auf allen verfügbaren Providern verglichen. Routing-Ketten werden nur datenbasiert per Prozess-CR geändert.

## 15. Prozessprofile (Genesis 2.0, P5-E1)

Die Organisation besteht aus Teams dreier Arten (ASPICE-Team, PM-Team, Projekt-Teams — Registry: `process/teams/registry.yaml`). Nicht jedes Team braucht den vollen SWE-Prozess: **Jedes Team erhält bei Gründung genau ein Prozessprofil** (Registry-Feld `profil`), das Gates, Pflicht-Artefakte und DoD festlegt. Ein Profilwechsel ist ein Klasse-A-Entscheid.

**Profil `entwicklung`** *(gelebt seit P0; ASPICE-Team und SW-Produkte)*: volle SWE.1–6 + SUP.1/8/9/10, Gates G0–G4 als Inbox-DRs, Sprints mit Planning/Report/Retro, requirements-first mit SWR↔Test-Matrix (0 Lücken), Baselines als Tags + Manifest, Aufwandsschätzung je Planning.

**Profil `dienstleistung`** *(einmalige/projektartige Lieferungen: wissenschaftliche Analyse, Steuererklärung eines Jahres)*: MAN.3 + SUP.1/8/9 light. Gates nur **G0** (Auftrag mit messbaren Abnahmekriterien) und **G4** (Abnahme je Lieferung) — beide Klasse A. Pflicht-Artefakte: Auftrag, Tickets mit DoD je Aufgabentyp, Decision-Log, Liefer-/Abnahmevermerk. Keine SWRs/Matrix; Qualität über DoD-Review (QM-Stichprobe). Probleme/CRs laufen über den normalen SUP.9/feedback_route-Weg.

**Profil `wiederkehrend`** *(Daueraufgaben: Mail-Zusammenfassung, Markt-Monitoring, PM-Betrieb)*: Kanban ohne Sprints — Tickets tragen `sprint: 0` dauerhaft, Steuerung über Status und Prioritäten. Wiederkehrende Aufgaben als Ticket-Vorlagen mit Takt (täglich/wöchentlich); Ausführung im **Session-Takt** (F14/D027): die nächste Session arbeitet fällige Takt-Tickets nach PM-Agenda ab. Statt G4 gilt ein **SLA je Aufgabentyp** (z. B. „Digest in jeder Session, in der er fällig ist"); SLA-Verstöße sind Cockpit-/Retro-Thema. Qualitäts-Stichproben durch PM (Klasse B), Eskalation an den Menschen nur bei Befund. Gründung und SLA-Änderungen: Klasse A.

## 16. Entscheidungsklassen und Organisations-Guardrails (Genesis 2.0, P5-E2)

**Klasse A — immer der Mensch** (via Inbox/Briefkasten, mit Frist + Default): alles mit Geld, Recht oder Außenwirkung; Team-Gründung, -Pausierung, -Archivierung und Profilwechsel; Projekt-Aufträge (G0) und -Abnahmen (G4) außerhalb des Profils `wiederkehrend`; Budget- und Zugangs-Freigaben (neue externe Dienste, Credentials); Änderungen an diesem Kapitel.

**Klasse B — das PM-Team allein**: Priorisierung und Session-Agenda, Staffing (Rollen/Skills innerhalb genehmigter Teams), Routine-Abnahmen im Profil `wiederkehrend` (SLA-Stichproben), Einplanung von CRs ohne Budgetwirkung. Jede Klasse-B-Entscheidung steht **append-only im PM-Decision-Log** (`pm/management/decisions/`) und ist über Mission Control einsehbar; der Mensch kann jederzeit einsprechen — Einspruch wird als neue Zeile verbucht, nie durch Umschreiben.

**Klasse C — die Teams selbst**: Arbeitsebene innerhalb von Auftrag, Profil und DoD (Lösungsweg, Reihenfolge, Werkzeugwahl aus dem Katalog).

Im Zweifel gilt die höhere Klasse. Eskalationsweg: C → B → A.

**Harte Organisations-Guardrails (F17/D027 — gleiche Verbindlichkeit wie „die Sandbox pusht nie"):**

1. **KI-Teams handeln nie selbst mit Außenwirkung.** Keine Order-Ausführung, keine Steuer-/Behörden-Abgabe, kein Geldtransfer, kein Mailversand an Dritte, kein Vertragsabschluss. KI-Teams analysieren, entwerfen und bereiten vor — die ausführende Handlung tut ausschließlich der Mensch. (Bestehende Ausnahme bleibt: DR-/Warn-Mails an die registrierten Team-Adressen nach SWR-033.)
2. **Sensible private Daten kommen nie in Repos mit GitHub-Remote.** Datenklassen: `intern` (heutige Repos — Prozess, Code, Tickets), `sensibel` (private Mails, Steuer-/Finanzunterlagen, Gesundheitsdaten → nur lokale Ablage oder lokales Repo ohne Remote; Team-Repos verweisen per Pfad, committen nie Inhalte), `geheim` (Credentials, PINs → nur Env/lokale Ablage, wie bisher). Die Einstufung je Team ist Teil der Gründung (Klasse A).

---

*Änderungshistorie: 0.1 initialer Entwurf; 0.2 +Automatisierungspyramide, besetzungs-agnostische Rollen, Lernzyklus, Live-Transparenz; 0.3 +Produktkatalog-Nutzung und Feedbackschleifen; 0.4 +Ortstransparenz und verteilter Betrieb; 0.5 +LLM-Provider-Einsatz (Claude, 2026-08-05); 0.6 +Requirements-first-Regel Kap. 5 (T-0025, CHG, 2026-08-06); 0.7 +Kap. 15 Prozessprofile und Kap. 16 Entscheidungsklassen/Organisations-Guardrails (P5-E1/E2 nach p0/D027, 2026-08-15).*

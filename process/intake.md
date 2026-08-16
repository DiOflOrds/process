# Intake-Workflow: Neues Projekt oder Team anlegen (v2, P5-E3)

*Vom Projektwunsch zum laufenden Projekt. Multi-Projekt ist gesetzt (F8/D023: parallel von Anfang an) — je Projekt ein eigenes Repo nach p0-Muster; Plattform-Ausbau für projektübergreifende Sichten steht im P1-Backlog.*

## Schritte

1. **Auftrag erfassen:** Projektwunsch als Kurzbeschreibung (Was, Warum, Zielprodukt-Typ, Vertraulichkeit) — Eingangsparameter aus den F-Antworten: Zielprodukte SW/Embedded (F6/D023), Nutzerkreis (F9), Vertraulichkeit (F10), Domäne (F11).
2. **Projekt-Hülle anlegen:** Repo `p<N>` nach Muster (README, `tickets/`, `management/{decisions,runs,sprint-0}/`, `backlog.md`, board-check-Workflow); GitHub-Repo legt der Mensch an, Skelett liefert das Team (Muster: `p1/`).
3. **Registrierung:** Projekt in `produkte.yaml`-Logik analog aufnehmen (Matrix/CI je Produkt-Repo), Rollen-Registry gilt teamweit; Guardrails/Budget je Projekt prüfen (D003-Mechanik).
4. **G0:** Projektauftrag als erstes Dokument, Freigabe durch den Menschen → Decision Log D000 des neuen Projekts.
5. **Sprint-0-Planning** durch PL — ab hier greift das Playbook unverändert.

## Definition of Done (Intake)

Repo-Hülle valide (board-check grün auf leerem Board), Auftrag mit G0 freigegeben, Eintrag im Projektstatus, erster Sprint-Plan committet.

## Team-Gründung (Genesis 2.0, P5-E3 — Playbook Kap. 15/16)

Ein **Team** ist auf Dauer angelegt (ein Projekt endet, ein Team arbeitet weiter). Gründung ist **Klasse A**.

1. **Bedarf erfassen:** Wunsch des Auftraggebers (Briefkasten/Chat/Session) → PM formuliert den Team-Steckbrief: Auftrag, Profil (`entwicklung`/`dienstleistung`/`wiederkehrend`), Rollen, Datenklasse, benötigte Zugänge, Grenzen.
2. **Gründungs-DR (Klasse A):** Inbox-DR mit Charter-Entwurf (Template `process/templates/team-repo/docs/01-team-charter.md`), Frist + Default. Bei Datenklasse `sensibel` oder externen Zugängen: explizit im DR benennen (Guardrails Kap. 16).
3. **Repo aus Template:** `process/templates/team-repo/` kopieren → `team-<name>`, Platzhalter füllen, `git init`, board-check lokal grün. Discovery macht das Team damit automatisch in Mission Control sichtbar. **Bei Datenklasse `sensibel`: kein GitHub-Remote** — sonst legt der Mensch das GitHub-Repo an (+ Secret PLATFORM_READ_TOKEN, PAT-Erweiterung).
4. **Registrierung:** CM trägt das Team in `process/teams/registry.yaml` ein (Status `aktiv`), PM nimmt es in die Session-Agenda auf; ggf. platform-CI-Checkout ergänzen (nur bei GitHub-Remote).
5. **Betriebsstart:** erste Tickets nach Profil (bei `wiederkehrend`: Takt-Vorlagen + SLA im team.yaml); Lieferungen/Stichproben nach Playbook Kap. 15.

**Lebenszyklus danach:** Pausierung/Archivierung/Profilwechsel = Klasse A (Registry-Status; Repo bleibt als Historie). Staffing-Anpassungen im Betrieb = Klasse B (PM, geloggt).

**Definition of Done (Team-Gründung):** Klasse-A-Entscheid im Log, Registry-Eintrag, Repo valide (board-check), Charter final, Datenklasse + Zugänge dokumentiert, Team in der Session-Agenda.

## Projekte im Sammel-Repo (v3, pm/D003 2026-08-16)

Ab P10 werden neue Projekte als **Ordner im Repo `projects`** angelegt (`projects/p10/` usw., gleiche Struktur wie bisher) — kein eigenes GitHub-Repo, kein PAT-Update je Projekt mehr. Einmalige Voraussetzung (Mensch): GitHub-Repo `DiOflOrds/projects` + Secret + PAT-Erweiterung. Discovery/board/Baselines für verschachtelte Projektordner: SWR-070 (P9). Bestandsprojekte p0–p9 bleiben eigenständig.

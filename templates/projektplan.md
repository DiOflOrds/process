# Projektplan: <Pxx> „<Name>" (v0.1 — Entwurf, PL)

*Template `process/templates/projektplan.md` v1.1 (Projektmodell, Konzept 04 Kap. 4; v1.1: Leichtvarianten aus LeLe pm/T-0074, Prozess-CR pm/T-0076). Erstellt vom PL in Phase 1 des Projekt-Setups — VOR der Rollen-Initialisierung. Reviewer: QM (Vollständigkeit) + PM (projektübergreifende Passung). Der Plan ist leichtgewichtig und wird je Sprint fortgeschrieben, nicht neu geschrieben.*

**Drei Varianten — wähle ehrlich nach Projektart** (der Setup-Nachzieh hat gezeigt, dass Dauer- und Ruhezustand-Projekte sonst Aktivität erfinden müssten):

| Variante | Wann | Zuschnitt |
|---|---|---|
| **Voll** (Default) | neues Entwicklungs-/Dienstleistungsprojekt | alle 8 Kapitel wie unten |
| **Dauerauftrag** | Betrieb ohne Endtermin (Plattform, P13–P15) | Kap. 2 „Phasen" = Betriebszuschnitt statt Meilensteine; Kap. 6 „Timeline" = Takt/SLA statt Sprints; Rest gilt |
| **Ruhezustand** | Ziel erreicht, Repo lebt weiter (P9, P12) | Kap. 1 = erreichter Zustand + heutiger Restauftrag; Kap. 2/6 entfallen mit einem Satz Begründung; Risiken/Chronik-Pflicht bleiben |

## 1. Ziele

| # | Ziel (messbar formuliert) | Erfolgskriterium | Quelle |
|---|---|---|---|
| Z1 | <was am Ende wahr sein soll> | <woran man es misst> | <Auftrag/G0/Brief> |

## 2. Phasen und Meilensteine

| Phase | Inhalt | Meilenstein / Gate | Ziel-Sprint |
|---|---|---|---|
| 0 Setup | Planung + Rollen-Initialisierung (Kap. 4.2 des Konzepts) | Plan reviewt, Init-Tickets done | <n> |
| 1 … | <…> | <G1/G2/…> | <n> |

## 3. Aufgabenstruktur und Workflows

- **Aufgaben:** grobe Arbeitspakete je Phase (Details entstehen als Tickets; keine Arbeit ohne Ticket).
- **Workflows:** welche Abläufe gelten hier (Profil `entwicklung`: Requirements-first, Reviews, Gates · Profil `wiederkehrend`: Takt-Tickets, SLA)? Abweichungen vom Playbook explizit benennen (COACH dokumentiert sie in den Projekt-Workflows).

## 4. Team und Rollen

- **Core Team:** implizit (alle 10 Rollen, Konzept 04 Kap. 3.1). Abweichungen vom Default (Motor/Takt) hier benennen → PM pflegt sie in `besetzungen.yaml`.
- **Projektspezifische Rollen:** <Rolle, Auftrag in einem Satz — neue Baupläne sind Klasse A>

## 5. Infrastruktur

| Element | Festlegung |
|---|---|
| Repo/Ordner | <projects/pxx — Identität, wird nie umbenannt> |
| Datenklasse | <intern/sensibel — sensibel: kein Remote (.kein-remote)> |
| Werkzeuge | Plattform-Pflicht (board.py, tick, Mission Control); Zusatzbedarf = CR ans Plattform-Projekt |
| Externe Zugänge | <keine / welche — Freigabe je Dienst ist Klasse A> |

## 6. Timeline (Sprints)

| Sprint | Geplanter Inhalt |
|---|---|
| <n> | Setup: Planung + Initialisierung |
| <n+1> | <…> |

## 7. Risiken (initial — danach in management/risikoliste.md gepflegt)

| Risiko | Wirkung | W'keit | Maßnahme | Eigentümer |
|---|---|---|---|---|
| <…> | <…> | <…> | <…> | <Rolle> |

## 8. Berichtsweg und Monitoring

PL berichtet je Sprint an das PM-Team (Report + Cockpit-Status); der PL kennt alle Tickets des Projekts (auch rollen-eigene), überwacht Ampeln/Überfälligkeit im Sync-Tick, eskaliert per DR und ergänzt fehlende Aufgaben. Chronik: je Sprint eine Zeile in `docs/historie.md`.

# Gold-Beispiel PL: Sprint Planning (MAN.3)

**Zweck:** Referenz für die Umwandlung Backlog → priorisiertes Sprint-Backlog. Regressionstest vor Wissensbasis-Updates.

## Input

Backlog-Auszug: EP2 „Orchestrator-MVP" (hoch), EP4 „Rollenkarten v1" (hoch), EP7 „Frontend-Skizze" (niedrig). Budget-Kapazität: 10 € API-Kosten. Vorwissen: Gateway (EP3) ist Voraussetzung für Orchestrator-Ticks.

## Referenz-Output

Sprint-Ziel: „Erster autonomer Tick läuft end-to-end auf dem Team-Node."

| Ticket | Rolle | Prio | blocked_by | Begründung |
|---|---|---|---|---|
| Board-Tooling härten | cm | hoch | — | Skript-Route zuerst: alle weiteren Tickets laufen darüber |
| Rollenkarten v1 | coach | hoch | — | Voraussetzung für Rollen-Agenten |
| Gateway v1 | dev | hoch | — | Voraussetzung Tick |
| Orchestrator-MVP | dev | hoch | Gateway, Board-Tooling | technische Abhängigkeit |
| Frontend-Skizze | — | — | — | **nicht gezogen**: niedrige Prio, Budget für Kernpfad reserviert |

Klärungsbedarf gebündelt als 1 DR (API-Key + Budgetbestätigung), nicht als Einzelfragen.

## Warum Gold

Abhängigkeiten explizit als `blocked_by`; Skript-Route zuerst; bewusstes Nicht-Ziehen mit Begründung; Mensch-Interaktion gebündelt.

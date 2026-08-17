
---

## L-2026-08-17i — Fällt die Sperre, fällt der Verschiebungsgrund mit — und die Antwort ist Zerlegen

*Anlass: `projects/p11/T-0003`, Sprint 4 (2026-08-17).*

`p11/T-0003` stand auf Sprint 5 mit einem sauber dokumentierten Grund: drei Vertragsfelder
waren bis `platform/T-0006` nur über eine Behelfsregel bedient, und davor zu bauen hätte
geheißen, die SWR-096-Tests zweimal zu schreiben. In diesem Sprint ist `platform/T-0006`
geschlossen — **damit war der Grund weg**, und der Termin stand nur noch da, weil er einmal
dort hingeschrieben wurde.

**Regel 1 — ein Verschiebungsgrund hat eine Verfallszeit, und geprüft wird sie beim
Sprint-Planning.** Genau der Sprint, der die Sperre auflöst, muss den Termin des gesperrten
Tickets erneut anfassen. Sonst überlebt die Verschiebung ihren Anlass, und das Board zeigt
eine Planung, die niemand mehr begründen kann — B043 in Zeitlupe.

**Regel 2 — „zu groß" heißt zerlegen, nicht verschieben.** `pm/D006` nennt drei erlaubte
Gründe fürs Verschieben; „zu groß" ist keiner davon, sondern die Anweisung, Teiltickets zu
bilden und den **ersten** davon in diesen Sprint zu nehmen. Der Unterschied ist nicht formal:
ein zerlegtes Ticket liefert in jedem Sprint etwas, ein verschobenes liefert einen Termin.

**Regel 3 — der erste Teil ist der, den die anderen brauchen und der ohne sie Bestand hat.**
Hier der ADR. Ein Layout-Entwurf zuerst hätte raten müssen, ob er eine Server- oder eine
Clientfrage beantwortet.

**Regel 4 — die Zerlegung erbt die Frist, sie erfindet sie nicht neu.** `T-0003` behält den
20.08.; die Teiltickets bekommen frühere Termine, keine späteren. Eine Zerlegung, bei der die
Summe der Teile hinter dem Ganzen liegt, ist eine Verschiebung mit mehr Schritten.

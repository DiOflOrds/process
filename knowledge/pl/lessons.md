
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

## L-2026-08-17m (pm/T-0044, platform/T-0008): Eine richtige Beobachtung mit einer geschätzten Zahl erzählt

Sprint 6 hat **drei** Sätze widerlegt, alle aus derselben Quelle — und keiner war ein
Denkfehler, alle drei waren ungezählte Mengen neben einer richtigen Diagnose:

* *„Seit dem 17.08. ist kein einziger Push durchgekommen, rund zweihundert Mal"*
  (Sprint 5, an vier Stellen). Gezählt: **12 Läufe, 4 erfolgreiche Pushes, 9 Fehler**, und
  die Serie beginnt um 02:14. Die Ursache war richtig gefunden, die Reichweite geschätzt.
* *„Die Prüfung meldet die sieben Zeilen"* (`pm/T-0044`, eigene Verifikation). Sie meldet
  **sechs**; die siebte fällt unter eine dokumentierte Ausnahme. Die Zahl stammte aus der
  Handauszählung des Plannings und nicht aus dem Werkzeug.
* *„Was dabei auftaucht, braucht Urteil"* (`platform/T-0008`, Verschiebungsgrund). Gemessen:
  **0 Befunde**. Die Menge, über die geurteilt werden sollte, war leer.

**Regel 1 — eine Zahl in einer Begründung ist eine Behauptung und wird wie eine behandelt.**
Sie wird gezählt oder sie wird weggelassen. „Rund zweihundert" klingt wie eine Beobachtung
und war eine Schätzung; hätte jemand sie nachgezählt, wäre der Zeitpunkt des Bruchs (02:14)
schon in Sprint 5 sichtbar gewesen — und damit der Commit, der ihn verursacht hat.

**Regel 2 — die eigene Verifikation ist die Stelle, an der am ehesten eine ungeprüfte Zahl
steht.** Zweimal in diesem Lauf stand der falsche Satz in einem Abschnitt namens
„Verifikation". Wer eine Prüfung baut, lässt sie **gegen den Altstand** laufen und schreibt
ab, was herauskommt, statt aufzuschreiben, was herauskommen sollte.

**Regel 3 — ein Verschiebungsgrund ist prüfbar, bevor die Arbeit getan ist.** *„Die
Korrektur schaltet eine Prüfung ein, die nie lief"* klingt zwingend. Ob dabei etwas
auftaucht, ließ sich in fünf Minuten **messen**, ohne die Korrektur zu bauen. L-2026-08-17j
Regel 2 (die zweite Wiederholung eines Wartegrundes löst eine Prüfung der Quelle aus) gilt
damit ausdrücklich auch für **Verschiebungs**gründe, nicht nur für Wartegründe.

**Regel 4 — zwei Angaben zu derselben Frage brauchen eine Prüfung, und man muss alle finden.**
Sprint 1 hat die Driftgefahr bei `frist` neben `geplant_sprint` erkannt und geprüft — und
dabei die **dritte** Angabe übersehen: die Fälligkeitsspalte in `sprint-aktuell.md`. Sieben
Zeilen sind auseinandergelaufen. Wer eine Doppelaussage absichert, zählt vorher, wie viele
Stellen dieselbe Frage beantworten.

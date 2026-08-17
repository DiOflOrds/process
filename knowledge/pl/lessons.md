
---

## L-2026-08-17o — Eine Prüfung, deren Ergebnis vom Zeitpunkt abhängt, prüft den Zeitpunkt

*Anlass: `platform/T-0010` / `pm/T-0049`, Sprint 8 (2026-08-17).*

Sprint 7 hat `platform/T-0010` an **vier** Stellen als erledigt gemeldet — Planzeile,
Sprintabschluss, Session-Agenda und Statusbericht an den Auftraggeber — während das Ticket
auf `status: open` stand. Die Arbeit war fertig; nur das Feld wurde nie umgelegt. **Alle drei
Planprüfungen meldeten `[]`, und jede hatte recht:** `nicht_geplant` fragt nur, ob das Ticket
vorkommt; `plan_drift` vergleicht die Sprintnummer und überspringt Zeilen mit „dieser
Sprint"; `sprint_vergangen` kann nicht anschlagen, solange der fragliche Sprint der laufende
ist.

**Regel 1 — die Zeilenart, die eine Prüfung überspringt, ist zu benennen, nicht zu
übersehen.** `plan_drift` überspringt „dieser Sprint", weil dort keine Nummer steht. Genau
diese Zeilen schließt ein laufender Sprint. Wer eine Ausnahme baut, schreibt dazu, **welcher
Fall dadurch ungeprüft bleibt** — sonst liest sich die Ausnahme wie eine Abdeckung.

**Regel 2 — eine Prüfung, deren frühester Zeitpunkt nach der Meldung liegt, verhindert die
Falschmeldung nicht.** `sprint_vergangen` hat den Fall gefunden — im **Folgesprint**, nach dem
Bericht an den Auftraggeber. Das ist kein Fehler der Prüfung, sondern eine Aussage über ihren
Ort. Bei jeder neuen Prüfung ist zu fragen: *läuft sie vor oder nach dem Zeitpunkt, an dem der
Fehler Schaden anrichtet?* Deshalb steht SWR-115 in `preflight` und nicht in der Sprintsicht.

**Regel 3 — jede Spalte, in der eine Aussage steht, braucht einen Gegenhalter.** Die
Plantabelle hat vier Spalten. Drei wurden geprüft, die vierte — **die Statusspalte, in der
„erledigt" steht** — gegen nichts. Eine Aussage ohne Gegenprüfung ist eine Behauptung (B038),
auch wenn sie in einer Tabelle steht, die insgesamt geprüft aussieht.

**Regel 4 — „im Abschluss gemeldet" ist kein Beleg für „im Ticket verbucht".** Ab jetzt gilt
für den Sprintabschluss dieselbe Frage wie seit Sprint 7 für den Push: *ist das Gemeldete das
Verbuchte?* Der Unterschied zu Sprint 7 ist nur die Fläche — dort Arbeitskopie gegen HEAD,
hier Plantext gegen Ticketfeld. **Es ist dieselbe Familie**, und sie hat jetzt drei
Mitglieder: SWR-110 (Messung ≠ Lieferung), `pm/T-0048` (Prüfung hängt an der Reihenfolge),
SWR-115 (Prüfung hängt am Zeitpunkt).

**Regel 5 — ein Verschiebungsgrund, der auf ein anderes Ticket zeigt, verfällt mit dessen
Abschluss.** `pm/T-0038` verlangte wörtlich, „gebündelt mit `pm/T-0036`" ausgeliefert zu
werden. `pm/T-0036` war seit Sprint 7 geschlossen — und hatte die gemeinsame Fläche **nie**
angefasst. Der Grund galt zudem nur für **einen** von fünf Teilen und hielt die anderen vier
mit fest. L-2026-08-17i Regel 1 sagt, der auflösende Sprint muss den Termin anfassen; **Regel
5 ergänzt: er muss auch prüfen, ob der Grund je für das ganze Ticket galt.**

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

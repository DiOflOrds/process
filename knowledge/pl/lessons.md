
---

## L-2026-08-17s — Eine Messung vor der Änderung misst den Ausgangszustand

*Anlass: `platform/T-0011`, Sprint 10 (2026-08-17).*

Der Abschlussbericht von Sprint 9 meldete an **drei** Stellen „Plan-Drift 0, überfällig 0".
Beide Zahlen waren beim Berichten falsch (3 und 1) — und beide waren richtig, **als sie
gemessen wurden**. Zwischen Messung und Bericht hat derselbe Lauf die Plantabelle
umgeschrieben und dabei den Drift erzeugt, den die Zahl daneben bestritt.

**Regel 1 — eine Kennzahl im Abschlussbericht wird NACH der letzten Änderung des Laufs
erhoben, nicht davor.** Wer eine Zahl früh misst und spät berichtet, berichtet den
Ausgangszustand unter dem Namen des Ergebnisses.

**Regel 2 — die bessere Lösung ist nicht „besser aufpassen", sondern eine Stelle, an der
die Reihenfolge egal ist.** Das ist dieselbe Antwort wie bei SWR-118 in Sprint 9: die
Prüfung gehört dorthin, wo sie ohnehin läuft (Preflight), nicht dorthin, wo jemand an sie
denken muss.

**Regel 3 — eine berechnete Kennzahl, die niemand liest, ist keine Prüfung.**
`plan_drift` (seit Sprint 6) und `sprint_vergangen` (seit Sprint 7) wurden bei jedem
Aufruf berechnet, in den Payload gelegt und von keiner Meldung gelesen. Sie waren in
vier bzw. drei Sprints wirkungslos vorhanden. **Wer eine Prüfung baut, prüft im selben
Zug, wer ihr Ergebnis liest.**

**Regel 5 — die ID-Kollisionsprüfung gilt auch für Lessons, und die Reihe läuft über
ZWEI Dateien.** Dieser Lauf hat zunächst `L-2026-08-17r` vergeben — die Kennung war
bereits in `process/knowledge/cm/lessons.md` belegt. Die Buchstabenreihe a–r ist über
`pl/lessons.md` **und** `cm/lessons.md` hinweg fortlaufend; wer nur die eigene Datei
ansieht, findet die nächste freie Kennung nicht. Die Kollisionsregel vom 2026-08-16 war
für **Ticket**-IDs geschrieben; derselbe Fehler steht bei jeder Kennung, die an mehr als
einer Stelle vergeben wird. (Vor der Vergabe: `grep -oE '^## L-[0-9-]+[a-z]?'` über
**beide** Dateien, gegen **HEAD**.)

**Regel 4 — ein sauber ausgeführter Verzug ist unsichtbar.** `pm/T-0039` wurde viermal
korrekt um eins erhöht und fiel nicht auf; beim fünften Mal wurde nur die Plantabelle
geändert, und **genau diese Schlamperei** erzeugte den Drift, an dem alles auffiel. Wer
sich darauf verlässt, dass Fehler sich zeigen, findet nur die unordentlichen.

---

## L-2026-08-17t — Ein Test, dessen Fehlerfall repariert wird, prüft nicht mehr seinen Namen

*Anlass: `pm/T-0055` und `pm/T-0057`, Sprint 10 (2026-08-17) — zweimal im selben Lauf.*

**Fall 1: die Provokation.** Drei Tests aus Sprint 9 erzeugten ihren Fehlerfall, indem sie
eine `.git/index.lock` anlegten. SWR-123 räumt genau diese Sperre weg — der Ablauf gelang,
die Tests wurden rot, und weder an der alten noch an der neuen Anforderung war etwas
falsch. Hier wurden sie rot, weil die Erwartung „wirft einen Fehler" lautete. **Hätte sie
„meldet nichts" gelautet, wären sie still grün geworden — aus dem falschen Grund.**

**Fall 2: der Ort statt der Zusage.** Zwei Tests aus den Briefen `pm/N-0023`/`pm/N-0024`
prüften, ob ein langer Text **in der Pool-Datei** steht. Die Zusage dieser Briefe lautet
„wird angenommen und nicht gekürzt" — eine Aussage über den **Wortlaut**, nicht über die
Datei. SWR-124 lagert den Text aus; die Zusage gilt, ihre Umsetzung ist eine andere.

**Regel 1 — ein Test prüft die Zusage, nicht ihre gegenwärtige Umsetzung.** Steht im Test
ein Dateipfad, ein Speicherort oder ein Mechanismus, wo die Zusage von einem Ergebnis
spricht, ist er an die Bauform gebunden statt an die Anforderung.

**Regel 2 — wird ein Test durch eine richtige Änderung rot, ersetzt man die Provokation
oder die Prüfgröße, NIE die Erwartung.** Eine Erwartung anzupassen, weil das Verhalten
sich geändert hat, ist Arbeit am Test. Eine Erwartung anzupassen, weil sie stört, ist
Aufgabe der Verifikation.

**Regel 3 — ein Fehlerfall wird über etwas provoziert, das die Anforderung NICHT
verspricht zu reparieren.** Wer den Fehler über genau den Mechanismus erzeugt, den ein
späteres Ticket beseitigt, baut ein Ablaufdatum in seinen Test ein.

---

## L-2026-08-17u — Eine Zahl von Befunden zählt Artefakte, keine Entscheidungen

*Anlass: `pm/T-0053`, Sprint 10 (2026-08-17).*

„21-mal `open -> in_review`" las sich wie ein Arbeitsweg, den die Organisation tatsächlich
geht — so stand es in der Frage des Tickets. Nach **Datum** aufgeschlüsselt zerfielen die
21 in **drei Ereignisse**: 7 vor Existenz der Prüfung, 13 aus *einer* Sitzung binnen 56
Minuten, 1 Einzelfall neun Tage später. Kein Repo außer p0 und p1 kommt vor.

**Regel 1 — bevor eine Befundzahl als Praxis gelesen wird, wird sie nach Zeitpunkt und
Urheber gruppiert.** 13 Tickets aus einer Sitzung sind ein Ereignis, nicht dreizehn.

**Regel 2 — die Gruppierung, in der ein Befund erhoben wurde, ist selten die, in der man
ihn deuten darf.** Die 52 aus `pm/T-0048` waren nach **Übergangstyp** gruppiert; genau
diese Gruppierung erzeugte die Vermutung eines fehlenden Arbeitswegs.

**Regel 3 — Schwesterregel zu L-2026-08-17q.** Dort hieß „uns sind zwei aufgefallen" nicht
„es waren zwei"; hier heißt „21 Fälle" nicht „21-mal so gearbeitet". Beide Male sagt die
Zahl etwas über die **Erhebung** und nicht über den Sachverhalt.

---

## L-2026-08-17v — Wer zerlegt, zieht die Klammer nach

*Anlass: `projects/p11/T-0003`, Sprint 10 (2026-08-17).*

Das Ticket wurde als **überfällig** gemeldet („offen auf Sprint 8, laufend ist Sprint 10"),
obwohl es seit Sprint 9 nur noch eine Klammer über drei Teiltickets ist. Es trug die
Sprintnummer aus der Zeit **vor** der Zerlegung.

**Regel 1 — bei einer Zerlegung bekommt das Klammerticket den Termin des LETZTEN Teils.**
Sonst meldet es einen Verzug für Arbeit, die es selbst gar nicht mehr enthält — ein
Fehlalarm, der das Wegsehen trainiert.

**Regel 2 — die Klammer bleibt offen, aber sie behauptet nichts über den Zeitpunkt der
Teile.** Ihre Frist ist die des letzten Teils, nicht die kürzeste und nicht die alte.

---

## L-2026-08-17q — Eine Zahl in einem Ticket ist eine Behauptung, bis jemand sie erhoben hat

*Anlass: `pm/T-0048`, Sprint 9 (2026-08-17).*

Das Ticket sagte an drei Stellen „die **beiden** Altfälle" und baute einen ganzen Punkt
seiner Definition of Done darauf auf („Was passiert mit den beiden Altfällen?"). Der
erste Lauf der gebauten Prüfung fand **52** — verteilt auf acht Repos, davon 28 vom
selben Typ wie die zwei genannten.

**Regel 1 — die Zahlen in einem Ticket sind ungeprüfte Beobachtungen, keine Messungen.**
Was ein Ticket zählt, hat jemand beim Schreiben gesehen. Was es *gibt*, weiß erst die
Prüfung. Zwischen beidem kann ein Faktor 25 liegen, und der Unterschied ist nicht die
Genauigkeit, sondern die **Art der Aussage**: „ich habe zwei gesehen" und „es sind zwei"
sind verschiedene Sätze, und Tickets schreiben regelmäßig den zweiten, wenn sie den
ersten meinen.

**Regel 2 — die erste Ausgabe einer neuen Prüfung wird gegen die Erwartung gehalten, die
sie ausgelöst hat.** Nicht um zu bestätigen, sondern um den Unterschied zu sehen. Weicht
sie ab, ist die Abweichung der eigentliche Befund und gehört ins Ticket — noch bevor die
Prüfung eingebaut wird. In Sprint 9 hat genau dieser Vergleich aus einem
Werkzeug-Ticket einen Befund über die Organisation gemacht: die Fehlerart war nicht ein
Unfall aus Sprint 7, sondern der Normalfall seit dem ersten Sprint.

**Regel 3 — „nicht aufgefallen" ist keine Aussage über Häufigkeit, sondern über
Aufmerksamkeit.** Die zwei genannten Fälle fielen auf, weil in Sprint 7 gerade jemand
auf ihre Dateien sah (SWR-110 war frisch gebaut). Wer aus „uns sind zwei aufgefallen"
schließt „es waren zwei", verwechselt die Reichweite seines Blicks mit der Größe des
Bestands. **Dieselbe Verwechslung wie B049**, nur eine Ebene höher: dort war die Zahl je
Kachel lesbar und nie als Summe, hier war sie je Sprint sichtbar und nie über die
Historie.

**Regel 4 — auch Kostenschätzungen in Tickets sind solche Zahlen.** Dasselbe Ticket
verwarf einen Lösungsweg als „teuer" und stellte ihm den billigeren gegenüber; eine Zahl
stand nirgends. Gemessen: rund 10 s gegen einen Preflight, der ohnehin 60 s braucht. Die
Kostenfrage war real — ihre Antwort lag in einer Minute vor und trug die **entgegen**-
gesetzte Entscheidung.

---

## L-2026-08-17p — Ein Verschiebungsgrund, der die eigene Definition of Done zitiert, ist zirkulär

*Anlass: `pm/T-0047`, Sprint 9 (2026-08-17). Vierter Treffer von L-2026-08-17j Regel 2 in
vier aufeinanderfolgenden Sprints.*

`pm/T-0047` wurde zweimal verschoben mit dem Grund „Vertragsfrage vor Bau — die
Entscheidung gehört vor die Umsetzung". Punkt 1 der Definition of Done desselben Tickets
lautet: *„Entscheidung zu Punkt 1 und 2 im Ticket ausgeschrieben."*

**Das Ticket wurde also verschoben, weil seine erste Aufgabe noch nicht erledigt war.**

**Regel 1 — die Erkennungsfrage.** Steht der Verschiebungsgrund als Aufgabe in der
Definition of Done desselben Tickets? Dann ist er kein Grund, sondern eine Beschreibung
der Arbeit. Ein solcher Grund kann nie erfüllt werden, solange man ihm folgt: er
verschiebt genau die Arbeit, die ihn auflösen würde.

**Regel 2 — „eine Entscheidung ist nötig" ist erst dann ein Verschiebungsgrund, wenn
jemand anders sie treffen muss.** Der Test ist die Entscheidungsklasse (Playbook Kap. 16),
und er dauert eine Minute: Klasse A → Inbox-DR mit Frist und Default, und *dann* wartet
das Ticket zu Recht. Klasse B oder C → das Team entscheidet, und die Entscheidung ist
Arbeit dieses Sprints. **Gibt es keinen DR zu einer angeblich vorzulegenden Frage, wurde
sie nie vorgelegt** — sie wurde nur nicht beantwortet.

**Regel 3 — ein Nachbar, der auf dieses Ticket wartet, ist ein Argument dafür, es zu
bauen.** In Sprint 8 kam als zusätzlicher Grund „ein neuer Nachbar will an dieselbe
Stelle" (`pm/T-0051`) hinzu. Dessen eigene DoD verlangt wörtlich: *„`pm/T-0047` ist
geschlossen."* Die Abhängigkeit zeigte in die falsche Richtung. **Bei „X blockiert
dieses Ticket" wird in X nachgesehen**, nicht geglaubt.

**Regel 4 — ein B025-Grund verfällt mit dem Lauf, für den er galt.** „Diese Fläche wurde
in diesem Lauf schon angefasst" ist eine Aussage über **einen** Sprint. Unverändert in
den nächsten übernommen, wird sie falsch, ohne dass jemand etwas tut. `git log` auf die
genannte Datei beantwortet das in Sekunden — hier: zuletzt in Sprint 7 angefasst, der
Grund wurde durch zwei Sprints getragen.

**Regel 5 — eine falsch gestellte Frage kann ein Ticket jahrelang festhalten.** „Kopfblock
**oder** Feld?" unterstellte, ein Kopfblock ändere die Antwort für jeden Leser. Gemessen
liest jeder Leser `payload["projekte"]`; ein Schlüssel **daneben** ändert für keinen
etwas. Die Frage zerfiel bei der ersten Messung in eine harmlose und eine schwere Hälfte,
und nur die schwere hätte die Abstimmung gebraucht — die niemand gewählt hätte. **Vor dem
Messen des Grundes lohnt das Prüfen der Frage.**

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

---

## L-2026-08-17x — Umfang ist der Zerlegungsgrund, nicht der Verschiebungsgrund (Sprint 11)

**Gemessen an drei Tickets in einem Lauf.**

| Ticket | `geplant_sprint`-Kette | letzter Grund |
|---|---|---|
| `pm/T-0039` | 6→7→8→9→(10)→11 | Sprint 10 hat es **zerlegt** — daraus stammt die Regel |
| `pm/T-0028` | 7→8→9→10→11 | *„Rest = Umfang (3 Flächen)"* — **vierte Verschiebung** |
| `projects/p12/T-0003` | 8→9→10→11 | Sprint 10 notierte *„beim Anfassen zerlegen"* und schob |

**`pm/T-0028` stand in derselben Plantabelle desselben Laufs, der die Regel aus
`pm/T-0039` abgeleitet hat** — mit demselben Zählerstand — und wurde ein viertes Mal
verschoben. Nach `pm/D006` ist Umfang **der Zerlegungsgrund selbst**; „zu groß für diesen
Sprint" ist die Beschreibung eines Tickets, das zerlegt werden muss, nicht die Begründung,
es zu verschieben.

> **Eine Regel, die in einem Lauf aufgeschrieben und im selben Lauf am Nachbarfall nicht
> angewandt wird, ist noch keine Praxis.** Erkennungsfrage beim Aufschreiben: *Auf welche
> anderen offenen Fälle trifft dieser Satz gerade zu?*

**Regel 2 — die Klammer zählt nicht mit.** `projects/p11/T-0003` ist seit Sprint 9 nur noch
eine Klammer über drei Teiltickets. Ihr Feld wandert mit dem letzten Teil; das sind
**Nachführungen, keine Verschiebungen**. Zählt man sie mit, schlägt die Vier-Runden-Regel
bei einem Ticket an, das selbst nichts mehr enthält — und trainiert damit das Wegsehen bei
denen, die etwas enthalten.

**Regel 3 — ein wiederkehrender Verschiebungsgrund ist zu messen, nicht zu wiederholen.**
Fünf offene Aufgaben lagen auf der HMI-Fläche, und jeder Lauf verschob sie mit **B025**
(„nicht zwei Bauflächen in einem Lauf"). Gemessen: die Arbeit jedes Laufs entsteht aus den
Briefen des Auftraggebers, und die treffen zuerst das Backend. **Solange jeder Lauf Backend
baut, ist B025 für die HMI kein Grund, sondern ein Ausschluss.** Aufgelöst nicht durch einen
sechsten Satz, sondern durch einen Beschluss: Sprint 12 ist ein HMI-Sprint.
Erkennungsfrage: *Kann der Zustand, den mein Grund beschreibt, überhaupt eintreten, wenn
ich weiter so plane wie bisher?*

**Regel 4 — ein Ticket im selben Sprint wie sein Blocker behauptet, beides gehe in einem
Lauf.** `promt-team/T-0003` stand auf Sprint 12 wie seine beiden `blocked_by`-Tickets,
während es selbst sagt *„ohne Baseline kein Optimierungslauf"*. Stiller Widerspruch — keine
Prüfung hält `geplant_sprint` gegen den Sprint des Blockers. Notiert als Prüfungskandidat.

**Regel 5 — ein Briefkasten-Stand vom Laufbeginn ist am Laufende keine Aussage mehr.**
Sprint 11 startete mit „1 offener Brief" und endete mit drei: `promt-team/N-0001` und
`team-dashboard/N-0002` trafen **während** des Laufs ein und wurden erst vom
**Abschluss**-Preflight gemeldet. Wäre der Abschlusscheck übersprungen worden, hätte der
Lauf „kein offener Brief" berichtet und zwei unbeantwortete hinterlassen. Schwesterregel zu
`L-2026-08-17t` (eine Messung vor der Änderung misst den Ausgangszustand): **hier misst eine
Messung vor dem Ende nicht den Eingang.** Der Briefkasten gehört an **beide** Enden eines
Laufs, nicht nur an den Anfang.

---

## L-2026-08-17aa — Der Abschluss-Preflight gehört VOR die Berichte, nicht danach

**Gemessen, Sprint 12.** Der Lauf schrieb `sprint-aktuell.md`, `session-agenda.md` und
`PROJEKTSTATUS-UPDATE.md` fertig, ließ danach den Abschluss-Preflight laufen — und der fand
einen Brief (`pm/N-0042`, eingegangen 12:00), der in allen drei Berichten fehlte. Alle drei
mussten nachträglich ergänzt werden.

**Zweiter Sprint in Folge.** Sprint 11 traf es zweimal (`promt-team/N-0001`,
`team-dashboard/N-0002`) und leitete daraus die Regel ab, den Briefkasten **zweimal** zu
prüfen. Diese Regel hat gegriffen. Die **Reihenfolge** hat sie nicht mitgeregelt.

> **Bei 60-Minuten-Takt und Briefen zu beliebiger Zeit ist „Brief mitten im Lauf" kein
> Sonderfall, sondern der Regelfall.** Ein Lauf dauert eine Stunde; die Wahrscheinlichkeit,
> dass in dieser Stunde nichts eintrifft, ist keine Konstante, auf die man einen Ablauf
> bauen kann.

**Regel 1 — Reihenfolge des Abschlusses:** Tickets terminieren → **Abschluss-Preflight** →
*dann* Berichte schreiben → Verifikation. Nicht: Berichte → Preflight → Berichte flicken.
Ein Bericht, der nach seiner eigenen Prüfung entsteht, muss nicht korrigiert werden.

**Regel 2 — dieselbe Familie wie SWR-122.** Dort wurde eine Zahl **vor** der Änderung
gemessen, die sie beschreiben sollte („eine Messung vor der Änderung misst den Ausgangs-
zustand, nicht das Ergebnis"). Hier wird ein **Bericht** vor der Prüfung geschrieben, die
ihn vollständig machen soll. Gleiche Ursache: die Reihenfolge ist nicht festgelegt, also
gewinnt die bequeme.

**Erkennungsfrage am Sprintende:** *Ist zwischen meiner letzten Messung und dem Satz, den
ich gerade schreibe, noch etwas passiert — und wer hätte es mir gesagt?*

## L-2026-08-17ad — Eine Wiederöffnung ist ein Statuswechsel und braucht ihren eigenen Commit

*Anlass: Sprint 13 (2026-08-17), selbstverschuldeter Befund an `platform/T-0014`.*

Dieser Lauf hat `platform/T-0014` geschlossen, danach **zwei weitere Leser** des
Entscheidungsmarkers gefunden und das Ticket bewusst wiedereröffnet — richtig so, die
Wiederöffnungsquote ist genau dafür eine KPI. Falsch war die Buchführung: die Datei ging
`done → in_progress → in_review → done`, die **Commits** aber `done → in_review`. Der
Zwischenstand bekam keinen eigenen Commit, weil die Wiederöffnung sich wie Buchhaltung
anfühlte und nicht wie ein Zustandswechsel.

`uebergang_historie` hat es gemeldet — und beim zweiten Hinsehen waren es **zwei**:
`platform/T-0014` (`done → in_review`) und `pm/T-0064` (`open → in_review`). Der Befund
steht im Sprintabschluss und ist **nicht** geglättet worden — weder durch ein Verschieben
des Stichtags noch durch ein Umschreiben der Historie. Der Altbestand von 52 Fällen liegt
aus demselben Grund unangetastet da.

⚠⚠ **Der zweite Fall ist der eigentliche Befund an dieser Lesson.** Der erste wurde beim
Testlauf gefunden, diese Regel daraufhin geschrieben — und der zweite lag zu diesem
Zeitpunkt **schon in der Historie** und wurde erst vom Abschluss-Preflight gefunden. Eine
Regel aufzuschreiben ist nicht dasselbe wie den Bestand danach zu prüfen: *wer eine Lesson
formuliert, prüft im selben Zug, wie oft der Fall schon eingetreten ist.* Genau diese Frage
stellt `L-2026-08-17x` („auf welche anderen offenen Fälle trifft dieser Satz gerade zu?") —
hier ging es nicht um andere Fälle, sondern um **denselben Fall ein zweites Mal**.

**Regel — jeder Statuswert, der auf der Platte stand, braucht einen Commit.** Auch der
rückwärts. `board.setze_status` und `git commit` sind ein Paar; wer das erste ohne das
zweite aufruft, erzeugt einen Sprung, den die Historie später nicht erklären kann.

**⚠ Die eigentliche Lehre liegt eine Ebene höher.** Der Lauf, der die Prüfung gegen
*„Zustand nur in Prosa"* gebaut hat, hat im selben Ticket den *Zustand in der Historie*
verloren. Es ist die vierte Wiederholung desselben Musters an einem Tag: die Regel war
bekannt, die Prüfung existierte, und sie wurde am **Nachbarfall** nicht angewandt.

**Erkennungsfrage:** *Habe ich gerade ein Feld geändert, ohne zu committen — und wie sieht
die Änderung für den aus, der nur die Commits liest?*

## L-2026-08-17ap — Ein Lauf, der zwischen Registerzeile und erstem Commit stirbt, hinterlässt einen Sprint, der laufend aussieht und nichts hervorgebracht hat

**Anlass (Sprint 17, dieser Lauf).** Die Registerzeile für Sprint 17 stand um **16:49** in
`pm/management/sprints.jsonl` und war **nicht committet**. Seit dem Ende von Sprint 16
(17:10) hatte **kein** Repo einen Commit bekommen. Der Lauf, der die Zeile schrieb, ist
zwischen dem Schreiben und seinem ersten Commit abgebrochen.

⚠ Für `laufender()` war das ein **laufender Sprint** — richtig gelesen und trotzdem
irreführend: er hatte nichts hervorgebracht.

Die mechanische Anwendung von SWR-136 wäre gewesen: Schreibspur beobachten, einen Takt
warten, dann übernehmen (`ende` mit `abgebrochen: true`) und **Sprint 18** eröffnen. Das
hätte eine Sprintnummer für einen Lauf verbraucht, der nichts getan hat, und den Zähler der
Organisation gegen ihre eigene Arbeit verschoben.

Gewählt ist der **idempotente Wiedereintritt**, den `beginne()` seit SWR-136 ausdrücklich
vorsieht: dieselbe Kennung übergeben, nichts wird angehängt, die Nummer bleibt. Der
Docstring nennt genau diesen Fall — *„ein Lauf, der zweimal startet (Wiederholung nach
Fehler)"*.

**Regel:** Steht beim Start ein laufender Sprint im Register, wird **zuerst** gefragt, ob
er etwas hervorgebracht hat (Commits in irgendeinem Repo seit seinem Beginn). Hat er
**nichts** und ist seine Kennung die des eigenen Takts, ist der Wiedereintritt unter
derselben Kennung die richtige Antwort — keine Übernahme mit neuer Nummer. Die Tatsache,
dass ein Lauf abgebrochen ist, gehört dann in die **Sprintdatei**, nicht in eine zweite
Registerzeile.

⚠ Und der Nebenbefund, der ernster ist als der Fall: die Eröffnung eines Sprints ist erst
mit ihrem **Commit** haltbar. Bis dahin ist sie eine Datei auf einer Platte, die jeder
Absturz mitnimmt oder — schlimmer — stehen lässt.

## L-2026-08-17at — Wer den Kopf einer Datei „fortschreibt", muss den alten Kopf VERSCHIEBEN und nicht ersetzen

**Anlass (Sprint 17, dieser Lauf, selbst verursacht und selbst gefunden).**
`PROJEKTSTATUS-UPDATE.md` ist so gebaut: oben *„Das Wichtigste (Stand Sprint N)"*, darunter
die Marke *„Frühere Stände (**unverändert, nichts umgeschrieben**)"*, darunter alle älteren
Stände.

Dieser Lauf hat den Sprint-17-Block geschrieben, indem er alles **vor** der Marke ersetzte
und alles **danach** behielt. Das Ergebnis: der komplette **Sprint-16-Block war weg** — 92
Zeilen, gelöscht von einem Lauf, der im selben Atemzug schrieb, er habe nichts
umgeschrieben.

> **Die Datei verspricht in ihrer eigenen Überschrift, dass nichts umgeschrieben wird. Der
> Schreibvorgang, der sie fortschreibt, war die einzige Stelle, an der dieses Versprechen
> gebrochen werden konnte — und er hat es gebrochen.**

⚠ Gefunden hat es **kein** Test. `PROJEKTSTATUS-UPDATE.md` liegt im Wurzelverzeichnis und
damit in **keinem** Repo: kein `git diff`, keine Historie, kein Wiederherstellen. Gefunden
wurde es nur, weil beim Nachsehen wegen eines **anderen** Verdachts (hat der parallele Lauf
etwas geschrieben?) die Überschriftenliste zufällig aufgefallen ist. Wiederhergestellt
werden konnte der Block nur, weil derselbe Lauf ihn zu Beginn **gelesen** hatte.

**Regel:** Beim Fortschreiben eines „Neues oben"-Dokuments wird der bisherige Kopfblock
**unter die Marke verschoben** und dann der neue davorgesetzt — nie „alles vor der Marke
ersetzen". Wer es doch tut, prüft danach die **Zahl der Blöcke**: sie muss um genau eins
gewachsen sein.

⚠ **Zweiter Punkt, der schwerer wiegt als der Fehler:** eine Datei, die die Geschichte der
Organisation trägt, liegt **außerhalb jedes Repos**. Für sie gibt es keine Rücknahme, keinen
Diff und keinen Wächter. Das ist als Befund aufzunehmen und nicht als Pech zu verbuchen —
`PROJEKTSTATUS-UPDATE.md`, `PUSH-ANFORDERUNG.txt` und die `SESSION-BEFUND-*.md` teilen diese
Eigenschaft.

## L-2026-08-17be — „Erst messen, dann entscheiden" erledigt Optionen an ihrer Voraussetzung statt an ihrem Preis

**Anlass (Sprint 19, `p11/T-0014`).** Das Ticket fragte, ob `/api/dashboard` ohne Leser
bestehen bleibt, und verlangte in seinem eigenen Text: *„Erst dessen Bedarf feststellen, dann
entscheiden."* Der einzige benannte künftige Leser war `p11/T-0011`.

Nachdem `T-0011` in **demselben** Sprint gebaut war, stand fest: es liest `/api/widgets` und
braucht den Endpunkt **nicht**. Damit fielen **zwei von drei** Optionen sofort — Option A
hätte einen **falschen** Satz in einen Docstring geschrieben, Option C war ausdrücklich an
diesen Bedarf konditioniert.

> **Beide waren Aussagen über ein Ticket, das es noch nicht gab. Über ihren Preis musste
> niemand mehr reden.**

**Regel:** Wenn ein Entscheidungsticket eine Eingabe benennt, die noch nicht existiert, ist
die richtige Reihenfolge **nicht verhandelbar** — und die Wartezeit ist kein Verlust: sie
spart die Diskussion über Optionen, die die Messung ohnehin streicht.

⚠ Und die Gegenrichtung: ein Entscheidungsticket ist **mit der Entscheidung fertig**. Die
Ausführung bekommt ein eigenes Ticket mit eigener DoD, wenn sie eine abgenommene Anforderung
anfasst — sonst hängt ein Bau mit Teststrecke, Matrix und Anforderungstext als Haken unter
einer Entscheidung.

## L-2026-08-17bf — Eine aufgeschriebene Lesson schließt keine Lücke; sie ist binnen eines Sprints wiederholt worden

**Anlass (Sprint 19, Abschlussbericht).** Sprint 18 hat einen eigenen Abschnitt darüber
geschrieben, dass seine Testzahl **fortgeschrieben statt gemessen** war (1069 statt 1061).
Sprint 19 hat im ersten Entwurf seines Abschlusses **1077** genannt; gemessen waren **1079**.

> **Der Lauf, der die Warnung beim Schreiben vor Augen hatte, hat denselben Fehler an
> derselben Stelle gemacht. Eine Zahl aus „letzter Stand plus eigene Neuzugänge" liest sich
> wie eine Messung und ist eine Fortschreibung.**

⚠ Aufgefallen ist es **beim Nachzählen**, wieder nicht durch eine Prüfung — der
Abschlussbericht hat für seine eigenen Kennzahlen keine. Das ist **Frage 3 von
`platform/T-0020`**, und die Wiederholung binnen eines Sprints ist der Beleg, dass die
Aufschreibung allein nicht wirkt.

**Regel:** Kennzahlen im Abschlussbericht werden **erhoben, nicht fortgeschrieben** — die
Testzahl über die Sammlung (`TestLoader.discover`), nicht über den letzten Bericht. Und die
richtige Reaktion auf eine zum zweiten Mal aufgelaufene Lesson ist **kein dritter Merksatz**,
sondern eine Prüfung.

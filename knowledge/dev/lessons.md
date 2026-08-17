
## L-2026-08-17bb — Eine gespeicherte Auswahl altert gegen einen wachsenden Bestand

**Anlass (Sprint 19, `p11/T-0011`/SWR-151).** Die Dashboard-Konfiguration speichert, welche
Widgets der Mensch **nicht** sehen will — eine **Ausschlussliste**. Der naheliegende Weg wäre
die **Auswahlliste** gewesen („zeig diese drei"), und sie wäre falsch:

> **Eine gespeicherte Auswahl sagt „zeig diese" — und was danach dazukommt, fällt lautlos aus
> der Ansicht. Niemand vermisst ein Widget, von dem er nie erfahren hat, dass es existiert.**

Ein Team, das ein `widget.yaml` neu hinlegt, hätte für jeden, der einmal konfiguriert hat,
**nichts** getan.

**Regel:** Wo eine gespeicherte Vorliebe auf einen Bestand trifft, der wächst, wird das
**Abgewählte** gespeichert und nicht das Gewählte. Die Frage, an der man es erkennt: *Was
passiert mit einem Eintrag, den es beim Speichern noch nicht gab?*

## L-2026-08-17bc — Ein Einwand gegen Persistenz verbietet sie selten; meistens verlangt er eine Erklärung

**Anlass (Sprint 19, `p11/T-0011`, DoD 2).** SWR-133 hat für den Faltzustand der
Cockpit-Gruppen die Persistenz **abgelehnt**, mit einem richtigen Satz: *„Ein Zustand, der
einen Neustart überlebt, müsste beim Wiedersehen erklärt werden — sonst fehlt eine Gruppe und
niemand weiß, warum."* Die DoD von `T-0011` verlangte, den Unterschied zu **begründen und
nicht zu behaupten** — zwei benachbarte Ansichten, eine speichert und eine nicht.

Die Auflösung war **nicht** „hier ist es anders":

> **Falten ist ein Griff beim Lesen. Eine Auswahl ist eine Aussage. Die eine wieder
> aufzumachen kostet einen Klick; die andere jedes Mal neu zu treffen macht sie wertlos.**

Der Einwand blieb damit **gültig** und wurde zur **Auflage**: was ausgeblendet ist, steht im
Kopf der Ansicht — als Zahl, mit den **Namen** darunter und einem Weg zurück.

⚠ Die zweite Hälfte der Begründung ist eine **Messung**: eine Zusicherung hält fest, dass der
Faltzustand **weiterhin flüchtig** ist. Wäre er beiläufig persistent geworden, gäbe es den
Widerspruch nicht mehr — und die Begründung wäre gegenstandslos, **ohne dass jemand sie
zurückgenommen hat**.

**Regel:** Ein früherer Einwand gegen eine Bauform wird nicht „überholt", sondern gelesen:
verbietet er die Sache, oder verlangt er etwas dazu? Wer sich auf einen Unterschied beruft,
sichert die **andere** Seite des Unterschieds mit einer Prüfung ab.

## L-2026-08-17bg — Ein Schreibweg, der eine Datei voraussetzt, die ein ANDERER Weg anlegt, ist so lange richtig, bis ein Repo anders entsteht

**Anlass (Sprint 20, `platform/T-0022`/SWR-152).** Der Auftraggeber hat eine
**Klasse-A-Entscheidung** über die Inbox getroffen, und der Schreibweg starb mit
`[Errno 2] No such file or directory` — `promt-team` hatte kein
`management/decisions/decision-log.md`. `open(..., "a")` legt eine **Datei** an, aber kein
**Verzeichnis**; das Verzeichnis legt `pool.gruende` bei der Gründung an.

> **Solange jedes Repo durch den einen Weg entstanden ist, ist die Annahme unsichtbar
> richtig. Sie wird erst sichtbar an dem Repo, das anders entstand — und das war ausgerechnet
> das, in dem eine Klasse-A-Entscheidung fällig war.**

⚠⚠ **Kein bestehender Test hätte es finden können.** Jede Fixture legt ihre Verzeichnisse
selbst an — und damit auch das, das in Produktion fehlte. Das ist wörtlich die offene
Erkennungsfrage aus Sprint 16 (`L-2026-08-17ai`), zum ersten Mal an einem **echten Schaden**
belegt statt als Sorge formuliert.

**Regel:** Wer eine Datei anlegt, die ein anderer Weg voraussetzt, hat eine **Abhängigkeit
zwischen zwei Wegen** gebaut. Zwei Prüfungen gehören dazu:

1. Eine Zusicherung, die den Zustand **ohne** die Datei herstellt — ausdrücklich, nicht
   nebenbei, weil die Fixture ihn sonst wegräumt, bevor sie ihn misst.
2. Eine **Zählung im echten Bestand**, wie viele Repos das Pflichtartefakt nicht haben, mit
   den **Namen** und nicht nur der Zahl.

⚠ Und die Kehrseite der Reparatur gehört benannt: ein Weg, der den Mangel im Vorbeigehen
**heilt**, macht ihn unsichtbar. Ob das Anlegen in den Schreibweg gehört oder als Befund in
den Preflight, ist offen (Frage 3 in `platform/T-0022`) — die Entscheidung „erst mal heilen,
damit die Entscheidung nicht verloren geht" ist bewusst und nicht endgültig.

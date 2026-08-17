
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


---

## L-2026-08-17z — „Übersprungen" sieht aus wie „grün", solange beide dasselbe melden

**Gemessen, Sprint 12 (ADR-008, SWR-128).** Die Organisation hatte **741 Python-Tests und
null JS-Tests** bei 1.524 Zeilen `app.js` — und SWR-098/099/100 verlangen Nachweise an
JavaScript. Fünf Sprints lang meldete jeder Abschluss „Tests grün", und die Aussage war
wahr: alle Tests, **die es gab**, waren grün.

> **Eine Teststrecke, die es nicht gibt, meldet dasselbe wie eine, die grün ist:
> nichts.**

Das ist die dritte Gestalt derselben Familie in drei Sprints — SWR-122: eine Prüfung ohne
Leser; SWR-125: eine Regel ohne Prüfung; hier: eine **Fläche ohne Prüfung**, an der
niemand das Fehlen bemerkte, weil Fehlen und Erfolg gleich klingen.

**Regel 1 — eine Prüfung hat drei Zustände, nicht zwei.** `ok`, `rot`, `übersprungen`.
Wer nur zwei kennt, verbucht das Nichtlaufen als Erfolg. `js_tests.lauf()` gibt deshalb
alle drei zurück, und der teuerste Test der ganzen Strecke ist der, der verlangt, dass
`übersprungen` **nicht** `ok` ist.

**Regel 2 — die Meldung nennt, was der Leser dagegen tun kann.** „JS-Teststrecke
übersprungen" allein ist eine Sackgasse. Die Zeile nennt deshalb `node` und den offenen
Decision Request `p12/T-0007`. Eine Meldung ohne Handlungsweg wird nach dreimal Lesen
Tapete.

**Regel 3 — was die Trennung tragen soll, braucht eine Prüfung, nicht nur ein ADR.**
ADR-008 sagt: Entscheidungen nach `regeln.js`, Darstellung in `app.js`. Sprint 11 hat
**dreimal** gemessen, was eine aufgeschriebene Regel ohne Prüfung wert ist. Also steht die
Prüfung im selben Lauf: `regeln.js` enthält im **Code** kein `document.`/`fetch(`, und
`app.js` liest die vier Regeln aus `Regeln` statt sie zu wiederholen.

⚠ **Nebenbefund am eigenen Test:** der erste Entwurf dieser Prüfung wurde rot an einer
**Kommentarzeile** — `regeln.js` erklärt in seinem Kopf, warum es `document.createElement`
nicht anfasst. Eine Prüfung, die die Begründung bestraft, erzieht zum Weglassen der
Begründung. Kommentare werden deshalb vor der Prüfung entfernt — mit einer Gegenprobe, dass
diese Nachsicht keinen echten Zugriff durchlässt.

## L-2026-08-17aq — Ein Fehler, den man dem System durch Wegnahme seiner Voraussetzungen beibringt, ist ein anderer Fehler als der, den man messen wollte

**Anlass (Sprint 17, `pm/T-0065` / SWR-144).** Die Zusicherung für die Rücknahme bei
gescheitertem Commit schob `.git` weg, um den Commit scheitern zu lassen. Gemessen wurde
dann etwas anderes: ohne `.git` findet `aggregation.projekte` das Projekt nicht mehr, der
Aufruf endet mit **404** statt **503** — und die **Rücknahme wird nie erreicht**. Der Test
war rot, und zwar aus dem falschen Grund.

Gemessen wird jetzt an einem **kaputten Git-Index**: das Repo bleibt ein Repo, `git add`
scheitert. Dasselbe Muster hat in derselben Session `test_gruendung_vorlegen` übernommen.

**Regel:** Wer einen Fehlerpfad prüft, nimmt dem System **genau eine** Voraussetzung weg —
und zwar die, deren Fehlen der geprüfte Pfad behandelt. Nimmt man mehr, gewinnt eine
frühere Prüfung, und die spätere bleibt ungemessen. Der Test dokumentiert, **welchen**
Fehler er erzeugt, sonst wandert er beim nächsten Refactoring stillschweigend auf einen
anderen.

## L-2026-08-17ar — Ein Commit, der eine unveränderte Datei mitnimmt, gibt es nicht — und eine Zusicherung, die ihn verlangt, misst die Absicht des Testschreibers

**Anlass (Sprint 17, `pm/T-0065` / SWR-144).** Der erste Entwurf verlangte, dass der
Terminierungs-Commit **Ticket und `BOARD.md`** enthält — nach dem Muster von SWR-078, wo
genau das gilt. Er wurde rot.

Der Grund ist kein Fehler: `generiere_board` führt die Spalte `Sprint` aus dem Feld
`sprint`, **nicht** aus `geplant_sprint`. Eine Terminierung ist eine Änderung, die auf dem
Board **unsichtbar** ist — das Board wird regeneriert und ist byteweise dasselbe, also hat
Git nichts zu committen.

Gemessen wird jetzt, was **gilt**: das Ticket steht im Commit, `BOARD.md` **nicht**, und
`BOARD.md` ist danach deckungsgleich mit einer frischen Regeneration — nicht veraltet,
sondern gleich geblieben.

**Regel:** Eine Zusicherung über den **Inhalt** eines Commits wird gegen das gehalten, was
sich wirklich ändert, nicht gegen ein Muster von einer anderen Stelle. Vor dem Übernehmen
eines Musters wird geprüft, ob dessen Voraussetzung hier gilt.

## L-2026-08-17as — „Nichts passiert" und „hat nicht funktioniert" brauchen zwei Gestalten, und die Unterscheidung gehört in einen Typ, nicht in eine Meldung

**Anlass (Sprint 17, `pm/T-0065` / SWR-144).** `board.aktualisiere` wirft bei einem Feld,
das den gewünschten Wert schon trägt, einen `ValueError` — und `tickets.speichere`
übersetzte **jeden** `ValueError` in HTTP **400**. Ein bereits terminiertes Ticket
antwortete damit in **derselben Gestalt** wie ein abgewiesener Schreibvorgang.

Ein Aufrufer, der beides trennen wollte, hätte auf den **Wortlaut** der Meldung prüfen
müssen — und dass eine Textsuche eine Warnung nicht von ihrem Gegenstand unterscheiden
kann, hat `L-2026-08-17ak` in Sprint 16 gemessen.

Gebaut ist ein **eigener Typ** (`board.KeineAenderung`, weiterhin `ValueError`, damit kein
bestehender Aufrufer sich ändert), und der Fall antwortet **200** mit `unveraendert: true`
und **ohne Commit**. ⚠ Ein Commit ohne Änderung schriebe ein Ereignis in die Historie, das
nicht stattgefunden hat — und die Historie ist die Quelle von `status_in_head`.

**Regel:** Wo ein Aufrufer zwei Ausgänge unterscheiden muss, unterscheidet der **Typ** sie,
nicht die Prosa. Und der Erfolgsfall „es war nichts zu tun" ist ein Erfolg: eine Oberfläche,
die ihn wie einen Fehler zeigt, lehrt ihre Benutzer, Fehler zu ignorieren.

⚠ Die Unterscheidung muss **bis in die Ansicht** durchgehalten werden — drei Zustände, drei
Gestalten. Im Server erfüllt und in der Anzeige verloren ist sie nicht erfüllt.

## L-2026-08-17au — Ein Beleg, der als Satz im Ticket steht, ist keiner

**Anlass (Sprint 18, `promt-team/T-0007` / SWR-149).** Die DoD verlangte wörtlich: *„Real
heißt: aus dem Bestand belegt, nicht ausgedacht."* Dieser Satz stand seit Sprint 15 im
Ticket. **Keine Prüfung liest einen Satz.** Ein Lauf hätte 51 plausibel formulierte Fälle
schreiben und die DoD als erfüllt melden können — und nichts hätte widersprochen.

Gebaut ist `herkunft` als **Pflichtfeld mit Belegstelle** (`pfad::suchtext`), gegen den
Bestand aufgelöst. ⚠ Die **Stelle** ist die eigentliche Prüfung:

> **Eine Datei existiert auch für einen erfundenen Fall. Erst die Belegstelle darin
> unterscheidet „aus dem Bestand" von „plausibel formuliert".**

**Regel:** Verlangt eine DoD, dass etwas **echt** ist, dann gehört der Beleg als **Feld** in
das Artefakt und als **Auflösung** in die Prüfung. Eine Echtheitsforderung in Prosa ist eine
Bitte an den nächsten Lauf, ehrlich zu sein — und `L-2026-08-17ag` hat an einem Tag dreimal
gezeigt, wie zuverlässig das ist.

⚠ Und die Formentscheidung gehört dazu: `herkunft` als **Zeichenkette** wird abgelehnt. Ein
Satz und ein Pfad sehen als Zeichenkette gleich aus, und nur einer von beiden lässt sich
auflösen — dieselbe Entscheidung, die `fehlschlag_erkannt_an` gegen Prosa schon getroffen
hatte, am Nachbarfeld **angewandt statt ein zweites Mal gelernt**.

## L-2026-08-17av — Eine Prüfung, die überall aufgeht, ist grün und wertlos

**Anlass (Sprint 18, `promt-team/T-0007` / SWR-149).** Der erste Entwurf des Goldsets war
**mangelfrei**: 51 Fälle, Form geprüft, Herkunft aufgelöst, Registry geprüft, Quote 100 %.
Eine zweite Messung, die die DoD **nicht** verlangt hatte — **Trennschärfe** —, hat ihn
widerlegt: von 46 textbasierten Prüfungen gingen **41** auch in **fremden** Artefakten des
Sets auf.

> **Eine Prüfung, deren Suchtext überall steht, geht auf, ohne etwas zu unterscheiden. Ein
> Goldset aus solchen Prüfungen ist kein Maßstab, sondern ein Maß für die Beliebigkeit
> seiner Suchtexte — und es ist grün.**

Geschärft auf **2 von 40** vor dem ersten Commit.

**Regel:** Wer einen Maßstab baut, misst **auch dessen Auflösungsvermögen** — nicht nur, ob
seine Prüfungen aufgehen, sondern ob sie am **falschen** Gegenstand **nicht** aufgehen. Ohne
diese zweite Messung ist eine grüne Quote von einer leeren Prüfung nicht zu unterscheiden.

⚠ **Die Grenze der Messung gehört in denselben Bericht:** sie kann einen beliebigen Suchtext
nicht von einem Suchtext über eine **Konvention** unterscheiden. Gilt eine Regel absichtlich
an vielen Stellen, **soll** ihre Prüfung überall aufgehen. Deshalb wird die Zahl
**berichtet und nicht erzwungen** — ein Gate dort verböte richtige Fälle (SWR-131).

## L-2026-08-17az — Ein Muster im Quelltext ist eine Absicht; erst der Knoten, der danach dasteht, ist ein Befund

**Anlass (Sprint 19, `p12/T-0006`).** Der Code-Zaun-Zweig war gebaut, der Zähltest über den
Quelltext war **grün** — und der Zweig war **toter Code**. Erreicht wurde er nie: Absatz- und
Listenpfad sammeln Folgezeilen, bis ein bekanntes Muster kommt, und ```` ``` ```` stand in
dieser Abbruchregel nicht. Ein Zaun **nach** einem Absatz wurde samt Inhalt in den Absatz
gezogen.

> **Ein neuer Block-Zweig ist erst dann erreichbar, wenn die Fortsetzungsregel des
> vorherigen Blocks ihn kennt. Beide Stellen sind für sich genommen richtig.**

Der Zähltest konnte es nicht sehen: er liest den Quelltext und fand den Zweig. Gefunden hat
es der **Verhaltenstest**.

⚠ **Und dessen erste Fassung war ebenfalls grün** — sie hatte den Zaun am **Textanfang**, wo
keine Fortsetzungsregel greift. Rot wurde sie erst mit einer Zeile davor.

**Regel:** Eine Statikprüfung darf eine neue Verzweigung **melden**, aber nicht **abnehmen**.
Zu jedem neuen Zweig gehört ein Beispiel, das ihn **im Zusammenhang** trifft — also mit dem,
was in der Wirklichkeit davorsteht. Ein Beispiel, das die Stelle nicht trifft, ist grün und
sagt nichts.

## L-2026-08-17ba — Ein Test, der den heutigen Zustand einfriert, ist erst dann etwas wert, wenn der Lauf, der ihn rot macht, ihn auch umdreht

**Anlass (Sprint 19).** Sprint 18 hatte in `test_renderweg_zaehlung.py` **drei** Zusicherungen
hinterlassen, die absichtlich den **Befund** und nicht die Lösung festhielten („der Inline-Pass
kennt die Ticketnummer heute **nicht**", „es gibt noch **keinen** Block-Pass für Zäune", der
eingefrorene Zähler bei 4). Sprint 19 hat sie rot gemacht — **genau diese drei und keine
vierte** — und sie **umgedreht** statt gelöscht.

> **Ein eingefrorener Befund ohne die Zusage, ihn beim Bauen umzudrehen, ist eine Warnung mit
> Zeitstempel. Wer ihn löscht, nimmt der Historie ihre Richtung; wer ihn stehen lässt,
> erzieht zum Wegsehen.**

⚠ Die Zahl **drei** ist dabei der eigentliche Nachweis: hätte der Umbau vier oder zwei
Zusicherungen rot gemacht, wäre entweder mehr passiert als geplant oder weniger als
versprochen.

**Regel:** Wer einen Zustand als Zahl oder als Negativzusicherung einfriert, schreibt in ihren
Docstring, **welches Ticket sie umdreht und wie**. Und der Lauf, der sie rot macht, prüft
zuerst, ob **genau die erwarteten** rot sind.

## L-2026-08-20bj — Eine Zusicherung gegen den echten Bestand, die eine Momentaufnahme festschreibt, prüft ab morgen die Uhr

**Anlass (Sprint 21, `platform/T-0024`).** `test_widget_post.BestandTest` liest den
**echten** Bestand — richtig, und ausdrücklich die Lehre aus SWR-128 („Attrappen genügen
nicht"). Sie hält ihn aber gegen ein **fest eingetragenes Datum** (`"2026-08-16"`). Am
**2026-08-17 um 22:22** ist ein neuer Digest entstanden; seitdem ist die Zusicherung rot.

⚠ Zwei Dinge machen das teuer:

1. Der Digest kam **vier Minuten nach dem Abschlussbericht von Sprint 20** (22:18). Das
   ist wortwörtlich der Befund jenes Sprints — nur mit einem Artefakt statt einer
   Entscheidung. **Was nach dem Abschlussbericht eintrifft, ist für ihn unsichtbar und
   für den nächsten Lauf Altbestand.**
2. Die Zusicherung stand **drei Tage rot**, ohne dass es jemandem aufgefallen wäre, weil
   in diesen drei Tagen niemand die volle Suite gelaufen ist.

> **Ein Test gegen den echten Bestand prüft mehr als eine Attrappe. Schreibt er dabei
> eine Momentaufnahme fest, prüft er ab dem nächsten Tag die Uhr statt den Code — und
> sein rotes Kreuz sagt nichts über die Software.**

**Regel:** Eine Zusicherung gegen einen wachsenden Bestand formuliert eine **Invariante**
(„der jüngste Digest ist der jüngste"), keine Momentaufnahme („es ist der vom 16.08.").
Und die richtige Reaktion auf so ein rotes Kreuz ist **nie**, die Erwartung
hochzuzählen — das verschiebt den Befund nur bis zum nächsten Digest.

## L-2026-08-20bp — Erst zählen, dann reparieren: die Zählung war der Ertrag, nicht die Reparatur

*Anlass: Sprint 22, `platform/T-0024` Frage 3 (SWR-157).*

Ein Test hielt den jüngsten Mail-Digest gegen das feste Datum `2026-08-16` und war seit
drei Tagen rot. Die Reparatur einer einzelnen Zeile hätte zehn Minuten gedauert. Das
Ticket verlangte davor eine **Zählung**: *wie viele Zusicherungen derselben Bauart gibt
es?*

Gemessen über alle 66 Testdateien mit dem **Syntaxbaum**:

| Bauart | Fundstellen |
|---|---|
| fester Wert gegen den echten Bestand (`assertEqual`) | **1** |
| **Schranke** gegen den echten Bestand (`assertGreaterEqual`) | **2** |

**Regel 1 — die beruhigende Zahl war nicht die interessante.** Dass es nur eine
Fundstelle gibt, heißt: die Reparatur ist eine und keine Kampagne. Dass die **richtige**
Gegenbauart an zwei Stellen längst existierte, heißt etwas anderes:
> **Der Fehler war nicht Unwissen, sondern eine Gelegenheit — die Zahl stand gerade da
> und war richtig.** Das Haus konnte es; an dieser einen Stelle hat es zugegriffen.
Eine Regel, die „kennt die richtige Bauart nicht" unterstellt, hätte nichts geändert.

**Regel 2 — gezählt wird mit dem Syntaxbaum und nicht mit einer Textsuche.** Diese
Organisation hat an zwei Tagen **fünf** Fehlalarme kassiert, weil eine dateiweite Suche
an einem Kommentar oder einer richtigen Nachbarregel ansprang. Der erste Wurf dieser
Zählung fand entsprechend 15 statt 1 — darunter ein `len(ZUSTAENDE) == 3`, das eine
Konstante im Code prüft und kein Verzeichnis.

**Regel 3 — eine Zählung, die auf 0 steht, braucht eine Gegenprobe.** Sie ist sonst von
einer kaputten Zählung nicht zu unterscheiden (`L-2026-08-17ai`). Die Prüfung stellt
deshalb ein Testmodul mit genau der alten Bauart **her** und muss es finden.

**Regel 4 — und sie sagt, was sie nicht kann.** Sie erkennt ein ISO-Datum als Literal,
keine beliebige festgeschriebene Zahl. Das steht im Docstring, weil eine Prüfung, die
mehr zu decken scheint als sie deckt, schlimmer ist als keine.

## L-2026-08-20bq — Die tragende Annahme einer Auswahl stand als Kommentar daneben

*Anlass: Sprint 22, Nebenbefund beim Reparieren von `platform/T-0024`.*

`post_widget` nimmt je Takt das **erste** Vorkommen aus der Digestliste und begründet das
im Kommentar: *„`digest_liste` liefert neueste zuerst — es wird hier nicht ein zweites
Mal sortiert."* Die gesamte Auswahl „jüngster Digest" hing daran. Keine Zusicherung hat
je geprüft, dass die Liste wirklich absteigend sortiert ist.

Der rote Test daneben prüfte stattdessen, dass das Ergebnis **an einem bestimmten Tag**
`2026-08-16` lautete — also die Folge der Annahme an einem Stichtag statt der Annahme
selbst.

**Regel — wo ein Kommentar eine Annahme trägt, gehört die Annahme in eine Zusicherung**
(`L-2026-08-17ag`, hier zum wiederholten Mal). Der Kommentar darf bleiben; er erklärt.
Prüfen tut er nichts.

## L-2026-08-20bt — Die Zusicherung, die den eigenen Entwurf verwirft, ist die wertvollste im Bestand

*Anlass: Sprint 23, `platform/T-0026` → SWR-159.*

Der erste Entwurf der Uhrenprobe holte sein Material mit einem eigenen `git log` **in
`sprint_register`**. Er wurde rot an `test_die_messung_ruft_KEIN_git_auf` — einer
Zusicherung aus **Sprint 16**, die seit SWR-134 im Bestand steht: auf diesem Mount
hinterlässt schon ein **lesender** Git-Aufruf eine `index.lock`, die niemand mehr löschen
kann.

> **Eine Prüfung, die Uneinigkeit zwischen zwei Läufen erkennen soll und dabei selbst
> sperrt, ist ihr eigener Schadensfall. Gefunden hat das nicht der Entwurf, sondern eine
> Regel, die sieben Sprints früher jemand aufgeschrieben und in einen Test gegossen hat.**

**Regel 1 — eine Regel, die nur in einem Kommentar steht, hätte hier nichts verhindert.**
Der neue Code hat den Kommentar nie gelesen; der Test hat ihn gestoppt. Das ist die
Gegenprobe zu `L-2026-08-20bs` aus demselben Lauf, und beide zeigen dasselbe von zwei
Seiten.

**Regel 2 — die Reparatur trennt Material und Regel, statt die Zusicherung zu
erweichen.** Die Versuchung war, den Test „für diesen Fall" auszunehmen. Stattdessen holt
`uebergang_historie` das Material (dort ist Git-Lesen ohnehin zu Hause), die Zeitregel
bleibt beim Sprintzähler (dort wohnt `_wanduhr`), und der Preflight fügt beides zusammen.
*Eine Zusicherung, die man beim ersten Widerspruch aufweicht, hätte man nicht schreiben
müssen.*

**Regel 3 — wiederhole die Zusicherung dort, wo der nächste Erweiterer sie sucht.** Sie
steht jetzt zusätzlich in der Teststrecke der neuen Prüfung. Wer diese Prüfung erweitert,
liest nicht die Tests des Sprintzählers.

## L-2026-08-20bu — Wer eine Sperre baut, muss zuerst sagen, was genau der Inhalt ist

*Anlass: Sprint 23, `projects/p11/T-0013` → SWR-160.*

Das Ticket lautete „Mail-Widget hinter dem PIN-Lesegate". Beim Bauen war die schwierige
Frage nicht *wie* gesperrt wird, sondern *was*. Die Antwort steckte in **derselben
Rubrik** des Digests:

> **Die ANZAHL offener Punkte ist eine Kennzahl. Ihr WORTLAUT sind Betreffzeilen,
> Absender und Links.**

**Regel 1 — Zahl und Wortlaut müssen aus DERSELBEN Auswahl kommen.** Zwei
Auswahlregeln über eine Rubrik wären B033: die offene Kachel zählte dann andere Punkte,
als hinter dem Gate stehen — und niemandem fiele es auf, weil beide Zahlen plausibel
aussehen.

**Regel 2 — die Gegenprobe lautet „nichts", nicht „weniger".** Geprüft wird gegen
Zeichenfolgen, die im **echten** Digest nachweislich vorkommen. Ein Gate, das eine
gekürzte Fassung durchlässt, ist grün, solange man nur „weniger" prüft — *ein Leck mit
gutem Gewissen*.

**Regel 3 — eine Sperre ist kein Datenzustand.** `wert`/`echte_null`/`nicht_geliefert`
sagen, WAS die Quelle liefert; eine Sperre sagt, WER es sehen darf. Ein vierter Wert wäre
die kürzeste Änderung gewesen und hätte „keine Daten" mit „nicht für dich" verwechselt.

**Regel 4 — die Kachel verschwindet nicht.** *Eine Kachel, die ohne PIN weg ist, verrät
nichts und behauptet dabei, es gäbe hier nichts.* Sie bleibt stehen und sagt, dass ihr
Inhalt eine Freigabe braucht.

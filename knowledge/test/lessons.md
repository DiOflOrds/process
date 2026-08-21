
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

---

## L-2026-08-20bz — Eine Zusicherung, die von einem neuen Bau rot wird, wird geschärft und nicht gelöscht — zum dritten Mal dieselbe Bewegung

**Anlass (Sprint 24, SWR-163).** `test_ohne_pfade_gibt_es_kein_zwischenraeumen` verlangte,
dass ohne `add` **überhaupt nicht** geräumt wird. SWR-163 räumt nach dem gelungenen Commit
hinter sich her — der Test wurde rot.

Er war **richtig** und seine Formulierung war **zu grob**. Seine Absicht war nie „nie
räumen", sondern **kein Räumen ohne Nachweis**. Nach dem Commit gibt es einen Nachweis, und
zwar einen stärkeren als vorher: der eigene Aufruf ist zurück.

> **Zugesichert wird ab jetzt die REIHENFOLGE und nicht mehr die Abwesenheit.**

**Regel 1 — dieselbe Bewegung ist in diesem Haus jetzt dreimal gefallen** (Sprint 16 an der
Nachbarmethode, Sprint 17 an `kompaktKachel`, Sprint 24 hier). Dreimal ist ein Muster:
**eine Zusicherung, die eine Abwesenheit misst, ist fast immer die grobe Fassung einer
Zusicherung über eine Reihenfolge oder eine Bedingung.** Wer eine schreibt, sollte sie
gleich in der feineren Form schreiben.

**Regel 2 — der Beleg dafür, dass geschärft und nicht aufgeweicht wurde, gehört in den
Docstring, nicht in die Commit-Nachricht.** Sonst ist eine Verschärfung von einer
Aufweichung nach zwei Sprints nicht mehr zu unterscheiden — beide sehen aus wie „Test
angepasst".

**Regel 3 — eine Zusicherung darf ihren Gegenstand nicht selbst erzeugen.** `_nachraeumen`
darf **kein git** aufrufen (Auflage aus SWR-159); geprüft wird das, indem der Test
`subprocess.run` beobachtet — nicht indem der Code es verspricht.

---

## L-2026-08-20cd — Ein Zähltest über ein Literal hat in zwei Läufen zweimal den eigenen Entwurf verworfen

**Anlass (Sprint 24, SWR-165).** Die Anforderung verlangt wörtlich, der Rumpfmarker einer
Inbox-Entscheidung stehe an **einer** Stelle. Ihr erster Entwurf legte in `aggregation`
eine **zweite** Konstante dafür an. Rot gemacht hat das nicht der Autor, sondern
`test_dr_verbuchung.test_keine_zweite_kopie_des_markers_im_quelltext` — eine Zusicherung
aus **Sprint 17**, die nichts anderes tut, als zu zählen, wie viele Dateien das Literal
`"**Entscheidung ("` führen.

> **Zum zweiten Lauf in Folge hat eine ältere Zusicherung den eigenen Entwurf verworfen.**
> Sprint 23: SWR-134 gegen die Uhrenprobe (`test_die_messung_ruft_KEIN_git_auf`).
> Sprint 24: SWR-131 gegen den eigenen Marker. **Beide Male sah der Entwurf für seinen
> Autor richtig aus.**

**Regel 1 — Zähltests über Literale wirken pedantisch und sind die billigste
B033-Bremse, die dieses Haus hat.** Sie prüfen nichts über das Verhalten und alles über
die Bauart. Wer sie beim Aufräumen entfernt, weil sie „nichts testen", entfernt genau den
Wächter, der in zwei aufeinanderfolgenden Läufen zugeschlagen hat.

**Regel 2 — die gefährlichste Dopplung entsteht beim Bau der Anforderung, die sie
verbietet.** Der Autor hat den Satz im Kopf und schreibt trotzdem die zweite Stelle, weil
die erste in einer anderen Datei liegt. Ein Satz im Docstring hätte es nicht verhindert —
er stand da.

**Regel 3 — die Zählung gehört an EINE Stelle, auch als Test.** Die neue Teststrecke prüft
deshalb nur, dass `inbox` den Marker aus derselben Konstante schreibt; die Zählung der
Kopien bleibt drüben. Sie hier zu wiederholen wäre dieselbe Dopplung eine Ebene höher.

---

## L-2026-08-20cf — Zum VIERTEN Lauf in Folge hat ein Werkzeug den frischen Entwurf verworfen (Sprint 25)

| Lauf | Entwurf | Was ihn rot gemacht hat |
|---|---|---|
| Sprint 22 | Uhrenprobe rief git auf | `test_die_messung_ruft_KEIN_git_auf` (SWR-134, Sprint 16) |
| Sprint 23 | SWR-159, dieselbe Stelle | dieselbe Zusicherung |
| Sprint 24 | SWR-165 legte eine **zweite** Markerkonstante an | Zähltest aus Sprint 17 (`test_dr_verbuchung`) |
| **Sprint 25** | `p12/T-0012` behauptete, es gehe **nicht** in die Inbox | **`board.py`** wies den DR ab (`optionen` fehlt); `inbox.py` legt **jeden** offenen DR vor |
| **Sprint 25** | `scripts/organigramm.py` (Orga-Rework) ohne `konsole.sichere_ausgabe()` | `test_konsole.test_jeder_einstiegspunkt_sichert_seine_ausgabe` (platform/T-0009) |
| **Sprint 25** | `p12/T-0012` **ohne Frist** — „eine Darstellungsfrage darf nicht drängen" | **`preflight`** meldete es als unterminiertes Ticket (SWR-114): *ein DR ohne `frist` ist unterminiert* |

> **DREIMAL in EINEM Lauf, und jedes Mal war der Entwurf plausibel: der erste beschrieb,
> wie er behandelt werden wollte, der zweite vergaß eine Regel für allen Produktionscode,
> der dritte hielt Rücksicht für Freundlichkeit. Kein Mensch hat eines davon beim Lesen
> bemerkt.**

**Regel 0 — eine Frage ohne Frist ist keine schonende Frage**, sondern eine, deren Ausgang
niemand aufgeschrieben hat. Rücksicht gehört in den **Default**, nicht in die Abwesenheit
einer Frist.

**Regel 1 — eine Prüfung, die über den GESAMTEN Produktionscode läuft, erfasst auch den
Code, den es beim Schreiben der Prüfung noch nicht gab.** Das ist ihr eigentlicher Wert und
der Grund, sie nicht auf eine Liste umzustellen.

**Regel 2 — ein Ticket beschreibt nicht, wie es behandelt wird.** Der Satz „das geht nicht
in die Inbox" ist eine Behauptung über `inbox.py` und gehört dort nachgesehen — sonst ist
er der Kommentar, der drei Sprints lang das Gegenteil des Codes sagt (SWR-162).

**Regel 3 — Zähl- und Regeltests wirken pedantisch und haben in vier aufeinanderfolgenden
Läufen je einen Fehler verhindert.** Das ist das Argument, sie zu behalten.

---

## L-2026-08-20ck — Eine Bedingung, die ein Fehlschlag erfüllt, ist keine Bedingung (Sprint 26)

**Der Fall.** Drei Tickets warteten auf *„mindestens einen durchgelaufenen Tick"*. Drei
Ticks liefen durch — alle drei mit `status=fehler` und null Artefakten. Die Bedingung war
**wörtlich erfüllt und inhaltlich nicht**.

Im selben Lauf stand daneben derselbe Fehler eine Ebene tiefer: `print("Tick
abgeschlossen…")` lief unbedingt vor `return 0`.

> **Beides misst das ENDE eines Vorgangs und nennt es sein ERGEBNIS. „Durchgelaufen" und
> „abgeschlossen" sind Aussagen über den Kontrollfluss, nicht über die Arbeit.**

**Regel 1 — eine Bedingung nennt das Ergebnis, nicht den Abschluss.** Nicht „ein Tick ist
gelaufen", sondern „ein Tick hat `status: ok` **und** mindestens ein Artefakt".

**Regel 2 — Zusicherungen über den ECHTEN Bestand fangen, was zwischen zwei Läufen
passiert.** `test_dr_verbuchung` wurde mitten in diesem Lauf rot, weil der Auftraggeber um
20:34 eine Entscheidung eingetragen hatte. Kein Preflight hätte das gefunden — er war
vorher gelaufen. *Die rote Zeile war nicht die Störung des Laufs, sie war sein Ergebnis.*

**Regel 3 — eine Prüfung auf ein WORT prüft den Text, nicht den Code.** Der erste Entwurf
von `test_kein_return_im_finally` suchte nach „return" und wurde von dem Kommentar rot
gemacht, der das Verbot **erklärt**. Geprüft werden Anweisungen.

**Regel 4 — Namen werden abgelesen, nicht gebildet.** Der erste Entwurf von
`test_am_echten_bestand…` erwartete `MAIL-RED@mail`, aus dem Team-Kürzel abgeleitet. Die
Instanz heißt `MAIL-RED@team-mail`. *Zwei Entwürfe, zwei Tests, beide Male hat das Werkzeug
seinen eigenen Verfasser widerlegt — zum fünften Lauf in Folge.*

**Regel 5 (Nachtrag Sprint 26) — ein beantworteter Brief ist keine getroffene
Entscheidung.** Die Planzeile zu `p9/T-0008` sagte „erledigt", weil der Brief beantwortet
war; der Entscheidungsantrag darin war offen. Der Preflight hat es als Plan-Drift gemeldet.
*Hätte die Zeile so gestanden, wäre der Zähler „auf dich wartende Entscheidungen" um eins
zu niedrig gewesen — in genau dem Bericht, der ihn nennt.* **Dritte Werkzeug-Widerlegung
in diesem Lauf.**

---

## L-2026-08-20cq — Eine Prüfung kann dadurch falsch werden, dass ein Ticket erledigt wird

**Sprint 27.** Der Preflight meldete bei der Schlussverifikation einen Plan-Drift für
`promt-team/T-0008` — Planzeile und Ticket sagten beide **Sprint 28**. Ausgelöst hat den
Befund eine **andere** Zeile: `p9/T-0008`, in genau diesem Lauf geschlossen und deshalb
nicht mehr in der Menge der offenen Tickets. Die Auflösung fiel auf die nackte `T-0008`
zurück, und die war **unter den offenen** eindeutig.

> **⚠⚠ Die Eindeutigkeitsprüfung lief über die offenen Tickets, die Planzeile gehörte
> einem geschlossenen. Eine ID wird nicht dadurch eindeutig, dass die Restmenge klein
> ist.**

⚠ Das Schwestermodul `statusdrift` beantwortet dieselbe Frage über **alle** Tickets und ist
nie in diese Falle gelaufen. Zwei Funktionen, eine Frage, zwei Grundmengen — und nur eine
davon war richtig gewählt.

**Regel.** Wer eine Kennung über eine **Teilmenge** auf Eindeutigkeit prüft, prüft nicht
Eindeutigkeit, sondern Seltenheit. Die Grundmenge einer Namensauflösung ist der
**Gesamtbestand**; die Teilmenge darf höchstens entscheiden, was danach gemeldet wird.

⚠ Und die Beobachtung, die über den Einzelfall hinausgeht: **eine Prüfung kann durch das
Schließen eines Tickets falsch werden.** Der Befund existierte vor diesem Lauf nicht und
wäre ohne ihn nie entstanden. Wer Grundmengen aus dem Bestand bildet, ändert sie mit jeder
Statusänderung mit.

## L-2026-08-21cz

**Regel:** Eine Zusicherung, die einen Namen im **Quelltext** sucht, muss den Docstring
ausschließen — sonst ist sie durch **Prosa** erfüllt. Und eine Zusicherung, die eine ganze
**Datei** von ihrer Prüfung ausnimmt, hat keinen Ausnahmefall, sondern einen blinden Fleck.

Beides ist in Sprint 33 gleichzeitig eingetreten und beides hat das unabhängige Gegenlesen
gefunden:

* `test_kennzahlen_gehen_durch_die_tuer` suchte `"board.briefkasten_dateien"` als Text —
  der Name stand bereits im Docstring derselben Funktion. Die Hauptzusicherung war erfüllt,
  ohne dass ein Aufruf existieren musste.
* `test_kein_literal_ausserhalb_von_board` nahm `board.py` **pauschal** aus und konnte
  deshalb den **fünften** Namen derselben Menge (`board.GESCHLOSSEN`) strukturell nie
  finden — er saß ausgerechnet dort, wo die Menge wohnt.

> **Anwesenheit ist nicht Verwendung. Und eine Ausnahme für eine ganze Datei ist keine
> Ausnahme, sondern ein blinder Fleck an der teuersten Stelle: dort, wo die Sache wohnt.**

⚠ Dritter Fall im selben Lauf: `test_keine_toten_sperren_ausser_dem_bestand` war
**vakuum-grün** — sie verglich `[] == []`, und ihr `except Exception` hätte ein komplett
unladbares Haus grün gemacht. Die Schwester-Zusicherung nebenan trug die passende untere
Schranke seit ihrer ersten Zeile.

**Verbleib:** Rollenkarte TEST/QM, Vertreter
`platform/tests/test_brief_discovery.py::_ruft_auf` (AST statt Text),
`platform/tests/test_tote_sperre.py::test_kein_literal_ausserhalb_von_board` (eine Zeile
statt einer Datei) und `::test_keine_toten_sperren_ausser_dem_bestand` (untere Schranke).


## L-2026-08-21dg

**Regel:** Jeder **Halbsatz** einer Anforderung braucht seine eigene Zusicherung. Eine
Anforderung, die drei Dinge verlangt und zwei davon prüft, ist zu zwei Dritteln eine
Absichtserklärung — und der ungeprüfte Teil ist erfahrungsgemäß der, der schon einmal
gefehlt hat.

`SWR-208` verlangte wörtlich, **jeder** Gründungsweg schreibe `status: aktiv`. Das
unabhängige Gegenlesen entfernte den Halbsatz aus **beiden** Wegen und fuhr 139 Tests:
**kein einziger wurde rot.**

> **⚠⚠ Genau dieser Halbsatz hatte `projects/p13` sein Core Team gekostet. Die
> Anforderung, die den Fehler repariert, hatte ihn selbst nicht abgesichert.**

⚠ Zwei weitere Fälle derselben Sorte im selben Lauf:

* **Eine Auflage, die nur im FEHLERTEXT steht,** gilt genau so lange, wie niemand sie
  brechen will. *„Der Tag sitzt auf dem Abschluss-Commit, nie auf HEAD"* stand in der
  Meldung einer anderen Prüfung — sie ist eine Bitte und keine Schranke.
* **Eine Verfallsprüfung, die etwas anderes misst als das, was die Ausnahme verfallen
  ließe,** ist ein zweiter Name für „grün". Die Ausnahme ließ sich spurlos löschen.

⚠ Und der teuerste Einzelbefund war eine **selbst aufgestellte** Falle: derselbe Bau, der
die B033-Doppelung benannte, legte eine neue an — mit einem **Kommentar**, der die
Gleichheit zweier Mengen *behauptete*. **Ein Kommentar, der Gleichheit behauptet, stellt
sie nicht her; `assertIs` tut es.**

**Verbleib:** Rollenkarte TEST, Vertreter
`platform/tests/test_datenklasse_platziert.py::test_jeder_gruendungsweg_schreibt_status_aktiv`,
`::test_die_datenklassen_sind_EINE_menge_und_keine_kopie`,
`platform/tests/test_baseline_folgt_der_abnahme.py::test_jede_namensausnahme_wird_tatsaechlich_gebraucht`,
`::test_der_tag_ist_nicht_juenger_als_seine_abnahme` und
`platform/tests/test_vertragsversion_eine_zahl.py`.

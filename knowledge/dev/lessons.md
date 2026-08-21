
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

---

## L-2026-08-21cj — Eine Prüfung NACH der Auswahl ist kein Filter, sondern ein Veto gegen genau einen Kandidaten

**Sprint 30, `SWR-196` / `platform/T-0048`.** Der Ollama-Schnelltakt lief zum ersten Mal
bis zur Ticketauswahl durch (`SWR-191` hatte den falschen Preflight-Befund beseitigt, der
65 Ticks vorher abgebrochen hatte) — und **2 von 2** Ticks endeten trotzdem ohne Ergebnis.
`waehle_ticket()` gab `kandidaten[0]` zurück; die Besetzungsprüfung lief **danach** und
brach ab. Ein Ticket der besetzten Rolle auf Platz 2 wäre nie angesehen worden.

> **Der Unterschied ist nicht kosmetisch: ein Filter entscheidet über die MENGE, ein Veto
> über ein EXEMPLAR. Wer die Prüfung ans Ende stellt, hat die Menge nie geprüft.**

⚠ Die Gegenprobe muss den Unterschied treffen können: eine Zusicherung, die nur *„am Ende
kommt nichts heraus"* misst, ist bei einem Veto **ebenfalls grün**. Nur ein Kandidat, der
**hinter** einem untauglichen steht, trennt die beiden Bauformen.

**Regel:** Ein Auswahlkriterium gehört in die Auswahl. Steht eine Prüfung hinter einer
Sortierung, gehört zu ihr eine Zusicherung mit einem **hinteren** Treffer — sonst ist
nicht belegt, dass sie mehr als den Sieger ansieht.

---

## L-2026-08-21cl — Eine wahre, aber zu enge Meldung über einen strukturellen Zustand ist der Zwilling des falschen Befunds

**Sprint 30, `SWR-196`.** Die alte Meldung — *„Rolle CM hat in Einheit 'platform' keine
Besetzung … T-0001 bleibt unangetastet"* — war **wahr**. Sie las sich aber wie ein
Zufall: *dieses eine Ticket* passte nicht. Gemessen war ein Zustand: **0 von 8** offenen
Tickets der Organisation trugen eine ollama-besetzte Rolle.

> **Die enge Aussage lädt zum Wiederkommen in 15 Minuten ein — und der Takt hat das
> 90-mal getan. Die weite sagt, dass Warten die falsche Handlung ist.**

`L-2026-08-21ce` hat gelernt: *ein falscher Befund ist teurer als kein Befund*, weil er
keine Handlung kennt, die ihn abstellt. Dies ist der Zwilling: eine **wahre** Meldung, die
zu klein zugeschnitten ist, erneuert bei jedem Lauf die Hoffnung auf eine andere Antwort.
**Sie trainiert dasselbe Wegsehen mit besserem Gewissen.**

⚠ Der Zustand war seit dem 20.08. gemessen — die Zahl stand im **Docstring einer
Funktion**, nicht in der Meldung, die der Betrieb 90-mal gedruckt hat.

**Regel:** Eine Absage nennt den **Bestand** und nicht das Exemplar: wie viele geprüft
wurden, welcher Art sie sind, und welche Handlung den Zustand abstellt. Eine gemessene
Zahl über einen Betriebszustand gehört dorthin, wo der Betrieb sie liest.

## L-2026-08-21cm

**Ein Stellvertreter, der lange mit der Sache zusammenfiel, wird zum Loch in dem Moment,
in dem die Sache einen eigenen Namen bekommt.**

`sprint_vergangen` nahm `decision-request` aus, mit wörtlich der Begründung, die
`blocked` braucht: *„das Team kann ihn nicht bewegen, und eine Sprintnummer daneben wäre
eine Zusage, die das Team nicht halten kann."* Die Ausnahme saß an einem **Typ**, weil
`decision-request` bis Sprint 29 der einzige Weg war, „das Team kann hier nicht handeln"
auszudrücken. Seit `SWR-193` gibt es `blocked` mit `blocked_by` — **einen Sprint alt** —,
und damit war die Ausnahme älter als der Zustand, den sie meinen müsste.

Die Folge war eine **Zange**: für ein gesperrtes Ticket gab es keinen zulässigen
Terminwert. Alter Sprint → Befund, leer → Befund, Zukunft → still, aber eine Zusage über
fremdes Handeln.

> **Eine Lage, in der die bequeme Handlung die einzige ist, die grün macht, ist die
> Bauart, gegen die `SWR-166` gebaut wurde.**

**Regel:** Wenn ein Zustand einen eigenen Namen bekommt, sind alle Ausnahmen zu prüfen,
die bisher an seinem Stellvertreter hingen — und die Ausnahme bindet sich an das
**Merkmal**, das die Sache trägt (hier: den `blocked_by`-Verweis), nicht an das Wort, das
sie behauptet.

*Verbleib: Rollenkarte DEV · `platform/tests/test_gesperrt_terminzange.py` ·
`platform/tests/test_termin_zange_blocked.py` · Historie `platform`.*

## L-2026-08-21cp

**Eine Zerlegefunktion, die an ihrem eigenen Ergebnis scheitert, ist nicht idempotent —
und Idempotenz ist genau das, was ein Aufrufer erwartet, der nicht weiß, ob vor ihm schon
jemand zerlegt hat.**

`board.parse_liste` kannte nur Text aus dem Frontmatter und brach mit `AttributeError`,
sobald ein Aufrufer `blocked_by` als echte Liste übergab. Gefunden hat es **eine
Zusicherung aus dem Vorsprint**, die ihre Vorrichtung so baut, wie ein Mensch die Angabe
denkt — nicht eine der neuen Zusicherungen dieses Laufs.

> **Der Fund gehört zum Ertrag der Umkehr-Bauform: eine alte Zusicherung, die einen
> Mangel BENENNT, wird beim Beheben rot und prüft den Fix an einer Vorrichtung, die der
> Autor des Fixes nicht geschrieben hat.**

**Regel:** Eine Funktion, die eine Angabe in eine Form bringt, akzeptiert diese Form auch
als Eingabe. Wer nur den Weg testet, auf dem die Daten heute ankommen, prüft die eigene
Gewohnheit.

*Verbleib: Rollenkarte DEV · `platform/scripts/board.py` (`parse_liste`) ·
`platform/tests/test_termin_zange_blocked.py` · Historie `platform`.*

## L-2026-08-21cq

**Regel:** Eine Ausnahme, deren Bedingung während der gesamten Arbeitszeit wahr ist, ist
keine Ausnahme — sie ist ein offenes Tor mit einer Aufschrift. Eine Bedingung wird an das
gebunden, was den Einzelfall **unterscheidet**, nicht an den Zustand, in dem man sich
ohnehin immer befindet.

`SWR-201` (`platform/T-0052`) nahm den Plannachlauf des **laufenden** Sprints aus der
Statusdrift. Die erste Fassung band die Ausnahme daran, **dass** ein Sprint läuft — und
während gearbeitet wird, läuft immer einer. Eine Planzeile aus **Sprint 7** mit längst
`done` Ticket wäre damit ebenfalls unterdrückt worden, obwohl die DoD des Tickets
ausdrücklich verlangte, dass `SWR-115` für vergangene Sprints **unverändert** wirkt. Die
Anforderung behauptete die Garantie im selben Satz, in dem sie sie brach.

⚠ Gefunden hat es das **unabhängige Review**, nicht der Autor und keine der sechs
Zusicherungen — sie prüften „kein Sprint läuft", einen Zustand, der während der Arbeit nie
eintritt.

⚠⚠ Zweiter Befund derselben Bauart im selben Bau: die Ausnahme hörte auf „geschlossen",
und `TICKET_GESCHLOSSEN` enthält `done` **und** `rejected`. **Eine Session hätte einen
unbequemen Befund loswerden können, indem sie das Ticket verwirft.** Ein verworfenes
Ticket ist kein „fertig, der Plan hinkt nach" — es ist eine Entscheidung, die der Plan
abbilden muss.

**Verbleib:** Rollenkarte DEV (`process/roles/dev.md`), Vertreter
`platform/tests/test_plan_nachlauf.py::test_vergangene_planzeile_bleibt_befund_obwohl_ein_sprint_laeuft`
und `::test_rejected_ist_kein_nachlauf`.

## L-2026-08-21ct

**Regel:** Eine Zusicherung, die eine Funktion direkt aufruft, sagt nichts darüber, ob der
Betrieb sie jemals ruft. Zu jedem neuen Payload-Feld gehört eine Zusicherung über die
**Verdrahtung** — und der Leser darf einen **fehlenden** Schlüssel nie per Vorgabewert in
eine leere Antwort verwandeln.

Das Review von `SWR-201` hat Aufruf **und** Payload-Schlüssel aus `sprint.plan` entfernt.
**Alle sechs** Zusicherungen blieben grün, weil sie `plan_nachlauf` direkt riefen, und der
Preflight meldete still „0", weil `sicht.get("plan_nachlauf", [])` den fehlenden Schlüssel
in eine leere Liste verwandelte.

> **Ein Vorgabewert verwandelt eine fehlende Antwort in eine beruhigende.**

⚠ Die Ironie steht drei Zeilen darüber: der Nachbar `statusdrift` gibt ausdrücklich `None`
zurück, wenn die Sicht nicht ladbar ist, *„weil ‚konnte nicht prüfen' und ‚nichts
gefunden' zwei Aussagen sind"*. Dieselbe Begründung, dieselbe Datei, im neuen Code nicht
angewandt — `B033` mit einer **Begründung** als Kopie.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_plan_nachlauf.py::test_die_verdrahtung_in_plan_wird_geprueft`.

## L-2026-08-21cw

**Regel:** Ein Zeitstempel ohne Zeitzone ist keine Zeit. Werden zwei Uhren verglichen, die
nie miteinander verglichen wurden, gehört die Umrechnung (`astimezone`) in denselben
Ausdruck wie der Vergleich — `replace(tzinfo=None)` wirft den Offset **weg** und sieht
dabei aus wie eine Normalisierung.

Sprint 33 hat `briefe_im_lauf` gebaut, um den Satz „keiner eingegangen" durch eine Messung
zu ersetzen. Briefe tragen **UTC** (`briefkasten.py`), das Sprintregister trägt
**Wanduhrzeit** (`sprint_register.py`). Der Vergleich schnitt den Offset ab: zwei Stunden
Versatz bei CEST, **länger als ein ganzer Sprint**. Die Kennzahl hätte dauerhaft `0`
gemeldet — und sie hatte, bevor jemand sie prüfte, bereits eine **Anforderung**
(`SWR-206`), ein Ticket und eine eingefrorene Regressionsschranke mit der falschen
Aussage gefüllt, das Ursprungsticket sei im Irrtum gewesen.

> **Eine Zeitzone ist keine Formatfrage, sondern eine Maßeinheit. Und eine falsche
> Messung, die eine Korrektur BEHAUPTET, ist teurer als gar keine: sie hat die richtige
> Aussage überschrieben.**

⚠ Gefunden hat es das unabhängige Gegenlesen. Weder der Autor noch eine der eigenen
Zusicherungen — die zweite Zusicherung („im Fenster von Sprint 31 kam **kein** Brief") ist
erst als Folge dieses Befunds entstanden.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_brief_discovery.py::test_zeitzone_wird_umgerechnet_und_nicht_abgeschnitten`
und `::test_das_gemessene_fenster_von_sprint_32`.


## L-2026-08-21cx

**Regel:** Eine Ausnahme braucht eine **Verfallsprüfung**. Wer eine Menge namentlich von
einer Prüfung ausnimmt, sichert im selben Zug zu, dass jeder Eintrag am echten Bestand
noch **beißt** — sonst meldet die Prüfung nicht zu viel, sondern für immer zu wenig.

`SWR-204` hat vier Verweise namentlich ausgenommen, weil eine Entscheidung des
Auftraggebers ausstand. Vier Stunden später war sie da (`pm/D029` = C), der Grund entfallen
— und **nichts** hätte es gemeldet. Die Organisation hat diese Bauform bereits einmal
gebaut (`test_uhrenprobe`: der eingefrorene Eintrag muss auf etwas Vorhandenes zeigen); sie
wurde beim zweiten Mal nicht mitgedacht.

> **Eine Ausnahme ohne Verfallsprüfung ist ein Dauerbefund mit umgekehrtem Vorzeichen.**

⚠ Der Umkehrschluss ist der eigentliche Ertrag: **eine Ausnahmeliste, die man leer bekommt,
war die richtige Bauform.** Der verworfene vierte Ticket-Zustand hätte 9 Quelldateien und
153 Literale gekostet — für eine Lage, die keine vier Stunden bestanden hat.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_tote_sperre.py::test_ausnahme_greift_oder_ist_weg`.


## L-2026-08-21cy

**Regel:** Wird eine Zuständigkeit in **eine** Funktion zusammengezogen, gehört die
**Auslegung** mit — nicht nur der Zugriffsweg. Eine vereinheitlichte Tür mit zwei
Schlüsseln ist derselbe Befund, eine Ebene tiefer.

`SWR-206` hat die Brief-**Discovery** auf `board.briefkasten_dateien` vereinheitlicht und
die Frage „ist dieser Brief offen?" doppelt gelassen: `preflight` las 300 Zeichen und
suchte einen Teilstring, `kennzahlen` zerlegte das Frontmatter und verglich
kleingeschrieben. Ein Brief mit längerem Kopf wäre von genau einem der beiden gezählt
worden — im selben Sprint, der diese Fehlerfamilie schloss.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_brief_discovery.py::test_eine_auslegung_von_offen`.


## L-2026-08-21da

**Regel:** „Kein Ende" heißt **abgebrochen**, nicht **aktiv**. Wer den „laufenden" Vorgang
sucht, nimmt den **neuesten** und fragt ihn, ob er beendet ist — nicht den neuesten, der
**kein** Ende trägt.

Gefunden hat es die **Pflicht-Nachmessung am Sprint-Ende**, nicht der Bau und nicht das
Gegenlesen: `_sprint_start` nahm den höchstnummerierten Sprint **ohne** `ende`. **15 von
33** Sprints haben nie eine Endezeile bekommen (abgebrochene Läufe). Solange Sprint 33
lief, war die Antwort richtig; in der Sekunde, in der er beendet wurde, fiel die Funktion
auf ein altes, offen gebliebenes Fenster zurück und meldete **14 Briefe statt 0**.

> **Eine Funktion, die nur solange richtig antwortet, wie der Normalfall gilt, ist im
> Ausnahmefall nicht ungenau — sie ist beliebig. Und der Ausnahmefall war hier die
> Mehrheit: 15 von 33.**

⚠ `sprint_register` führt für genau diese Lage ein eigenes Merkmal (`abgebrochen`). Es zu
haben und nicht zu lesen ist dieselbe Familie wie `SWR-198`: ein Zustand bekam einen
Namen, und der neue Leser hat ihn nicht übernommen.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_brief_discovery.py::test_nur_der_NEUESTE_sprint_kann_laufen`.


## L-2026-08-21db

**Regel:** Eine Funktion, die einen **Bezugsrahmen** braucht (Organisationswurzel, Repo,
Haus), holt ihn vom **Aufrufer** — nie aus `os.path.dirname(__file__)`. Ein Vorgabewert
aus dem eigenen Standort ist kein Vorgabewert, sondern eine stille Annahme über die Welt.

`inbox._naechste_d_id` sollte „alle Entscheidungslogs der Organisation" lesen und
bestimmte damit selbst, welche Organisation gemeint war: immer die, in der der Quelltext
zufällig liegt. Eine Vorrichtung baute ihr eigenes leeres Repo, erwartete `D001` und bekam
`D030` — die nächste freie Nummer des **echten** Hauses. Sieben Zusicherungen waren
deshalb seit Sprint 32 rot, und der Abschluss von Sprint 33 meldete *„alle 98 übrigen
Testmodule sind einzeln gelaufen"*.

> **⚠⚠ Der teuerste Teil war nicht die falsche Zahl, sondern der TOTE RÜCKFALL:
> `SWR-203` hat den Rückfall auf das einzelne Log ausdrücklich vorgesehen und mit
> `SWR-193` begründet. Er konnte nie greifen — die Wurzel war nie unbekannt, sie wurde
> erfunden. Ein erfundener Vorgabewert löscht den vorgesehenen Ausnahmepfad, ohne dass
> eine Prüfung das merkt.**

⚠ Gezählt statt geschätzt: **drei** solche Stellen über `backend/` und `scripts/`, nicht
die eine, die das Ticket nannte — bei 64 `__file__`-Fundstellen insgesamt. Die übrigen 61
lösen **Code**-Pfade auf und sind zu Recht relativ zum eigenen Quelltext.

**Verbleib:** Rollenkarte DEV, Vertreter
`platform/tests/test_org_wurzel_vom_aufrufer.py` (9 Zusicherungen, `SWR-207`).


## L-2026-08-21de

**Regel:** Wenn eine Vorlage nach einer Zahl fragt, die die Quelle nicht herstellt, wird
**keine erfunden**. Die Anzeige sagt „keine Daten", nennt den **Grund** und das **Ticket**,
unter dem die Zahl entstehen kann.

Die Design-Vorlage des Auftraggebers für das Post-Widget verlangt vier Kacheln
(`IN`, `Reaktion`, `Rechnung`, `SPAM`); `team-mail` liefert drei — eine SPAM-Rubrik gibt es
im Digest nicht.

> **⚠⚠ `0` anzuzeigen hieße „kein Spam" behaupten. Niemand sieht einer `0` an, dass sie
> erfunden ist — das ist die Verwechslung von „echte Null" und „nicht erhoben" aus
> `SWR-108`, diesmal von einer Vorlage erzwungen statt von einem Bau.**

⚠ Der zweite Teil der Regel ist der wichtigere: ein Dauerbefund **ohne** Weg nach vorn ist
die Bauform, die `SWR-166` 83 abgebrochene Läufe gekostet hat. Die Kachel nennt deshalb
`team-mail/T-0007` im Klartext.

⚠ Und ein scheinbarer Widerspruch zwischen Wunsch und Schranke war keiner: die Vorlage
zeigt den Wortlaut in der Kachel, `SWR-160` hat ihn hinter das PIN-Gate gestellt.
Aufgelöst hat es der Auftraggeber selbst — *„wenn man auf reaktion klickt"*. **Ein Klick
ist genau die Stelle, an der ein Lesegate hingehört.**

**Verbleib:** Rollenkarte DEV, Vertreter `platform/tests/test_widget_raster.py` (14
Zusicherungen) und `platform/tests/js/widget_raster.test.cjs` (7), `SWR-210`.


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

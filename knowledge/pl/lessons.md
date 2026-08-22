
---

## L-2026-08-21dp — Eine Verschiebung, die nur im Bericht steht, zündet erst am nächsten `--beginne`

**Beobachtung:** Bis eine Zusicherung diese Lehr-ID zitiert, wird sie ausdrücklich als Beobachtung geführt — gebuchte Entscheidung des Host-Laufs vom 22.08. (Auftraggeber, `platform/T-0070`); der Wortlaut bleibt unverändert stehen und wird mit seinem Vertreter bindend.

*Anlass: Folgelauf 2026-08-21 (kein Sprint 36, Shell ausgefallen). Gefunden beim Lesen,
nicht von einer Prüfung.*

Der Abschluss von Sprint 35 hat **neun Tickets in Prosa nach Sprint 36 verschoben** und
dazu „Grund je im Ticket" zugesichert. Am Bestand nachgezählt: **kein einziges Ticket
trägt die Verschiebung** — weder im Feld `geplant_sprint` noch als Notiz. Alle neun stehen
auf **35**, dazu `platform/T-0060` (`in_review`): **zehn**.

**Warum das monatelang still bleiben kann:** `sprint_vergangen` (SWR-112) vergleicht
`geplant_sprint` gegen `sprint_register.aktuell()` — die Nummer des **zuletzt begonnenen**
Sprints. Solange 35 der laufende ist, ist `35 < 35` falsch, und die Prüfung schweigt
vollkommen zu Recht.

> **Regel: Eine Verschiebung ist erst verschoben, wenn das TICKET sie trägt. Bericht und
> Plan sind Leser, nicht Quelle. Und die Nachziehung gehört VOR `--beende`/`--beginne`,
> nicht hinter die Planung des Folgesprints — sonst schlägt der ganze Rückstand in dem
> Moment auf, in dem der neue Sprint entsteht.**

⚠ **Präzise gezählt (Korrektur einer eigenen Formulierung dieses Laufs):** der Preflight
meldet **einen** Befund, der zehn Tickets namentlich nennt (`befunde += 1`), nicht zehn
Befunde. Für den Exit-Code und damit für den Schnelltakt ist das gleichwertig; für jede
Zählung ist es das nicht.

⚠ **Und die Auslösung braucht zwei Schritte:** solange die letzte Registerzeile kein
`ende` trägt, verweigert `beginne()` nach SWR-136 und nennt den laufenden Sprint. Der
Befund entsteht also nach `--beende` **und** `--beginne` — genau in der Reihenfolge, die
die „Erste Aufgabe des Folgelaufs" bis zu dieser Lehre vorschrieb.

---

## L-2026-08-21dq — Ohne `git` ist die Ticketdatei tabu, der Rest des Hauses frei

**Beobachtung:** Bis eine Zusicherung diese Lehr-ID zitiert, wird sie ausdrücklich als Beobachtung geführt — gebuchte Entscheidung des Host-Laufs vom 22.08. (Auftraggeber, `platform/T-0070`); der Wortlaut bleibt unverändert stehen und wird mit seinem Vertreter bindend. ⚠ Inhaltlich inzwischen BERICHTIGT durch `NOTBETRIEB-OHNE-SHELL.md`: seit Schritt [0/6] von `abschluss.cmd` verbucht der Host liegengebliebene Arbeitskopien — das Tabu gilt nur noch, solange der Host-Takt nicht läuft.

*Anlass: derselbe Lauf. Die Lehre ist die scharfe Grenze zu `L-2026-08-21db`.*

`L-2026-08-21db` sagt, bei Werkzeugausfall sei die nützlichste Arbeit die Teilmenge der
Blockade, die das Werkzeug ohnehin nicht gelöst hätte. Sie sagt **was**, nicht **wo** —
und ohne das **Wo** ist sie eine Haltung statt einer Regel.

Das Wo steht nachlesbar in `preflight.ist_verifikationsquelle`: drei Sorten Datei liest
eine Verifikation — `BOARD.md`, `*/requirements/**/software-requirements.md` und **jede**
Datei unter `*/tickets/`. Eine geänderte, **nicht committete** Verifikationsquelle **ist**
ein Preflight-Befund.

> **Regel: In einem Lauf ohne `git` wird keine Verifikationsquelle angefasst. Zehn Tickets
> richtigzustellen hieße, einen neuen Blocker anzulegen, um einen künftigen zu vermeiden.
> Erzähl-, Plan- und Lehrdateien sind frei — und dort gehört die Arbeit dann hin.**

⚠ **Zwei Einschränkungen, die das Gegenlesen gefunden hat und die die Regel nicht
aufheben, aber schärfen:**

1. `pm/management/sprint-aktuell.md` ist zwar keine Verifikationsquelle, wird aber von
   `plandrift`, `statusdrift` und `plannachlauf` **gelesen** — und
   `test_berichtskennzahlen` vergleicht ihren Kennzahlenblock gegen `kennzahlen.miss()`.
   **Wer sie ohne Shell anfasst, lässt Plantabelle und Kennzahlenblock in Ruhe.**
   `plan_tabelle` schneidet ausschließlich die **erste** Tabelle nach der
   Plan-Überschrift; jede weitere Tabelle der Datei ist für den Plan unsichtbar.
2. Dateien in der **Arbeitswurzel** (`PROJEKTSTATUS-UPDATE.md`, `PUSH-ANFORDERUNG.txt`)
   sind aus einem **anderen** Grund unbedenklich: die Wurzel ist kein Git-Repo, sie
   erscheinen in keinem `git status`. Die richtige Antwort aus dem falschen Grund ist
   keine Messung.

⚠ **Nebenbefund über das Gegenlesen selbst:** die unabhängige Prüfung meldete diese ID als
„bereits vergeben" — sie las die Dateien, die **derselbe Lauf gerade geschrieben hatte**.

> **Eine Prüfung, die gegen die Arbeitskopie läuft, kann „vergeben" nicht von „soeben von
> dir vergeben" unterscheiden. Sie prüft den Stand, nicht die Herkunft — dieselbe Familie
> wie „Anwesenheit ist nicht Verwendung".**

⚠ Verbleib: `process/roles/pl.md` (Punkte 11–12), `pm/docs/historie.md`.

### ⚠⚠ NACHTRAG 2026-08-21 (vierter Lauf ohne Shell): BEIDE Hälften dieser Regel sind widerlegt — am Bestand, nicht in der Meinung

Diese Lehre ist keine Woche alt und in beiden Hälften falsch. Sie steht hier
**unverändert samt Nachtrag** und wird nicht umgeschrieben: eine Lehre, die man glättet,
verliert den Grund, aus dem man sie geglaubt hat.

**Erste Hälfte — „die Ticketdatei ist tabu".** Die Begründung war, eine unverbuchte
Verifikationsquelle bleibe als Preflight-Befund liegen. Am **selben Tag** hat der Host
`abschluss.cmd` um den Schritt **[0/6] Nachverbuchung** ergänzt: `git add -A` + Commit in
**jedem** Repo, **vor** dem Preflight, alle 15 Minuten durch den Wächter.

| Nachgemessen (21.08.) | Wert |
|---|---|
| Repos mit unverbuchter Arbeitskopie um 21:15 | **0 von 18** (`ollama-schnelltakt.log`) |
| CM-Review 21:14, Prüfpunkt „Nachverbuchung" | **OK — nichts liegengeblieben** |
| Abschlussläufe des Hosts am 21.08. | **8 und wachsend** (20:44 – 22:25) |

⚠⚠ **Und das Beunruhigende ist nicht der Irrtum, sondern wo er steht:**
`NOTBETRIEB-OHNE-SHELL.md` trägt die Warnung *„Ticketdateien sind ohne git TABU"* und
**neun Zeilen tiefer** die Tabelle *„Was der Host inzwischen von allein auffängt"* mit
genau der Zeile, die ihr den Grund nimmt. Beide Absätze stammen aus **einem** Lauf,
aus **einer** Datei, von **einem** Autor.

⚠⚠ **Und auch diese Berichtigung war beim ersten Schreiben zu bequem — gefunden im
selben Lauf, im selben Protokoll, das sie zitiert.** „Der Verbucher macht die
Ticketdatei unbedenklich" stimmt nicht; er macht sie **vorübergehend** bedenklich.
Gemessen an den vier Tickets, die dieser Lauf angelegt hat:

| Schnelltakt-Lauf | Befund | Tick |
|---|---|---|
| **21:55** | `UNVERBUCHT tickets/T-0068.md · T-0069.md · T-0070.md` | **abgebrochen** |
| **22:10** | `UNVERBUCHT tickets/T-0070.md`, `T-0086.md` | **abgebrochen** |
| **22:25** | `UNVERBUCHT tickets/T-0069.md`, `T-0070.md`, `T-0086.md` | **abgebrochen** |
| dazu | `test_berichtskennzahlen` **neu rot** (`abschluss-...-215500.log:10025`, `[('tickets_offen', 34, 38), ('wartet_auf_mensch', 0, 1)]`) | |

⚠⚠ **Die erste Fassung dieser Tabelle schrieb „eine Minute später geheilt" und nannte
einen einzigen Lauf. Nachgezählt sind es DREI Schnelltakt-Läufe in Folge** — weil jede
**Korrektur** an einem Ticket eine neue unverbuchte Fassung erzeugt und der Verbucher im
15-Minuten-Takt hinterherläuft.

> **⚠⚠ Der Lauf, der den Ollama-Nachweis LEGEN wollte, hat den Ollama-Takt drei Takte
> lang blockiert — mit den Ticketdateien, die den Nachweis tragen sollen.**

> **Regel (ersetzt die obige): Eine Schutzregel nennt die Bedingung, unter der sie gilt,
> und die Messung, mit der man sie prüft. „Ohne `git` tabu" nennt keine — deshalb hat sie
> zwei Läufe lang gegolten, obwohl sie schon nicht mehr galt. Richtig ist: eine
> Verifikationsquelle darf ein Lauf ohne `git` anfassen, SOLANGE ein Verbucher hinter ihm
> läuft — aber jedes Schreiben kostet einen Takt, und jede KORREKTUR kostet noch einen.
> Wer eine Ticketdatei ohne `git` anfasst, schreibt sie EINMAL fertig, misst den Verbucher
> (`abschluss-logs/review-*.md`) und nennt den Preis. Ein Ticket im Lauf mehrfach zu
> überarbeiten ist ohne `git` teurer als das Ticket wert ist.**

**Zweite Hälfte — „Erzähl-, Plan- und Lehrdateien sind frei".** Diese Hälfte war die
Erlaubnis, auf die sich zwei Läufe berufen haben. Sie ist teurer als die erste:

**Acht** Lehren (`L-2026-08-21dj`…`dq`, diese eingeschlossen) tragen eine ausformulierte
`**Regel:**` und werden von **keiner** Zusicherung zitiert.
`test_keine_NEUE_lehre_ohne_vertreter`, `test_erweitern_und_weglassen_sind_dasselbe_ergebnis`
und `test_ohne_vertreter_ist_wieder_genau_die_basis` sind genau daran rot (Host-Lauf
21:14). Behandlung: `platform/T-0070`.

⚠⚠ **Die Erstfassung dieses Nachtrags hat daraus eine falsche Geschichte gemacht, und
das Gegenlesen hat sie im selben Lauf zerlegt.** Sie schrieb: *„Die zwei Läufe, die sich
das Bauen versagt haben, haben mit dem Einzigen, was sie sich erlaubt haben, drei
Zusicherungen rot gemacht."* Nachgezählt: **sechs der acht stammen aus Sprint 35** — einem
Lauf **mit** Shell, der gebaut hat (`SWR-213`). Nur `dp` und `dq` stammen aus Läufen ohne
Shell. **Die drei Tests waren am Ende von Sprint 35 bereits rot**; die zwei Läufe haben
den Befund um zwei Namen **vergrößert**, nicht erzeugt.

> **Regel: Wer beim Aufschreiben eines Befunds die Schuldigen zusammenfasst, hat aufgehört
> zu zählen. „Alle acht" war eine bessere Geschichte als „sechs und zwei" — und die
> bessere Geschichte hätte die eigentliche Ursache verdeckt: nicht der Ausfall der Shell,
> sondern ein Sprint MIT Shell, der 1551 Tests als Verifikation führte und 1136 davon
> nicht ausführte.**

⚠ Was von der zweiten Hälfte trotzdem bleibt: die zwei Läufe konnten ihren eigenen
Beitrag **nicht merken** — Merken hätte die Prüfstrecke gebraucht. Das ist der Kern und
er trägt auch ohne die Übertreibung.

> **Regel: „Frei" heißt nicht „folgenlos". Ein Lauf, der eine Datei anfasst, deren Wirkung
> er nicht messen kann, hat nicht vorsichtig gehandelt, sondern blind — und die Blindheit
> ist genau dann am größten, wenn sie sich Zurückhaltung nennt. Wer ohne Prüfstrecke eine
> Lehre schreibt, legt sie als `**Beobachtung:**` an oder lässt sie liegen.**

⚠ Behandlung der acht: `platform/T-0070` (je Lehre einzeln entschieden, Basis **nicht**
nachgezogen). Betriebsmittelfrage: `pm/T-0086` (Klasse A). Verbleib dieses Nachtrags:
`NOTBETRIEB-OHNE-SHELL.md` ist im selben Lauf berichtigt worden.

---

## L-2026-08-21dj — Ein Sprint-Abschluss kann den Nachweis blockieren, den derselbe Sprint für unmöglich erklärt

**Beobachtung:** Bis eine Zusicherung diese Lehr-ID zitiert, wird sie ausdrücklich als Beobachtung geführt — gebuchte Entscheidung des Host-Laufs vom 22.08. (Auftraggeber, `platform/T-0070`); der Wortlaut bleibt unverändert stehen und wird mit seinem Vertreter bindend.

*Anlass: `platform/T-0060`, Sprint 35 (2026-08-21).*

Vier Sprints lang trug die Frage „warum läuft Ollama nie?" vier verschiedene Antworten.
Sprint 34 schloss, der Nachweis sei „aus dieser Sandbox nicht führbar", und belegte das
sauber: `localhost:11434` tot, `host.docker.internal` von der Allowlist gesperrt.

**Beides stimmt und beides misst den falschen Rechner.** Der Takt läuft per
`ollama-schnelltakt.cmd` auf dem Rechner des Auftraggebers — **87 Läufe, 138 Abbrüche,
0 Erfolge** —, und sein Protokoll lag unangetastet im Arbeitsordner.

Alle 138 Abbrüche tragen wörtlich dieselbe Zeile: *„Preflight hat Befunde"*. Die zwei
Befunde hat **Sprint 34 selbst erzeugt** — vier Planzeilen mit der alten Sprintnummer und
neun Tickets ohne Sprint aus der eigenen Projektgründung.

> **Regel: Bevor eine Diagnose „nicht führbar" lautet, wird gemessen, WO der Gegenstand
> läuft. Und ein Preflight-Befund am Sprintende ist keine Fußnote — er ist eine Sperre für
> jeden Automatiklauf bis zum nächsten Sprint.**

Der Preis war nicht die falsche Zahl, sondern die Richtung: eine Diagnose über die eigene
Umgebung erzeugt Arbeit an der eigenen Umgebung, und der Gegenstand bleibt unberührt.

---

## L-2026-08-21dk — Die vierte Berührung zählt Terminierungen, nicht Verschiebungen

**Beobachtung:** Bis eine Zusicherung diese Lehr-ID zitiert, wird sie ausdrücklich als Beobachtung geführt — gebuchte Entscheidung des Host-Laufs vom 22.08. (Auftraggeber, `platform/T-0070`); der Wortlaut bleibt unverändert stehen und wird mit seinem Vertreter bindend.

*Anlass: Sprint-Planung 35 (2026-08-21).*

Der Plan von Sprint 34 schrieb: *„Alle sind zweite Verschiebungen; die Regel der vierten
Berührung ist nicht berührt."* Nachgezählt tragen sechs Tickets in Sprint 35 ihre
**dritte Terminierung** — `platform/T-0055`, `T-0060`, `pm/T-0080`, `T-0082`,
`team-dashboard/T-0004`, `team-mail/T-0006`.

`pl.md` Regel 2 spricht von *„zum vierten Mal terminiert"*. Verschiebungen und
Terminierungen sind verschiedene Zählungen: ein Ticket, das im Sprint seiner Entstehung
terminiert wird, hat eine Terminierung mehr als Verschiebungen.

> **Regel: Die Zählung der vierten Berührung steht VOR der Arbeit im Plan, nicht danach im
> Abschluss — und sie zählt Terminierungen. Wer die bequemere der beiden Zählungen nimmt,
> verschiebt die Entscheidung um genau einen Sprint.**

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

## L-2026-08-20bh — Eine Regel kann formal erfüllt und in der Sache verfehlt sein; die Prüfung, die sie erzwingt, merkt davon nichts

**Anlass (Sprint 21, Brief `pm/N-0043` Punkt 3).** Der Auftraggeber verlangte, dass eine
Aufgabe beim Start auf `in_progress` geht. Die Regel **gibt es seit Sprint 1**:
`board.UEBERGAENGE` erzwingt `open → in_progress → in_review → done`, und ein Sprung
darüber hinweg ist seit SWR-118 ein Prüfbefund. Die bequeme Antwort wäre „haben wir
schon" gewesen, und sie wäre belegbar richtig gewesen.

Gemessen über die committete Historie, **320 Ticketdateien**:

| | |
|---|---|
| geschlossene Aufgaben, die **nie** `in_progress` waren | **159** |
| Median-Aufenthalt in `in_progress` bei den übrigen 141 | **22 Sekunden** |
| Maximum über den gesamten Bestand | 30 Minuten |

> **Der Status wurde gesetzt, weil die Prüfung ihn verlangt, und nicht, weil er jemandem
> etwas sagt. Die Übergangsprüfung kennt die REIHENFOLGE der Zustände und nicht ihre
> DAUER — der Mensch, der auf die Anzeige sieht, prüft genau umgekehrt.**

⚠ Und daraus folgte unmittelbar der zweite Befund: Punkt 4 desselben Briefes („am
Sprintende steht kein `in_progress` mehr") war **bereits erfüllt** — nicht durch
Disziplin, sondern weil der Zustand 22 Sekunden lebt. *Eine Prüfung, die auf einem
Bestand grün ist, in dem der geprüfte Zustand gar nicht vorkommt, prüft nichts.*

**Regel:** Wenn ein Wunsch des Auftraggebers eine Regel beschreibt, die es schon gibt,
wird vor dem Antworten **nachgemessen, ob sie wirkt** — nicht, ob sie dasteht. Und eine
neue Prüfung auf einen Zustand wird erst gebaut, nachdem belegt ist, dass der Zustand im
Bestand überhaupt vorkommt.

## L-2026-08-20bk — Wir messen, ob ein Lauf sauber endete, nicht ob der nächste jemals anfing — der ausgefallene Lauf ist der einzige, der sich nicht selbst melden kann

**Anlass (Nachtrag zu Sprint 21, Brief `team-mail/N-0004`).** Der Auftraggeber meldet, die
Routine sei zwei Tage nicht gelaufen, *„aber im aktuellen sprint müsste das aufgefallen
sein!?"* Gemessen aus dem eigenen Register: die Pause zwischen dem Ende von Sprint 20 und
dem Start von Sprint 21 betrug **3 612 Minuten = 60,2 Stunden** bei einem hinterlegten
Takt von **60 Minuten** — das **Sechzigfache**. Sprint 21 hat es nicht gemeldet.

`sprint_register.nicht_beendete()` prüft Sprints **ohne `ende`**, also Läufe, die
mittendrin abbrachen. Das ist eine Prüfung auf eine **Spur**.

> **Ein Lauf, der ausfällt, hinterlässt keine Spur. Alle unsere Prüfungen sehen Spuren —
> also sehen sie alles außer dem Ausfall selbst.**

⚠ Verschärfend: **derselbe Lauf hat an genau dieser Stelle gearbeitet.** SWR-153 hat der
Kachel „Letzte Session" den Zeitpunkt und die Sprintnummer des letzten Laufs gegeben —
und niemand hat gefragt, warum dieser Zeitpunkt zweieinhalb Tage zurücklag. Die Angabe
existierte, die Frage nicht.

⚠ Und `session.stille()` (SWR-102) beantwortet die Frage seit Sprint 16 — aber nur
**gegenüber der Ansicht**. *Eine Messung, die nur in einer Kachel steht, existiert für
jeden Ablauf nicht.*

**Regel:** Zu jeder Prüfung „ist X sauber beendet worden?" gehört die Gegenfrage „hat X
überhaupt stattgefunden?". Und eine Messung, die eine Ansicht anstellt, gehört an die
Stelle, die den Ablauf steuert — sonst ist sie für den Ablauf nicht vorhanden. Als
`platform/T-0025` verbucht.

## L-2026-08-20bo — Eine Messung, die nur eine Schwelle nicht bestimmen kann, hat trotzdem alles geleistet

*Anlass: Sprint 22, `platform/T-0025` Frage 1 (*ab wann ist eine Pause ein Befund?*).*

Das Ticket verlangte ausdrücklich: **erst am Bestand messen, welche Pausen normal sind**,
weil eine geratene Schwelle entweder ein Daueralarm wird oder bei 60 Stunden schweigt.
Gemessen wurde, und das Ergebnis war in Vielfachen des Takts: −0,35 · 0,0 · 0,1 · 0,23 ·
0,7 · 0,93 · **60,2**.

Zwischen der größten unauffälligen Pause und der einen auffälligen liegen fast zwei
Zehnerpotenzen. **Jede** Zahl dazwischen trennt die Daten gleich gut — die Messung sagt
also, wo die Grenze *nicht* liegen darf, und lässt offen, wo sie liegt.

**Regel 1 — das ist kein Fehlschlag der Messung.** Sechs Werte hätten jede daran
angepasste Grenze zu einer gefitteten gemacht. Der Unterschied zwischen „geraten" und
„gefittet" ist für den, der die Prüfung später liest, keiner.

**Regel 2 — wenn die Daten die Wahl nicht treffen, trifft sie der vorhandene
Regelbestand und nicht die Meinung des Laufs.** Gewählt ist `STILLE_TAKTE = 2` —
**dieselbe** Zahl, mit der die Kachel seit SWR-102 Stille meldet. Damit ist die Schwelle
begründbar, ohne neu zu sein, und Kachel und Preflight sagen über dieselbe Stille
dasselbe.

**Regel 3 — die Messung war trotzdem der Ertrag des Tickets.** Sie hat zwei Befunde
geliefert, nach denen niemand gefragt hatte (`L-2026-08-20bm`, `L-2026-08-20bn`), und
drei der vier Vorabfragen beantwortet. Eine Messung rechtfertigt sich nicht an der
Frage, die sie beantworten sollte.

**Regel 4 — „der Fall kommt nicht vor" ist eine vollwertige Antwort.** Frage 4 des
Tickets (*zählt ein abgebrochener Lauf als gelaufen?*) endete bei **0 von 22**. Nach der
eigenen Auflage wurde **keine Regel erfunden**. Eine Regel für einen Fall, den es nicht
gibt, ist eine Prüfung, die nie etwas prüft — dieselbe Familie wie `L-2026-08-17ai`, nur
vor dem Bauen bemerkt statt danach.

## L-2026-08-20bv — Bei der vierten Berührung gibt es drei ehrliche Ausgänge, und „noch ein Termin" ist keiner davon

*Anlass: Sprint 23 — `projects/p11/T-0013` (gebaut) und `promt-team/T-0008` (blockiert).*

Zwei Tickets standen in diesem Lauf bei der vierten Berührung. Sie sind **verschieden**
ausgegangen, und der Unterschied ist die eigentliche Lehre.

`p11/T-0013` wartete auf **Kapazität** — dreimal richtig verschoben, weil ein
PIN-Lesegate eine Zugriffsentscheidung ist und nicht nebenbei erledigt gehört. Der
vierte Ausgang war: **bauen**.

`promt-team/T-0008` wartete nicht auf Kapazität. Gemessen: die Run-Registry führt
**4 Läufe für `dev`, 3 für `cm` und 0 für die übrigen zehn Rollen** — unverändert seit
Sprint 18, fünf Sprints ohne einen einzigen neuen. Der vierte Ausgang war: **`blocked`
mit `blocked_by`**, und die Vorbedingung bekam ein eigenes Ticket.

> **Ein Ticket viermal auf „Kapazität" zu terminieren, während es in Wahrheit auf eine
> Tatsache wartet, die kein Lauf herstellt, indem er das Ticket anfasst, heißt den Grund
> viermal falsch aufzuschreiben.**

**Regel 1 — vor dem vierten Termin steht die Frage, WORAUF gewartet wird**, und die
Antwort ist messbar: Kapazität, ein Mensch, eine Umgebung oder eine Tatsache.

**Regel 2 — die drei ehrlichen Ausgänge sind bauen, zerlegen/blockieren, schneiden.**
Schneiden ist kein Scheitern; `promt-team/T-0010` führt es ausdrücklich als eine von drei
Optionen.

**Regel 3 — ein Verschiebungsgrund mit Verfallsdatum darf nicht dreimal wortgleich
dastehen.** Wo er das tut, ist er kein Grund mehr, sondern eine Gewohnheit.

## L-2026-08-20bw — Ein eigener Buchungsfehler ist die billigste Sonde, die man je bekommt

*Anlass: Sprint 23 — `projects/p11/T-0009` sprang `in_progress → done`.*

Dieser Lauf hat einen Statuswechsel falsch gebucht: der Schritt `in_review` fehlt, und
`board.UEBERGAENGE` verbietet den Sprung seit Sprint 1. Der Fehler steht in der Historie
und bleibt dort — die Historie wird nicht umgeschrieben.

Die Frage danach war nicht *„wie mache ich es weg"*, sondern **„warum ist dabei nichts
rot geworden?"** — und die Antwort war ungleich größer als der Fehler: die
Übergangsprüfung hat `projects/` seit **Sprint 9** nie angesehen (SWR-162).

> **Ein eigener Fehler an einer geprüften Stelle ist ein kostenloser Test der Prüfung.
> Wer ihn schnell wegräumt, bezahlt mit der Antwort auf die einzige interessante Frage.**

**Regel 1 — nach jedem selbstverschuldeten Verstoß gegen eine geprüfte Regel wird
zuerst nachgesehen, ob die Prüfung ihn gemeldet hat.** Zweiter Fall dieser Art in diesem
Haus: Sprint 15 hat einen Buchungsfehler benannt, Sprint 16 die Vorkehrung daraus gebaut.

**Regel 2 — nicht glätten.** Der Verstoß ist ab jetzt der **vierte** stehende Befund des
Preflights und der einzige, der diesem Lauf gehört. Er steht im Ticket, im Sprintplan und
im Bericht.

---

## L-2026-08-20ca — Ein Rückbau, der während seiner Ausführung wächst, ist genau der Rückbau, der etwas kaputt macht

**Anlass (Sprint 24, `projects/p11/T-0015`).** Der Rückbau der Kachelhälfte hatte eine
enge DoD: Route, Funktion, Konstante, Teststrecke, Anforderung. Beim Bauen fiel der Rand
auf — `regeln.js` trägt die Frontend-Hälfte derselben Anzeige und hat seit SWR-148 keinen
Aufrufer: **4 Bausteine, 11 von 111 JS-Zusicherungen**, gemessen.

Er ist **gezählt und aufgemacht** worden (`p11/T-0016`) und **nicht mitgenommen**.

**Regel 1 — der Grund, aus dem ein Rückbau dreimal verschoben wurde, gilt auch für seinen
eigenen Rand.** *Ein Rückbau liefert nichts und kann alles kaputt machen.* Wer ihn während
der Ausführung erweitert, hebt genau die Sorgfalt auf, wegen der er so lange gewartet hat.

**Regel 2 — der Beleg lag im selben Lauf.** `zustaende_von` sah aus wie Dashboard-Code und
trägt eine fremde, abgenommene Anforderung. `feldText` steht eine Datei weiter in derselben
Lage: zwei lebende Funktionen verweisen in ihren Docstrings **namentlich** auf sie als
Vorbild. Ein gelöschtes Vorbild macht zwei Kommentare unlesbar.

**Regel 3 — „nicht mitgenommen" muss aktenkundig werden, sonst ist es von „übersehen"
nicht zu unterscheiden.** Deshalb die Zählung im Ticket und ein eigenes Ticket, nicht ein
Satz im Abschlussbericht.

---

## L-2026-08-20cb — Eine Messgrundlage, die ihr eigener Anlass ist, misst sich selbst

**Anlass (Sprint 24, `promt-team/T-0010`, Entscheidung Klasse C).** Zehn von zwölf Rollen
haben **null** aufgezeichnete Läufe, unverändert seit Sprint 18. Zur Wahl standen: (a) je
Sprint eine Rolle für eine ohnehin anfallende Aufgabe aufrufen, (b) Übungsläufe, (c) die
Rollen ungemessen lassen und das Goldset-Ticket schneiden.

(a) und (b) fallen an derselben Stelle: beide erzeugen einen Lauf **um der Messung willen**.

> **Ein Goldset folgt dem Betrieb. Es geht nicht voran und es wird nicht nachgeholt.**

**Regel 1 — (a) sieht harmloser aus als (b) und ist es nicht.** „Die Aufgabe fällt ohnehin
an" stimmt für die Aufgabe und **nicht für die Rollenwahl**: wer die Rolle danach aussucht,
dass ihr ein Lauf fehlt, hat den Lauf wegen der Messung erzeugt.

**Regel 2 — ein ersatzloser Schnitt beantwortet eine Frage dauerhaft, die sich ändern
kann.** Richtig ist deshalb nicht „schneiden", sondern **umdrehen**: nicht *„zehn Rollen
vermessen"*, sondern *„bemerken, wenn eine Rolle vermessbar wird"*.

**Regel 3 — eine so umgedrehte Aufgabe braucht ihre Prüfung im selben Zug.** Eine Regel
ohne Vertreter hält keine drei Sprints (SWR-125, gemessen an den 14 zurückgekehrten
Kalenderdaten). Deshalb steht die Prüfung *„welche Rolle hat Läufe und kein Goldset?"* als
DoD-Punkt und nicht als Absicht.

---

## L-2026-08-20ch — Bei der fünften Berührung war der Grund nie Kapazität (Sprint 25)

`p12/T-0011` ist seit dem 17.08. **fünfmal** terminiert worden, jedes Mal mit „Kapazität",
und jedes Mal stimmte das. Sprint 24 hat im Plan festgehalten, die Regel der vierten
Berührung sei hier bereits **überzogen**.

Beim Hinsehen in Sprint 25 war der eigentliche Grund ein anderer:

> **In der Aufgabe steckte eine Entscheidung, die niemandem vorgelegt worden ist — und der
> einzige, der sie treffen kann, ist der, der die Ansichten liest.**

Was tatsächlich fällig war, war kleiner als der Bau und wurde in einem Lauf erledigt: die
Prüfung, die den Folgepunkt vertritt, maß **wie viele** Rohtext-Ansichten es gibt und nicht
**welche**. Ein Tausch — eine umgestellt, anderswo eine neue angelegt — wäre grün geblieben.

**Regel 1 — ein Ticket, das dreimal am selben Punkt hängenbleibt, enthält eine ungestellte
Frage.** Vor der vierten Terminierung wird gesucht, wer sie beantworten muss.

**Regel 2 — beim Umschneiden fragen: was davon ist HEUTE fällig und braucht niemanden?**
Hier war es die Zusicherung, nicht der Bau.

**Regel 3 — „gemessen" und „benannt" sind zwei Zusicherungen.** Eine Zahl ohne ihren
Gegenstand kann Stillstand nicht von Tausch unterscheiden (dieselbe Bewegung wie
`L-2026-08-20by`, eine Ebene höher).

---

## L-2026-08-20cl — Fünfmal terminiert, einmal gefragt, fünf Minuten Antwortzeit (Sprint 26)

**Der Fall.** `p12/T-0012` ist die Frage, die fünf Sprints lang als **Aufgabe** im Plan
stand und jedes Mal mit „Kapazität" verschoben wurde. Sprint 25 hat sie als Frage erkannt
und in die Inbox gestellt, Frist 27.08., Default A. Der Auftraggeber hat am **20.08. um
20:34** geantwortet — **fünf Minuten** nach Beginn des nächsten Sprints, sieben Stunden
nach dem Stellen der Frage.

> **Der Aufwand lag nie im Beantworten. Er lag darin, die Frage überhaupt als Frage zu
> erkennen — und fünf Läufe lang hat niemand hingesehen, weil ein Ticket mit einem Termin
> aussieht wie Arbeit.**

**Regel 1 — eine Aufgabe, die dreimal am selben Punkt hängenbleibt, enthält eine Frage.**
Das stand schon in `L-2026-08-20ch`. Neu ist die **Messung des Preises**: fünf
Verschiebungen kosteten sieben Stunden Wartezeit auf eine Antwort, die in fünf Minuten kam.

**Regel 2 — die Antwort kann teurer aussehen, als sie ist.** Der Einwand gegen das Fragen
war, es erhöhe den Zähler „auf dich wartende Entscheidungen". Er tat es für sieben Stunden.

**Regel 3 — „A = Ist-Zustand" heißt auch: kein Nachfolgeticket.** Die DoD sah eines für den
Bau-Fall vor. Bei A gibt es keinen Bau; ein Ticket „nichts tun" wäre ein Vorgang ohne
Gegenstand. **Geschlossen ist geschlossen.**

---

## L-2026-08-21cm — Ein Stellvertreter wird zum Loch, sobald die Sache einen eigenen Namen bekommt

**Sprint 30, `platform/T-0051`.** Drei gesperrte Tickets meldete der Preflight als *„offen
auf vergangenem Sprint"*. Nach dem Leeren des Termins meldete eine **andere** Prüfung
dieselben drei als *„Ticket ohne Sprint"*.

> **Für ein gesperrtes Ticket gibt es keinen zulässigen Terminwert. Der einzige Wert, der
> beide Prüfungen still hält, ist eine Terminzusage über fremdes Handeln — also die
> falsche Handlung. Eine Lage, in der die bequeme Handlung die einzige ist, die grün
> macht, ist die Bauart, gegen die `SWR-166` gebaut wurde.**

⚠ Beide Prüfungen sind **einzeln richtig** und gut begründet. Der Fehler liegt **zwischen**
ihnen: die Ausnahme steht an einem **Typ** (`decision-request`) statt an einem **Zustand**
(*„das Team kann hier nicht handeln"*). Bis Sprint 29 war `decision-request` der einzige
Weg, diese Lage auszudrücken — der Typ fiel mit der Sache zusammen. `SWR-193` hat der
Sache mit `blocked`/`blocked_by` einen eigenen Namen gegeben, und in dem Moment wurde aus
dem Stellvertreter ein Loch.

⚠ **Und es stellt die Vorgänger-Session richtig:** der Termin war in Sprint 29 nicht aus
Nachlässigkeit stehengeblieben. Es gab nichts Besseres. `L-2026-08-21cc` hielt fest, dass
*eine Begründung, die mit der einzigen möglichen Handlung zusammenfällt, von einer
Rationalisierung nicht zu unterscheiden ist*; **hier gibt es überhaupt keine mögliche
Handlung**, und das ist die schärfere Lage.

**Regel:** Bekommt ein Zustand einen eigenen Namen, werden alle Ausnahmen geprüft, die
bisher einen **Typ** nennen — sie meinten möglicherweise den Zustand. Und wo zwei
Prüfungen zusammen einen Zustand ohne zulässigen Wert erzeugen, wird die Zange als solche
gemeldet, nicht der bequeme Wert eingetragen.

## L-2026-08-21cs

**Regel:** „Briefkasten zuerst" ist eine **Reihenfolge** und keine Zusicherung. Ein
Zustand, der einmal am Anfang gemessen und am Ende als Ergebnis berichtet wird, ist eine
Momentaufnahme in der Aufmachung einer Garantie — der Briefkasten wird deshalb **am Ende
erneut** gemessen, bevor „0 offen" im Bericht steht.

Sprint 32 sichtete den Briefkasten als erstes: **0 offen** über 60 Briefe, richtig
gemessen. Beim Zusammenstellen der Abschlusszahlen meldete `kennzahlen.py` **7** — alle
sieben zwischen **06:32 und 07:03** eingegangen, also **nach** dem Durchgang. Der Haken
hinter „erfüllt" war beim Setzen wahr und zwei Stunden später falsch.

⚠⚠ Dieselbe Familie wie `platform/T-0052` aus **demselben** Sprint, in entgegengesetzter
Richtung: dort war der Bestand *während* des Laufs widersprüchlich und wurde nur am **Ende**
gemessen; hier ist er *während* des Laufs gewachsen und wurde nur am **Anfang** gemessen.
**Zweimal derselbe Messzeitpunkt-Fehler in einem Sprint.**

⚠ Zweiter Befund im selben Vorgang: der Handlauf suchte nur `*/management/briefkasten/`,
das Werkzeug liest zusätzlich `*/*/management/briefkasten/`. **Zwei Discovery-Wege mit
verschieden weiten Grundmengen** — heute hat das nichts verdeckt, aber das ist Glück und
keine Eigenschaft.

**Verbleib:** Rollenkarte PL (`process/roles/pl.md`), Ticket `platform/T-0057`
(Vertreter wird dort gebaut — Häufigkeit zuerst gezählt, dann gebaut).

## L-2026-08-21cv

**Regel:** `in_progress` wird gesetzt und **committet**, wenn die Arbeit beginnt. Ein
Status, der erst zusammen mit `done` in denselben Commit geht, existiert für die
Übergangsprüfung nicht — sie liest **Commits**, nicht Arbeitsspeicher.

Sprint 32 hat `platform/T-0052` und `T-0053` bearbeitet, sie erst am Ende auf
`in_progress` gesetzt und dann zusammen mit `done` committet. Aus Sicht von Git ist das
`open -> done` — ein **unzulässiger Übergang**, viermal (dazu `pm/T-0077` und
`p13/T-0001` beim Verbuchen).

> **⚠⚠ Der Bericht dieses Laufs hat den Fehler selbst BESCHRIEBEN — „die Tickets standen
> bis kurz vor dem Schließen auf `open`" — und ihn im selben Atemzug BEGANGEN. Ein Fehler,
> den man aufschreibt, ist damit nicht behoben; er ist nur belegt.**

⚠ Gefunden hat es `test_uebergang_historie`, eine Zusicherung aus Sprint 23 — die **dritte**
fremde Zusicherung in zwei Sprints, die den eigenen Entwurf verworfen hat. Repariert wird
nichts: Historie wird hier nicht umgeschrieben (Kap. 16). Die vier stehen ab jetzt
namentlich in der Liste der fortgeschriebenen Übergänge, die damit von **4 auf 8** wächst
— und die Hälfte davon gehört diesem Lauf.

**Verbleib:** Rollenkarte PL (`process/roles/pl.md`, Lehre 1 — sie stand dort bereits und
hat nicht getragen; ergänzt um das Wort **committet**), Vertreter
`platform/tests/test_uebergang_historie.py::test_im_laufenden_sprint_gibt_es_keinen_verstoss`.


---

## L-2026-08-22a — Ein Nachweis, den eine Maschine im Takt führt, ist erst geführt, wenn ihn jemand ABHOLT

*Anlass: Sprint 36, `platform/T-0060`.*

Der erfolgreiche Ollama-Tick lag **seit dem 2026-08-21 20:59** in
`platform/management/runs/run-registry.jsonl` — `status: ok`, `modell: ollama/gemma3:27b`,
zwei geschriebene Artefakte, 221,2 s. Das Ticket, dessen DoD genau **einen** solchen Lauf
verlangt, blieb rund **16 Stunden und mehrere Läufe** auf `in_review`.

Drei Sprints hatten zuvor die *Erreichbarkeit* von Ollama diskutiert. Der Beweis ist
nicht durch Argumentieren entstanden, sondern durch den Takt — und wäre ohne diesen Lauf
weiter liegen geblieben.

> **Ein Haus, das seine Maschinen arbeiten lässt, muss ihre Ergebnisse zur Bringschuld des
> Sprint-Anfangs machen. Sonst produziert es Belege, die niemand einlöst.**

**Regel:** Der Sprintanfang liest **zuerst** die Belege, die seit dem letzten Lauf von
allein entstanden sind — Run-Registry, Decision-Log, Takt-Protokolle —, **bevor** er plant.

**Verbleib:** Rollenkarte PL (`process/roles/pl.md`, Lehre 13), Vertreter
`platform/tests/test_dr_verbuchung.py::test_kein_entschiedener_dr_ist_unverbucht`
(dieselbe Familie: ein maschinell erzeugter Stand, den niemand abholt) sowie
`platform/docs/historie.md` (2026-08-22).

---

## L-2026-08-22c — Eine Entscheidung des Menschen ist erst angekommen, wenn ein TICKET sie trägt

*Anlass: Sprint 36, `pm/T-0086` / `pm/D030`.*

Der Auftraggeber entschied am 2026-08-22 um **00:23** (`pm/D030` = C). Die Zeile stand im
Decision-Log; das Ticket blieb `open` und zählte **13 Stunden** in `wartet_auf_mensch`
mit. **Der DR wartete in der Buchführung auf jemanden, der längst geantwortet hatte** —
und `wartet_auf_mensch` ist genau die Zahl, mit der dieses Haus dem Menschen sagt, woran
er dran ist.

Gefunden hat es die Zusicherung `test_dr_verbuchung`, die für exakt diesen Fall gebaut
wurde (SWR-131, zweite Hälfte) — und die **vier Sprints lang nicht gefahren** worden ist.

> **Zwei Buchführungen über dieselbe Tatsache driften immer; die Frage ist nur, wer es
> merkt. Hier war es keine Rolle, sondern ein Test.**

**Regel:** Das Decision-Log gehört zum **Briefkasten-Schritt** (Schritt 1 der Session),
nicht zum Berichtsschritt. Eine entschiedene Klasse-A-Frage wird im selben Lauf verbucht,
in dem sie gelesen wird — inklusive Entsperren der `blocked_by`-Nachbarn.

**Verbleib:** Rollenkarte PL (`process/roles/pl.md`, Lehre 14), Vertreter
`platform/tests/test_dr_verbuchung.py::test_kein_entschiedener_dr_ist_unverbucht`,
`pm/docs/historie.md` (2026-08-22).

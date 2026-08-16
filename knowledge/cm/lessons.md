# Lessons Learned — Rolle CM

*Kuratiert vom COACH (T-0016, Prozess-CR Retro Sprint 1). Quelle: Betriebsdaten des ersten autonomen Ticks (2026-08-06). Regeln: knowledge/README.md.*

**L-001 (2026-08-06, T-0013):** Artefakt-Pfade in Datei-Blöcken sind immer **relativ zur Repo-Wurzel** anzugeben — nie mit Repo-Präfix. Falsch: `process/cm/datei.md` (ergibt `process/process/cm/datei.md`), richtig: `cm/datei.md`. Das Gateway strippt bekannte Präfixe (Schutznetz), aber der Prompt muss es von vornherein richtig vorgeben.

**L-002 (2026-08-06, T-0014):** Ein Tick darf nur auf **sauberer Arbeitskopie** starten, und Ergebnis-Commits enthalten ausschließlich die eigenen Artefakte (selektives `git add`, nie `add -A`). Der Misch-Commit aus Sprint 1 (Session-Arbeit + Tick-Ergebnis unter einer Ticket-ID) hat die Traceability genau eines Commits geschwächt — Ursache im Orchestrator behoben (Precondition + selektives Add).

**L-003 (2026-08-06, T-0011/F13):** Provider-Realität je Gerät dokumentieren, bevor eine Kette geplant wird: Der guardrails-Default `llama3.1:8b` war auf dem Team-Node nicht installiert (vorhanden: `gemma3:27b`, Override per `OLLAMA_MODEL`). Modell-Defaults gegen das Geräteregister prüfen; Abweichungen als Registry-/Guardrails-CR nachziehen.

**L-004 (2026-08-06, T-0010/T-0018):** Schwache/lokale Modelle liefern **generische** Inhalte, wenn der Kontext die projektspezifische Realität nicht enthält: Die erste CM-Strategie erfand Rollen (RE, TECHWRITER, QA) und Pfade (`project/`, `backend/`). Der Aufgaben-Kontext muss die reale Artefakt-Landkarte (Playbook Kap. 3), die Rollen aus `roles/registry.yaml` und die real existierenden Repos benennen — und das Ergebnis ist dagegen zu reviewen.

## L-2026-08-16: cmd-Klammern-Bug ERNEUT (abschluss.cmd :repo_push)

Wiederholung der P1-Lehre: `echo`-Text mit `(`/`)` innerhalb eines `if (...)`-Blocks bricht cmd mit Syntaxfehler ab — diesmal die `.kein-remote`-Meldung, Folge: ALLE Auto-Pushes seit 2026-08-15 17:36 schlugen fehl (pm/N-0006). Regel verschärft: **In .cmd-Dateien NIE Klammern in echo-Zeilen** — auch nicht escaped, einfach weglassen. Erkennungsmuster: PUSH-ANFORDERUNG.txt bleibt liegen + abschluss-auto.log endet mit „kann syntaktisch an dieser Stelle nicht verarbeitet werden".

## L-2026-08-16b (B049): Ein Grund, der einmal stimmte, wird nie wieder geprüft — und deckt oft nur die Hälfte

Drei Tickets (`pm/T-0010`, `T-0013`, `T-0026`) trugen den Vermerk „Nachweis unerreichbar, kein
`gh`/Netzzugriff". Der Satz war beim Schreiben richtig und stand danach **sieben Session-Fußzeilen
lang wortgleich** da. Zwei Dinge waren daran falsch:

1. **Der Grund war seit Stunden weg.** Der Auto-Push-Wächter lief seit 10:30 wieder durch (13
   grüne Läufe, alle Repos `ahead 0`). Niemand hat den Grund nachgeprüft, weil er endgültig klang.
2. **Der Grund deckte nur eines von zwei Kriterien.** `T-0013` verlangt (a) `platform` als erstes
   Repo in der Push-Ausgabe und (b) grüne Actions. **(a) ist rein lokal prüfbar** — es steht in
   `abschluss-auto.log`, seit **07:59:59**, in jedem Lauf. Der Vermerk, der den Nachweis für
   unerreichbar erklärte, nannte selbst „letzter Push 08:30" — dieser Lauf trug den Beleg bereits.

**Regeln daraus:**

- **Ein Hinderungsgrund im Ticket ist ein Messwert mit Datum, keine Eigenschaft.** Wer ihn beim
  nächsten Aufgreifen wiederholt, prüft ihn vorher nach — sonst schreibt man eine Beobachtung von
  gestern als Zustand von heute (B038-Familie).
- **Verifikationskriterien einzeln abhaken, nie als Satz.** Sobald ein Ticket mehrere Kriterien
  hat, gilt „geht nicht" höchstens für einzelne davon. Ein gemeinsamer Hinderungssatz verdeckt die
  Kriterien, die man sofort erledigen könnte.
- **Ein Ticket ohne Frist ist für die Eskalationsregel unsichtbar** (`board.ist_ueberfaellig`
  braucht eine Frist). Der `unterminiert`-Zähler ist der einzige Melder — und er ist eine Zahl je
  Kachel, keine Summe. „Kachel X abgearbeitet" ist keine gültige Abschlussmeldung (CR `pm/T-0036`).

## L-2026-08-16c (B050): Belegpflicht ist eine Anforderung an die Nachvollziehbarkeit, nicht an die erste Seite

Der Auftraggeber meldete: *„du schreibst zu viel text … es kostet mich zu viel zeit alles
durchzulesen."* Zu Recht. Die Dokumente sind gewachsen, weil B025/B038 verlangen, nichts zu
behaupten, was man nicht zeigen kann — richtig, aber daraus wurde **Beweiskette vor Ergebnis**.

**Regel:** Ergebnis oben (max. fünf Zeilen, ohne Ticketnummern-Kauderwelsch), Beleg darunter.
Beides ist erfüllbar; es war nie ein Zielkonflikt, sondern eine Reihenfolge.

**Ausnahme, ausdrücklich:** Klasse-A-Entscheidungsanträge bleiben vollständig. Ein Antrag, der
Gegenargumente kürzt, ist kein kürzerer Antrag, sondern ein schlechterer — er bekommt den
Kurzblock obenauf, nicht statt des Textes.

## L-2026-08-16d (R7): Ein Umgehungsweg um eine Sperre darf den Zustand nicht raten

Der `.git/index.lock` auf dem Mount lässt sich anlegen, aber nicht löschen — jeder folgende
`git`-Aufruf bricht ab. Der naheliegende Ausweg, `GIT_INDEX_FILE` auf eine **Kopie** des Index zu
setzen, läuft durch und ist trotzdem falsch: Die echte `.git/index` bleibt auf altem Stand, und
**jede Datei, die nicht ausdrücklich im `git add` steht, wird aus diesem alten Stand
mitcommittet**. In der Praxis hat das ein Ticket um 26 Zeilen zurückgesetzt, während die
Commit-Message das Gegenteil behauptete.

**Richtig:** Locks per `mv` nach `.git/verwaiste-locks/` wegräumen (Umbenennen ist auf dem Mount
erlaubt, Löschen nicht — derselbe Ausweg wie in `pm/T-0023`), danach `git reset` gegen den echten
Index und normale Aufrufe.

**Regel:** Ein Workaround, der eine Sperre umgeht, muss denselben Zustand lesen wie das gesperrte
Werkzeug — sonst tauscht er einen sichtbaren Fehlschlag gegen einen unsichtbaren (B038). Und:
Diffstat-Zahlen nach dem Commit gegenlesen, nicht nur den Exit-Code (B041 Regel 3) — die Zahl
`-26` war hier der einzige Hinweis.

## L-2026-08-16e (B051): Eine Konvention, die nur von Hand existiert, überlebt den ersten Werkzeuglauf nicht

**P12 ist das erste Projekt, das über den „Starten"-Knopf entstanden ist** (`pm/T-0022` Teil 2,
`pool.kandidat_starten`). Der Lauf war technisch fehlerfrei — und hinterließ zwei Dinge, die eine
Session von Hand richten musste:

1. **Der Pool-Kandidat wurde gelöscht statt nach „Realisiert" verschoben.** Den Abschnitt gibt es
   seit 16:15 desselben Tages, eingeführt von Hand für Kandidat #13 mit der Begründung aus B029.
   Das Werkzeug war da schon gebaut und kennt ihn nicht.
2. **Das erzeugte Decision-Log hat keinen Tabellenkopf.** Die Inbox hängt ihre Entscheidungszeile
   an — ohne Kopf ist das keine Tabelle, sondern Pipe-Text. Alle von Hand angelegten Logs (P10,
   P11) tragen den Kopf; nur der vom Werkzeug erzeugte nicht.

**Beides scheitert lautlos.** Ein gelöschter Kandidat wirft keinen Fehler, eine kopflose Tabelle
auch nicht. Sichtbar wurde es nur, weil die Session den Commit des Knopfes gegengelesen hat
(`1 file changed, 1 deletion(-)` — B041 Regel 3).

**Regel:** Wird eine Konvention von Hand eingeführt, während ein Werkzeug denselben Weg
automatisiert, gehört sie in derselben Sitzung als Ticket ans Werkzeug — sonst hält sie genau so
lange, wie niemand den Knopf drückt. Das ist die Umkehrung von B033: dort waren es zwei Kopien
einer Regel, hier ist es eine Regel, die nur einen der beiden Wege kennt.

**Zweite Regel, aus dem Umgang damit:** Was ohne Code geht, wird sofort von Hand angewandt (Zeile
nachgetragen, Kopf ergänzt); die Werkzeugänderung wird eingeplant (`pm/T-0037`) und nicht nebenbei
in einer Routine-Session gebaut (B025/B038).

## L-2026-08-16f (B052): Der Status-Übergang wird gegen HEAD geprüft, nicht gegen den vorigen Aufruf

**Beim Schließen von `pm/T-0037`** wurde die vorgeschriebene Kette
`open → in_progress → in_review → done` in **drei** `board.py status`-Aufrufen ohne
Zwischencommit gefahren — so, wie „über die erlaubten Übergänge geschlossen" in sieben
Session-Fußzeilen steht. Der anschließende `board.py pm --check` meldete
`unzulässiger Status-Übergang: open -> done`.

**Der Grund steht in `board.validiere`:** Verglichen wird der Status der Arbeitskopie gegen
`status_in_head(repo, datei)` — also gegen den **zuletzt committeten** Stand. Für die Prüfung
existieren die Zwischenschritte nicht; drei Aufrufe hintereinander sind ein einziger Sprung.

**Das ist kein Werkzeugfehler, sondern die richtige Prüfung an der einzigen Stelle, die sie
belegen kann:** die Historie. Ein Ticket, dessen Zwischenschritte nur in der Arbeitskopie
stattgefunden haben, hat sie nicht nachweisbar durchlaufen — genau die Behauptung, gegen die
B038 geschrieben ist.

**Regel:** Ein Status-Übergang ist erst vollzogen, wenn er committet ist. Wer eine Kette fährt,
committet **je Übergang** — mit einer Commit-Botschaft, die sagt, was den Übergang rechtfertigt
(`-> in_review: DoD vollständig`, `-> done: Tests grün`). Ein Sammelcommit am Sessionende ist
für Ticketstatus kein gültiger Abschluss.

**Nebenbefund (R7, unverändert):** Jeder `git status`/`git add` hinterlässt auf diesem Mount ein
`.git/index.lock`, das er nicht löschen kann — bei einer Commit-Kette also **mehrfach**, und beim
`commit` zusätzlich ein `HEAD.lock`. Die Locks vor **jedem** Git-Aufruf per `mv` nach
`.git/verwaiste-locks/` wegräumen, nicht nur einmal am Anfang; und `*.lock` prüfen, nicht nur
`index.lock`.

## L-2026-08-16g (B053): Ein Feld beantwortet die Frage, für die es gebaut wurde — nicht die, die man ihm stellt

**Der Brief `pm/N-0030` fragte, wer an einem offenen Ticket arbeitet.** Das generierte `BOARD.md`
hat genau eine Zuordnungsspalte, `rolle`, und die sieht wie eine Antwort aus. Sie ist keine:
`rolle` ist die **Fachrolle des Autors** — `board.py` benutzt sie als solche
(*„reviewer darf nicht der Autor (rolle) sein"*). Vier offene Tickets (`pm/T-0034`, `T-0013`,
`T-0010`, `T-0026`) sind ausschließlich am Host durch den Menschen lösbar und tragen trotzdem
`prob` bzw. `cm`; im Board sehen sie aus wie Teamaufgaben.

**Die fehlende Auflösung existierte die ganze Zeit.** `process/roles/registry.yaml` trägt je Rolle
`besetzung: ki | mensch | script`. Gelesen wird das Feld von **keiner** Ausgabe — nicht vom Board,
nicht vom Cockpit, nicht vom Preflight. Der Unterschied Mensch/KI war vollständig hinterlegt und
nirgends sichtbar; die eigentliche Information stand im **Fließtext** der vier Tickets
(*„eine Handlung des Auftraggebers"*, *„Voraussetzung beim Menschen"*) — Muster **B043**.

**Die Abkürzung wäre ein stiller Schaden gewesen.** Naheliegend war, den vier Tickets
`rolle: mensch` zu geben. Dieses Feld hat in `board.py` aber eine **zweite** Bedeutung: Tickets
mit `rolle: mensch` sind Gates und von der Status-Übergangsprüfung ausgenommen
(`t.get("rolle") != "mensch"` in `validiere`). Die Umstellung hätte vier Tickets die
Übergangsprüfung abgeschaltet, ohne eine einzige Meldung.

**Regel:** Bevor ein vorhandenes Feld für eine neue Frage benutzt wird, wird geprüft, **wer es
sonst noch liest und wozu**. Trägt es bereits eine Bedeutung, die Verhalten steuert, bekommt die
neue Frage ein **eigenes** Feld (`pm/T-0038`) — ein Feld für zwei Zwecke dient am Ende nur einem,
und welchem, entscheidet der Aufrufer (Familie **B033**).

**Zweite Regel:** Eine Zuordnung, die nur im Fließtext steht, existiert für jede Übersicht nicht.
Sie muss in einem Feld stehen — und das Feld muss von der Ausgabe gelesen werden, auf die
tatsächlich jemand schaut.

---

## L-2026-08-16h — Ein Test, der nur die eigene Fassung kennt, prüft nichts (B054)

**Fund.** `briefkasten._parse` trennte die Team-Antwort von der Nachricht an einer wörtlichen
Überschrift: `## Antwort (Team, JJJJ-MM-TT)`. Der zugehörige Test schrieb genau diese Zeile selbst
in die Testdatei und war seit P4 grün. Die **Routine-Sessions** schreiben seit dem 15.08. eine
andere Fassung — `## Antwort des Teams (Routine-Session, JJJJ-MM-TT HH:MM)`, mit Uhrzeit, weil bei
einem 30-Minuten-Takt das Datum nicht mehr unterscheidet. Ergebnis: bei **10 von 30** beantworteten
Briefen blieb `antwort` leer, und die vollständige Team-Antwort stand im **Nachrichtenblock**. Die
Chat-Ansicht rendert den Antwortblock nur, wenn `b.antwort` gefüllt ist — sie zeigte Frage und
Antwort als **einen** Text ohne Absender und ohne Datum. Betroffen war auch `pm/N-0030`.

**Warum es niemand gemerkt hat.** Der Fehler hat keine Meldung, kein rotes Gate und keinen
Stacktrace. Er sieht nur falsch aus, und zwar nur dort, wo niemand aus dem Team hinschaut: in der
HMI des Auftraggebers. Der Preflight zählt Briefe nach `status`, nicht nach Lesbarkeit; die
SWR-Matrix meldete SWR-050 als verifiziert — durch einen Test, der seine eigene Eingabe erzeugt.

**Regel 1 — Testdaten aus dem Bestand, nicht aus der Fantasie.** Wo ein Parser ein Format liest,
das **andere** Teile des Systems schreiben, muss mindestens ein Testfall die Fassung benutzen, die
tatsächlich im Repo steht. Der billigste Vollzug: einmal über die echten Dateien laufen und zählen,
was herausfällt (`beantwortet ohne erkannte Antwort` — hier: 10).

**Regel 2 — auf die Überschrift prüfen, nicht auf ihre Fassung.** Wer eine Struktur erkennt, soll
das erkennen, was sie zur Struktur macht (`^## Antwort…`), und den Rest (Zusatz, Uhrzeit, Klammern)
als variabel behandeln. Ein exaktes Muster gegen einen Text, den Menschen und Sessions schreiben,
ist eine Zusicherung, die niemand gegeben hat.

**Regel 3 — der Gegenprobentest muss inhaltlich scheitern.** Ein neuer Test, der gegen den
Altstand nur mit `AttributeError` scheitert (die Funktion gab es noch nicht), belegt nichts über
den Schaden. Erst der Test über den echten Lesepfad — *die Team-Antwort darf nicht in `nachricht`
stehen* — scheitert mit `AssertionError` und benennt damit, was kaputt war.

## L-2026-08-16i — Eine Zahl, die aus der Historie kommt, ist noch keine Messung dessen, wonach gefragt war (B056)

**Woher.** `pm/T-0040` (Kachel „Letzte Session" in Mission Control) verlangt in seiner DoD „die
Zahl der **Sessions** des Tages (aus der Git-Historie, nicht aus dem Text)". Die Historie kennt
aber keine Sessions, sie kennt **Commits**. Am 16.08. stehen **42 Commits** auf
`pm/management/session-agenda.md` rund **30** Routine-Läufen gegenüber: eine Session schreibt die
Datei mehrfach.

**Die naheliegende Brücke trägt nicht.** Commits über eine Zeitlücke zu Sessions zu bündeln — „was
weniger als ein Takt auseinanderliegt, ist eine Session" — sieht sauber aus und unterschätzt
nachweislich: zwischen `16:35:24` und `16:51:41` liegen **16 Minuten** und **zwei verschiedene**
Sessions. Die Kachel hätte dann „12 Sessions heute" gesagt, wo 14 liefen, ohne jede Meldung.

**Regel.** Wenn die verfügbare Größe die gefragte nur annähert, wird die **verfügbare** geliefert
und sie wird nach sich selbst benannt (`fortschreibungen_heute`, nicht `sessions_heute`). Eine
Schätzung unter dem Namen einer Messung ist B027/B038 — der stille Ausfall, den niemand bemerkt,
weil er plausibel aussieht. Wer die gefragte Größe wirklich braucht, braucht eine **Markierung, die
die Session selbst setzt**; das ist ein eigener CR und wird nicht nebenbei erfunden.

**Zweiter Teil derselben Session, dieselbe Familie.** Ein Aufruf in der falschen CLI-Form
(`board.py pm --status T-0040=in_progress` statt `board.py pm status T-0040 in_progress`) hat
klaglos **BOARD.md neu erzeugt** und `OK: 40 Tickets validiert` gemeldet — der Statuswechsel fand
nicht statt, und die bereits geschriebene Commit-Botschaft behauptete ihn trotzdem. Gefunden nur
beim Nachlesen von `grep '^status:'`, nicht von einer Meldung. **Eine Commit-Botschaft ist eine
Aussage über den Zustand und wird wie eine Aussage geprüft**, bevor sie stehenbleibt; hier
richtiggestellt (`--amend`) und die drei Übergänge mit je einem Commit neu gefahren (B052).

## L-2026-08-16j — Ein Zähler, der zwei Fragen in eine Zahl faltet, verliert eine davon (B057)

**Woher.** `pm/T-0016` (Sprint-Workflow-Sicht, SWR-103) zählt die Zeilen des Sprint-Plans nach
Zustand: *dieser Sprint*, *terminiert*, *wartet auf Mensch*. Die erste Fassung las den Zustand
ausschließlich aus der Spalte **Fällig** — eine Zeile hatte genau einen Zustand, die Zerlegung war
sauber, alle 22 Tests grün.

**Der Widerspruch stand in derselben Datei.** Beim ersten Lauf gegen den **echten** Plan meldete
der Zähler `wartet_auf_mensch = 1`, während der Klartext oben in derselben Datei *„5 warten auf
eine Handlung am Host"* sagte. Ursache ist kein Tippfehler, sondern ein Denkfehler: **Termin und
Zuständigkeit sind zwei Fakten.** `pm/T-0034` trägt ein Datum (17.08.) **und** wartet auf den
Host. Wer beides in *einen* Zustand faltet, muss eine der beiden Aussagen wegwerfen — und wirft
die weg, die nicht in der gelesenen Spalte stand.

**Regel 1 — vor der Zerlegung die Frage zählen, nicht die Spalte.** Eine überschneidungsfreie
Zerlegung ist nur dann richtig, wenn die Dinge sich wirklich ausschließen. Merkmale, die
**quer** liegen (wartet auf jemanden, ist blockiert, ist wiederkehrend), bekommen eine **eigene**
Zahl, die sich mit den anderen überschneiden **darf** — und heißen nach dem, was sie zählen. Das
ist dieselbe Familie wie **B053** (`rolle` mit zwei Bedeutungen) und **B033** (zwei Quellen für
eine Aussage), nur in Zahlenform.

**Regel 2 — der Test, der das gefunden hätte, gibt es nicht; der Lauf gegen den Bestand schon.**
Alle Tests der ersten Fassung waren grün, weil die Testdaten dieselbe Annahme trugen wie der Code.
Gefunden hat es der erste Lauf gegen die **echte** Datei — wörtlich Regel 1 aus **L-2026-08-16h**.
Ein neuer Zähler wird deshalb **immer** einmal gegen den Bestand gefahren und seine Zahlen gegen
den **Klartext derselben Quelle** gelesen; stimmen sie nicht überein, ist zuerst der Zähler
verdächtig, nicht der Text.

## L-2026-08-16k — Eine Regel, die nur terminierte Vorgänge sieht, macht unterminierte unsichtbar — auch die eigenen (B057)

**Woher.** Das Sprint-Planning nach `pm/D006` sichtete zum ersten Mal **alle** Tickets **aller**
Repos in einem Durchgang. Dabei fiel `pm/T-0016` auf: `typ: change-request`, `prio: hoch`, ohne
Frist, ohne `takt` — und in **keiner** der drei Agendalisten („Für dich", „Für das Team",
Takt-Dauerläufer). Es war das **einzige** unterminierte Ticket der Organisation
(`cockpit_alle`: `unterminiert = 1`).

**Der vierte Auftritt desselben Musters.** B049 hat es beschrieben: `board.ist_ueberfaellig`
setzt eine Frist voraus, ohne Frist ist die Ampel grau, und ein Ticket ohne Frist kann **nie**
überfällig werden. Die Regel gegen das Liegenbleiben hat ihren blinden Fleck genau dort, wo es
stattfindet. Einziger Melder ist eine **Zahl je Kachel**, die niemand als Summe liest —
`pm/T-0036` (Frist 23.08.) soll das beheben und ist selbst noch offen.

**Die Pointe, die es zur Lehre macht:** Getroffen hat es den CR, der die Workflow-Sicht **liefern
soll**. Der Vorgang, der die Unsichtbarkeit beheben sollte, war selbst unsichtbar.

**Regel — jede Sicht auf einen Plan meldet, was im Plan fehlt.** `sprint.plan()` vergleicht die
handgeschriebene Plantabelle mit dem echten Bestand aller entdeckten Repos und weist jedes offene
Ticket aus, das in **keiner** Planzeile vorkommt (`nicht_geplant`) — mit Ref und Titel, **über**
der Tabelle, nicht darunter. Eine Sicht, die nur wiedergibt, was ihr vorgelegt wird, kann einen
fehlenden Eintrag grundsätzlich nicht finden; sie bestätigt nur die Lücke, die sie zeigen sollte.
Das gilt über diesen Fall hinaus für jede Ansicht, deren Quelle von Hand gepflegt wird.

## L-2026-08-16l — Eine Regel, die auf Tagen rechnet, kann nichts über Uhrzeiten sagen — auch nicht schweigend (B058)

**Woher.** `pm/T-0032` Teil 2 sollte den Uhrzeit-Takt bauen (`takt: taeglich@14:00`). Teil 1 hatte
schriftlich entschieden: der abgeleitete Termin geht durch **dieselbe** `board.frist_ampel` wie
eine Frist — *„eine Ampelregel, zwei Quellen"*, eine zweite Rechnung wäre B033. Genau das wurde
gebaut, und es war beim ersten Test falsch.

**Der Fehler.** `frist_ampel` verglich **Kalendertage** (`frist < heute` → rot). Der abgeleitete
Termin „**heute** 14:00" ist um 15:00 **verstrichen**, der **Tag** aber nicht. Die geteilte Regel
hätte deshalb **gelb — „fällig, aber noch Zeit"** gesagt, ausgerechnet für den Takt, der versäumt
wurde. Kein Tippfehler: die Funktion beantwortete korrekt die Frage, die sie kannte („ist der Tag
vorbei?"), und wurde nach etwas anderem gefragt („ist der **Moment** vorbei?"). Dieselbe Familie
wie **B057** (zwei Fakten in eine Zahl) und **B053** (ein Feld, zwei Bedeutungen).

**Regel 1 — eine Regel teilen heißt prüfen, ob ihre Auflösung zur neuen Frage passt.** Bevor eine
bestehende Funktion eine zweite Quelle bekommt, wird ihre **Granularität** gegen die neue Frage
gehalten: Tage gegen Minuten, Datum gegen Moment, Zähler gegen Menge. Passt sie nicht, ist die
Wahl nicht „Kopie oder falsche Antwort", sondern **die geteilte Regel wird verfeinert** — und die
alte Aussage wird dabei bewiesen, nicht behauptet: der Test vergleicht die neue Fassung mit der
alten Formel **Tag für Tag über einen ganzen Monat gegen jeden Bezugstag desselben Monats**
(961 Vergleiche). Erst dann ist es eine Erweiterung und keine Bedeutungsverschiebung.

**Regel 2 — wo Auflösung fehlt, gilt die vorsichtige Richtung, und sie steht im Code.** Wo nur ein
Tag bekannt ist, aber ein Moment gebraucht wird, gilt der Termin als **verstrichen** und eine
Erledigung als **frühestmöglich** (Datum ohne Uhrzeit = Tagesbeginn, Termin ohne Uhrzeit =
Tagesende). Beide Richtungen zeigen auf **„im Zweifel fällig, nie frisch"** — dieselbe
Vorsichtsregel wie bei `session.stille` (B038). Was aus Vorsicht gilt und nicht aus Messung, wird
im Docstring als solches benannt.

**Regel 3 — Vorsicht darf nicht zur Falschmeldung werden.** Die naheliegende Abkürzung wäre
gewesen, den Tagesbezug des Cockpits einfach als Moment zu behandeln (Tagesende). Das hätte
`taeglich@23:00` **jeden Morgen** als fällig gemeldet — Vorsicht, die zur Fehlmeldung wird, ist
keine mehr, sondern erzieht zum Wegsehen. `aggregation.cockpit` führt deshalb **zwei** Bezüge
(`heute` für Tagesarithmetik, `jetzt` für Momente) statt eines überladenen. Der Prüfsatz: *Welche
Zahl meldet diese Vorsicht, wenn niemand etwas falsch gemacht hat?* Lautet die Antwort „Alarm",
ist die Vorsicht an der falschen Stelle.

## L-2026-08-16m — Wer eine geteilte Regel erweitert, muss ihre Nachbarn mitziehen — und die eigene Suite merkt es nicht (B059)

**Woher.** Unmittelbar nach dem Commit zu SWR-104 (Uhrzeit-Takt, L-2026-08-16l) prüfte eine
**unabhängige Gegenprüfung** die Änderung. Sie fand zwei echte Defekte. **Alle 400 Tests waren
grün.**

**Was passiert war.** `frist_ampel` liest eine Frist seit SWR-104 über `als_moment` und akzeptiert
damit auch `2026-08-15 14:00`. Zwei Nachbarn lasen denselben Wert weiter mit der **alten**
Auflösung:

1. `aggregation.cockpit` filterte über `ist_ueberfaellig` (neu, akzeptiert Uhrzeit) und rechnete
   die Tage-über daneben mit `date.fromisoformat` (alt). Ergebnis: `ValueError` — **erst nachdem
   der Termin abgelaufen war** (vorher greift der Filter nicht), und weil `cockpit_alle` über alle
   Projekte läuft, riss **ein** Ticket in **einem** Repo die **ganze** Cockpit-Seite mit, nach
   außen als irreführendes `HTTP 404 „unbekanntes Projekt"`.
2. Der Ticket-Editor baut sein Auswahlfeld aus einem festen Vokabular. Der neue Wert stand nicht
   darin — der Browser wäre auf „einmalig" zurückgefallen, und das Speichern eines **beliebigen
   anderen Feldes** hätte den Takt **stillschweigend gelöscht**.

**Warum die Suite schwieg.** Die neuen Tests prüften die **neue** Fähigkeit (Takt mit Uhrzeit) und
die **alte** Zusage (Datumsfristen unverändert). Keiner prüfte die **Kreuzung**: einen *alten*
Feldnamen mit einem *neuen* Wert. Genau dort liegt der Schaden — und dort schaut niemand hin, der
die Änderung geschrieben hat, weil er weiß, wofür er sie gemeint hat.

**Regel 1 — die Erweiterung einer geteilten Regel ist erst fertig, wenn ihre Nachbarn geprüft
sind.** Wird der **Wertebereich** einer geteilten Funktion vergrößert (Datum → Moment, Zahl →
Bereich, ein Feld → Liste), wird jeder Aufrufer **desselben Rohwerts** aufgesucht und gefragt:
*liest er ihn durch dieselbe Funktion, oder hat er seine eigene Kopie der alten Auflösung?* Eigene
Kopien werden ersetzt, nicht ergänzt. Das ist die Anschlussfrage an B033 („wo steht die Regel
schon?"): **wer liest denselben Wert noch, und mit welcher Annahme?**

**Regel 2 — für jeden neuen Wertebereich ein Test am alten Feld.** Neben dem Test für die neue
Fähigkeit gehört einer für den **alten** Namen mit dem **neuen** Wert (hier: `frist` *mit* Uhrzeit,
obwohl niemand das vorhatte). Der Prüfsatz: *Welchen Wert kann dieses Feld ab heute tragen, den es
gestern nicht tragen konnte — und wer liest dieses Feld?*

**Regel 3 — eine grüne Suite ist kein Ersatz für einen fremden Blick, und der kommt zuletzt.**
Die Defekte fand niemand, der den Code kannte. Bei Änderungen an **geteilten** Regeln gehört nach
dem Commit eine unabhängige Gegenprüfung dazu, die ausdrücklich nach *Nachbarn und Grenzfällen*
sucht statt nach der Absicht. Sie wird nicht als Zusatz geführt, sondern als **letzter Schritt der
Änderung** — vorher ist sie nicht abgeschlossen.

## L-2026-08-17a — Wählt der Auftraggeber die Option mit der bekannten Schwäche, wird sie umgesetzt **und** abgesichert (B060)

**Woher.** Bei der Umstellung von Datums- auf Sprintplanung (`pm/T-0041`) stand die Frage, ob das
Datumsfeld `frist` verschwindet. Das Team hat drei Möglichkeiten vorgelegt und die dritte —
**beide Felder parallel** — ausdrücklich als die schwächste bezeichnet: zwei Angaben zu „wann ist
es dran" driften auseinander (B033). Der Auftraggeber hat genau diese gewählt.

**Was daraus nicht folgt.** Weder „dann eben, er wird schon wissen, was er tut" noch „wir machen
es trotzdem anders". Das erste liefert wissentlich eine Konstruktion mit eingebautem Fehler; das
zweite überstimmt eine Entscheidung, die nicht dem Team gehört.

**Regel 1 — die Schwäche wird zur Prüfung, nicht zur Fußnote.** Wenn eine gewählte Option eine
benannte Schwachstelle hat, wird **die Schwachstelle geprüft**, nicht vorausgesetzt. Hier: `frist`
und `geplant_sprint` beantworten ab jetzt schriftlich **zwei verschiedene Fragen** (Zusage nach
außen / Lauf des Teams) — damit sind es zwei Fakten und keine zwei Quellen —, und
`board.sprint_widerspruch` meldet jedes Ticket, dessen geplanter Sprint **auch im günstigsten
Fall** nach seiner Frist läge. Zwei Fakten dürfen nebeneinander stehen; zwei Angaben, die sich
widersprechen, nicht. Der Prüfsatz: *Woran würde man merken, dass die Schwäche eingetreten ist —
und wer schaut dort hin?*

**Regel 2 — der Melder zeigt nur, was schon der günstigste Fall nicht auflöst.** Die Prüfung
rechnet mit **ununterbrochenem** Takt. Ein Plan, der nur bei Stillstand reißt, ist ein Risiko und
kein Widerspruch; ein Melder, der Risiken als Fehler ausgibt, erzieht innerhalb weniger Läufe zum
Wegsehen. Dieselbe Regel wie bei der Takt-Vorsicht aus **L-2026-08-16l**, Regel 3: *Welche Zahl
meldet dieser Melder, wenn niemand etwas falsch gemacht hat?*

**Regel 3 — die Entscheidung und ihr Einwand stehen im Ticket, nicht im Gedächtnis.** `pm/T-0041`
nennt beide: die Wahl des Auftraggebers und den Einwand des Teams samt Gegenmaßnahme. Wer in drei
Monaten auf die Doppelung stößt, findet dort, dass sie gewollt war und was sie absichert — statt
sie als Versehen zu „reparieren".

**Anmerkung zur zweiten Wahl desselben Vorgangs (B025).** Der Auftraggeber hat außerdem alle vier
Flächen in einem Lauf gewählt (Feld, Zähler, Plandatei, Kachel). Auch das steht im Ticket. Die
Gegenmaßnahme war, dass **keine** der vier eine neue Regel erfindet: die Ampel bleibt
`board.frist_ampel`, die Kachel bleibt die aus SWR-103, neu ist genau eine Datei. Vier Flächen
sind beherrschbar, solange sie vier **Anwendungen** einer Regel sind und nicht vier Regeln.

# Lessons Learned — Rolle CM

*Kuratiert vom COACH (T-0016, Prozess-CR Retro Sprint 1). Quelle: Betriebsdaten des ersten autonomen Ticks (2026-08-06). Regeln: knowledge/README.md.*

## L-2026-08-17r — Eine Datei, die von sich sagt „einzige Quelle", braucht eine Prüfung, die sie beim Wort nimmt

*Anlass: B066 (Widget-Vertrag), Sprint 9 (2026-08-17).*

`widget-vertrag-v2.yaml` trägt seit Sprint 3 in Großbuchstaben den Satz: DIESE DATEI IST
DIE EINZIGE STELLE, DIE DIE FELDLISTE FÜHRT. Seit v2.1 (Sprint 7) **fehlte darin ein
Feld**: beim Einschieben von `letzte_baseline_text` über den `team`-Eintrag ging dessen
Zeile `- name: team` verloren. YAML machte daraus keinen Fehler, sondern verschmolz beide
zu **einem** Eintrag — bei doppelten Schlüsseln gewinnt der hintere. `team` kam in der
Feldliste nicht mehr vor, und `letzte_baseline_text` trug den Typ des verschluckten
Nachbarn.

**Regel 1 — der Satz „das ist die einzige Quelle" ist eine Zusicherung und gehört
geprüft.** Zwei Sprints lang war die einzige Prüfung, die dieser Vertrag kannte, dass er
**lesbar** ist. Lesbarkeit ist keine Übereinstimmung. Ein Vertrag, gegen den nichts
gehalten wird, ist eine Beschreibung — und veraltet lautlos.

**Regel 2 — geprüft wird in BEIDE Richtungen.** Ein Feld im Payload, das der Vertrag
nicht führt (der Fall hier), und ein Feld im Vertrag, das der Payload nicht liefert.
Die erste Richtung ist die gefährlichere: ein vertragstreues Widget **ignoriert
unbekannte Felder** (so steht es in der Datei), hätte `team` also stillschweigend nicht
mehr angezeigt — und der Vertrag hätte das gedeckt.

**Regel 3 — gegen einen Fehler, den der Parser schluckt, hilft der Parser nicht.**
`yaml.safe_load` meldet doppelte Schlüssel nicht, es nimmt den hinteren. Die Prüfung auf
Duplikate geht deshalb **roh über den Text**. Wer hier über das geparste Ergebnis prüft,
prüft genau die Stelle nicht, an der der Fehler entsteht.

**Regel 4 — der Wächter greift sofort, auch gegen seinen Erbauer.** Im selben Lauf, in
dem `test_vertrag_feldliste.py` entstand, meldete er zwei weitere Schlüssel im Payload,
die der Vertrag noch nicht kannte (`wartet_auf_mensch_*` aus SWR-120). Dass sie jetzt im
Vertrag stehen, ist nicht der Sorgfalt zu verdanken, sondern der Prüfung.

---

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

## L-2026-08-17b — Zwei Gates, die einander auschecken, haben keine gemeinsame Push-Reihenfolge (B061)

**Woher.** Der erste Hostlauf von SWR-105 meldete `platform` als rot, obwohl derselbe Stand lokal
grün war. Ursache, nachgestellt statt vermutet: `abschluss.cmd` pusht die Werkzeug-Repos zuerst,
also startet der CI-Lauf von `platform`, während `p0`/`p9` auf `origin/main` noch einen Commit alt
sind — und das Matrix-Gate von `platform` checkt genau diese Repos aus. Dieselbe Reihenfolge
existiert aus gutem Grund: der board-check jedes Projekt-Repos checkt `platform` aus und braucht
dort die **neue** `board.py` (`pm/T-0013`).

**Der Befund ist nicht die Reihenfolge, sondern der Zyklus.** A prüft B, B prüft A, beide werden im
selben Lauf gepusht: wer zuerst geht, sieht den anderen alt. Es gibt keine Reihenfolge, die beide
grün macht. `pm/T-0013` hat die eine Hälfte gelöst und die andere erzeugt — und niemand hat es
gemerkt, weil der Beleg fehlte, bis SWR-105 ihn geliefert hat.

**Regel 1 — bei einem Gate wird gefragt, wen es auscheckt.** Ein Gate, das fremde Repos ausleiht,
prüft nicht den Stand, für den es läuft, sondern eine Mischung aus zwei Zeitpunkten. Der Prüfsatz
vor jedem neuen Checkout in einem Workflow: *Wird dieses Repo im selben Lauf gepusht wie das
prüfende — und wenn ja, in welcher Reihenfolge?*

**Regel 2 — die Asymmetrie entscheidet, nicht die Symmetrie.** Beide Richtungen sind gleich
plausibel und **nicht** gleich häufig: das Board-Format ändert sich zweimal im Projektleben, eine
neue Anforderung entsteht in fast jedem Sprint. Die heute eingestellte Reihenfolge macht also den
Regelfall rot und den Sonderfall grün. Wer zwischen zwei unauflösbaren Übeln wählt, zählt sie.

**Regel 3 — eine Diagnose in demselben Lauf wie ihre Behebung ist eine Behebung ohne Diagnose.**
Die Wahl zwischen den Wegen ändert `abschluss.cmd` oder schwächt ein Gate; `abschluss.cmd` ist im
Vorlauf versehentlich geleert und rekonstruiert worden. Der Befund steht deshalb in `pm/T-0042`
mit vier ausgeschriebenen Wegen und ihrem jeweiligen Preis, entschieden wird im nächsten Sprint.

## L-2026-08-17c — Ein Zustand ohne Grund ist eine Farbe und verschiebt die Arbeit nur (B062)

**Woher.** `CI-STATUS.md` meldete `p3`, `p5` und `platform` als **ROT** — und beantwortete damit
die Frage, für die es gebaut worden war („sind die Läufe grün?"), ohne die Frage zu beantworten,
die danach unweigerlich kommt („was ist kaputt?"). Für zwei der drei Repos ließ sich die Ursache in
der Sandbox nicht ermitteln; der Mensch hätte doch wieder die Actions-Seite öffnen müssen — genau
das, was `platform/T-0003` abschaffen sollte.

**Regel 1 — wer einen Melder baut, baut die nächste Frage mit.** Ein Melder ist erst fertig, wenn
seine Ausgabe zu einer Handlung führt, die nicht „nachsehen gehen" heißt. Der Prüfsatz: *Was tut
der Leser als Erstes, nachdem er das gelesen hat — und kann der Melder ihm das abnehmen?* Hier war
die Antwort eine einzige zusätzliche, ebenfalls anmeldefreie Abfrage (SWR-107).

**Regel 2 — die Diagnose darf den Befund nie verschlucken.** Scheitert die Nachfrage — Budget leer,
Netzfehler, unerwartete Antwort —, bleibt das Repo **rot** und der Bericht sagt „Schritt
unbekannt". Ein Diagnoseweg, der bei eigenem Scheitern den Zustand aufweicht, ist gefährlicher als
gar keiner (B038). Ebenso zählt die Nachfrage gegen **dasselbe** Budget und erscheint in derselben
Zahl; ein zweiter, stiller Zähler wäre B033.

**Regel 3 — die erste Erklärung wird gegen einen Nachbarn geprüft, bevor sie eine Ursache heißt.**
Für `p3`/`p5` lag die Erklärung fertig da (Board-Formatänderung plus Push-Reihenfolge, `pm/T-0013`)
und war falsch: `p7` trägt denselben Commit-Zeitpunkt **auf die Sekunde**, dieselbe Workflow-Datei,
dieselbe Formatänderung — und ist grün. Gegen jede `board.py`-Fassung seit dem 16.08. sind alle
drei grün, gegen die Fassung davor alle drei rot; es gibt also keine Version, die p3/p5 rot und p7
grün macht. Der Prüfsatz: *Welcher Nachbar müsste nach dieser Erklärung dasselbe Ergebnis haben —
und hat er es?* Ohne diese Frage wären zwei Tickets mit einer plausiblen, falschen Begründung
geschlossen worden.

## L-2026-08-17d — Ein Fall, den die Anforderung ausdrücklich nennt, ist damit noch nicht gebaut (B063)

**Woher.** Die unabhängige Gegenprüfung von SWR-107 (L-2026-08-16m, zweiter Einsatz) fand bei
**grüner Suite** fünf echte Befunde. Der schwerste: eine unerwartete Nutzlast der Jobs-Abfrage —
eine Liste, wo ein Objekt erwartet wurde — ließ `fehlerschritt()` werfen, der Wurf verließ
`pruefe()`, `main()` brach ab, und **es wurde gar keine Datei geschrieben**. Der rote Befund war zu
diesem Zeitpunkt bereits vollständig ermittelt; `abschluss.cmd` schickte den Menschen mit
„Details in CI-STATUS.md" auf eine Datei mit dem Stand des Vortages („ALLES GRÜN").

**Das Besondere daran.** Der Fall stand **wörtlich in der Anforderung**: *„If the lookup fails for
any reason — exhausted budget, network error, **unexpected payload** … the repository shall remain
red."* Er war also nicht übersehen, sondern **aufgeschrieben und nicht gebaut** — und die Suite
konnte es nicht merken, weil sie prüft, was gebaut wurde. Die beiden vorhandenen Grenzfalltests
(`None`, `{"jobs": []}`) trafen ihn nicht: beide sind falsy und laufen deshalb gerade *nicht* in
den Wurf. Zwei Tests, die den Fall zu prüfen scheinen und die einzige gefährliche Variante
auslassen, sind schlimmer als keiner — sie erzeugen die Überzeugung, es sei geprüft.

**Regel 1 — die DoD-Punkte werden gegen die Tests abgehakt, nicht gegen die Erinnerung.** Nach dem
Bauen: jeden Satz der Anforderung durchgehen und zu jedem den Test benennen, der ihn hält. Wo kein
Test benennbar ist, ist der Satz nicht umgesetzt, egal wie sicher man sich ist.

**Regel 2 — bei „bei jedem Fehler" wird die Liste der Fehler ausgeschrieben.** „Scheitert die
Nachfrage" klingt vollständig und ist es nie. Netzfehler, HTTP-Fehler, leere Antwort, Antwort mit
falschem Typ, Antwort mit richtigem Typ und fehlenden Feldern — jede Zeile ein Test.

**Regel 3 — zwei Quellen entstehen auch zwischen Zahl und Text.** Ein zweiter Befund derselben
Prüfung: der Fließtext sagte viermal „Abfragebudget aufgebraucht", während das maschinenlesbare
Feld `budget_erschoepft` daneben `false` meldete. Beide beantworten dieselbe Frage — das ist B033,
auch wenn die eine Quelle Prosa ist und die andere JSON. Der Prüfsatz: *Steht dieselbe Aussage
irgendwo zweimal, und wird sie an beiden Stellen von derselben Rechnung gespeist?*

**Regel 4 — ein Ergebnis, das kein Fehlschlag ist, darf nie als Ursache erscheinen.** `neutral`
wurde wie `failure` behandelt und hätte den wirklich gescheiterten Job verschwiegen. Bei jeder
Aufzählung von „Zuständen, die zählen" gehört die Gegenliste mit ins Feld — hier `OHNE_BEFUND`,
eine Konstante statt einer Aufzählung an drei Stellen.

## L-2026-08-17e — Wechselt der Behälter, wechselt die Bedeutung jeder Frage an ihn (B064)

**Woher.** Beim Entwurf des Widget-Vertrags (`team-dashboard/T-0001`) wurde jedes Feld der
Cockpit-Aggregation gegen den **echten** Payload aller 16 Projekte und Teams gehalten. Dabei fiel
auf: `p11` und `p12` trugen als „letzte Baseline" den Tag **`p10-v1.0`** — die Baseline eines
fremden, abgeschlossenen Projekts. Kein fehlender Wert, ein **falscher**, und er stand nicht erst
im geplanten Dashboard, sondern seit P10 in Mission Control.

**Ursache.** `git tag` beantwortet die Frage nach dem **Repository**, nicht nach dem Ordner. Seit
`pm/D003` (Monorepo) liegen Projekte ab P10 als Ordner in `projects`; `git -C projects/p11 tag`
liefert deshalb die Tags von `projects`. Das Cockpit nahm davon die letzte Zeile.

**Regel 1 — bei einem Behälterwechsel wird jeder Werkzeugaufruf neu befragt.** Als aus „ein
Projekt = ein Repo" „ein Projekt = ein Ordner in einem Repo" wurde, hat sich die Bedeutung **jedes**
repo-bezogenen Aufrufs still geändert: `git tag`, `git log`, `git status`, jede Pfadableitung, jede
Zählung „pro Repo". Die Discovery wurde damals nachgezogen (SWR-070) — die Nachbarn nicht. Der
Prüfsatz: *Welche Frage stelle ich hier eigentlich dem Repository, und meine ich den Ordner?*

**Regel 2 — ein Nachbar, der richtig aussieht, kann aus Versehen richtig sein.** `einstufung` liest
**denselben** Tag-Text und war korrekt — aber nur, weil sie nach `f"{projekt}-v1.0"` sucht und
damit **zufällig** nach dem Projektnamen filtert. Wer beim Prüfen „der Nachbar stimmt ja" notiert,
muss dazuschreiben, **warum** er stimmt. Hier hätte die Antwort gelautet: *weil er filtert — und
der andere Leser tut es nicht.* Das ist B059 und L-2026-08-16m ein zweites Mal: **eine geteilte
Quelle mit zwei Auflösungen.** Beide gehen jetzt durch **eine** Funktion.

**Regel 3 — ein Substring-Test über Namen ist ein Präfix-Test, der noch nicht fertig ist.**
`"p11-v1.0" in text` hält `xp11-v1.0` für einen Treffer. Wo Namen einen Namensraum bilden, wird am
Anfang verglichen, nicht irgendwo.

**Regel 4 — die Korrektur braucht den Test, der sie widerlegen würde.** Nach dem Filter lag nahe,
ihn überall anzuwenden. `p0` trägt `genesis-v1.0` — konventionsfremd; ein globaler Filter hätte p0
Baseline **und** Status `abgeschlossen` genommen. Der Test dazu steht ausdrücklich in der Suite,
nicht nur die fünf, die die Korrektur bestätigen.

**Gefunden hat es niemand, der danach suchte.** Der Anlass war eine Feldliste, die jemand
vollständig durchgehen musste. Der Befund ist damit der zweite Beleg in zwei Sprints für Regel 1
aus **L-2026-08-16h**: *gegen den echten Bestand laufen, nicht gegen die Testwelt.*

## L-2026-08-17f — Ein Gate am falschen Ort ist keine Reihenfolgefrage (B061, Auflösung)

**Woher.** `pm/T-0042` beschrieb die Zwickmühle korrekt und bot vier Wege an — drei davon
Reihenfolge- oder Wiederholungsvarianten. Die Entscheidung in Sprint 3 hat keinen davon gewählt,
sondern die Frage eine Ebene tiefer gestellt: *Kann dieses Gate den Zustand, über den es urteilt,
überhaupt jemals sehen?*

**Die Antwort war nein.** Die SWR↔Test-Matrix ist eine Aussage über **alle** Repos **zur gleichen
Zeit**. Eine CI, die je Repo läuft, sieht die anderen immer so, wie der Push sie hinterlassen hat.
Kein Push-Auftrag der Welt erzeugt Gleichzeitigkeit.

**Regel 1 — vor dem Justieren eines Gates kommt die Frage nach seinem Ort.** Prüft ein Gate eine
Eigenschaft, die über seine Grenze hinausreicht (mehrere Repos, mehrere Dienste, mehrere Läufe),
dann gehört es dorthin, wo diese Grenze nicht existiert. Alles andere ist Feinjustage an einem
Denkfehler.

**Regel 2 — der Test für „überflüssig oder falsch".** Für jeden Push über `abschluss.cmd` konnte
das Gate nur zweierlei sein: überflüssig (Schritt [2/5] hatte dieselbe Prüfung schon **vor** dem
Push mit Abbruch gefahren) oder falsch. Ein Gate, das **keinen** echten Befund erbringen kann, den
ein früheres Gate nicht schon verhindert, ist keine Sicherheit, sondern Rauschen — und Rauschen an
einer Ampel erzieht innerhalb weniger Läufe zum Wegsehen.

**Regel 3 — wer ein Gate entfernt, schreibt auf, welcher echte Befund damit verloren geht.** Hier:
ein Push, der `abschluss.cmd` umgeht, wird nicht mehr gegen die Matrix geprüft. Der Satz steht im
Ticket **und im Kopf der Workflow-Datei**, nicht nur im Ticket — wer den Workflow liest, soll den
Preis sehen, ohne ein Ticket zu suchen. Ein „aufgeräumtes" Gate ohne benannten Preis ist eine
stille Schwächung.

**Regel 4 — die gleiche Bauart nebenan wird benannt, nicht mitgerissen.** Der Katalog-Check in
derselben CI hat dasselbe Problem und hat noch nie falsch rot gemeldet. Ihn auf theoretischen
Verdacht mit abzuräumen wäre das Gegenteil der Sorgfalt, die den Befund gefunden hat. Er bleibt,
mit einem Satz im Workflow-Kopf und einer Zeile, die seine häufigere Rennhälfte beseitigt
(`process` wird vor `platform` gepusht).

## L-2026-08-17g — Die Gegenprüfung prüft auch die BEGRÜNDUNG, nicht nur den Code (B064/B065)

**Woher.** Die unabhängige Gegenprüfung von `platform/T-0005` (L-2026-08-16m, dritter Einsatz) fand
bei grüner Suite elf Punkte. Bemerkenswert ist nicht die Zahl, sondern die **Art**: die Hälfte
waren keine Codefehler, sondern **falsche Sätze in Ticket, Doku und Kommentar** — geschrieben von
demselben Lauf, der den Code korrigiert hat.

Die vier, die wehtaten:

* *„Aus dem Substring-Test wurde ein Präfix-Test."* Galt nur für den Zeilenfilter. Der Statustest
  in `einstufung` suchte weiter im **ganzen** Text inklusive Tag-Annotation — ein Zwischenstand
  `p11-v0.9` mit der Nachricht „Vorbereitung auf p11-v1.0" hätte das Projekt als abgeschlossen
  ausgewiesen. Der Satz war nicht ungenau, er war falsch, und er stand **dreimal** (Ticket, Doku,
  Test-Docstring). Ein Fehler, der behoben zu sein **behauptet** wird, wird nicht mehr gesucht.
* *„Getragen wird das von `preflight.py` und Schritt [2/5]."* `preflight.py` kennt die Matrix
  nicht. Das ausgesprochene Sicherheitsnetz war doppelt so groß gemalt wie das echte — im selben
  Absatz, der den Preis einer Gate-Entfernung „ehrlich" nennen sollte.
* *„Fünf Regressionstests."* Einer davon war ohne die Korrektur grün und bewies nichts. Eine
  Testzahl ist eine Behauptung über Deckung, keine über Zeilen.
* *„Teams haben kein G4, also keine Baseline."* Klang zwingend, war am echten Payload widerlegt:
  `platform` ist `festes-team` und trägt eine. Ein vertragstreues Widget hätte einen echten Wert
  unterdrückt.

**Regel 1 — jeder Satz der Form „X ist jetzt Y" bekommt den Befehl daneben, der es zeigt.** Nicht
in die Datei, aber in den Lauf: wer schreibt „der Test ist jetzt ein Präfix-Test", führt vorher
`grep` auf die Stelle aus, die er meint. Die vier Fehler oben wären jeder von einem einzigen
Befehl aufgedeckt worden.

**Regel 2 — die Deckung eines Tests wird durch Rückbau bewiesen, nicht durch Zählen.** Korrektur
in einer Kopie zurückbauen, Tests laufen lassen: was grün bleibt, beweist nichts. Zwei von acht
blieben grün; einer davon zu Recht (Gegenprobe gegen Über-Filtern), einer zu Unrecht.

**Regel 3 — ein ausgesprochener Preis ist eine Tatsachenbehauptung und wird wie eine geprüft.**
Wer eine Sicherung entfernt und dazuschreibt, wer sie ersetzt, sagt etwas Nachprüfbares über den
Ersatz. Die Versuchung, das Netz größer zu malen, ist genau dort am größten, wo man gerade etwas
wegnimmt.

**Regel 4 — Korrekturen werden nachgetragen, nicht überschrieben.** Die widerlegten Sätze stehen
in `pm/T-0042` und `platform/T-0005` **wörtlich** als widerlegt da. Wer sie stillschweigend
ersetzt, nimmt dem nächsten Leser den einzigen Hinweis darauf, welche Art Satz hier schon einmal
ungeprüft durchging.

---

## L-2026-08-17h — Eine leere Stelle sieht immer nach „noch nicht" aus (B038-Familie)

*Anlass: `platform/T-0006` / SWR-108, Sprint 4 (2026-08-17).*

Fünfzehn von sechzehn Einträgen im Cockpit meldeten `kpi: {laeufe: 0}`, obwohl nur `p0`
überhaupt eine Run-Registry führt. Der Payload behauptete fünfzehnmal eine Messung, die es
nicht gab — und zwar in der Form, die am meisten nach Fakt aussieht: als **Zahl**. Kein
Mensch hätte diese Null hinterfragt.

**Regel 1 — für „nicht geliefert" ist die Tatsache eine andere als für den Wert, und sie muss
benannt werden.** Nicht „laeufe ist 0", sondern „die Registry-Datei existiert nicht". Nicht
„das Feld ist leer", sondern „die SLA sagt nichts dazu". Wer die Unterscheidung an der Leere
des Werts festmacht, hat sie nicht getroffen, sondern nur umbenannt.

**Regel 2 — die Tatsache ist die Zusage, nicht ihr Nebenprodukt.** „Führt das Team Digests?"
war beinahe an `os.path.isdir("digest")` festgemacht worden. Das Verzeichnis entsteht aber
erst mit dem **ersten** Digest — die Regel hätte genau im Moment davor „führt keine" gesagt,
also in dem einzigen Moment, für den man sie braucht. Dieselbe Frage bei der Baseline: es
entscheidet das **Profil** (welche Gates gelten), nicht die **Gruppe** (was der Eintrag ist).
An dieser Verwechslung war schon der erste Widget-Vertrag gescheitert.

**Regel 3 — eine vorhandene Redewendung erweitern schlägt eine neue erfinden.** `team: null`
hieß im Payload längst „kein Team". Drei Wege standen im Ticket; gewählt wurde der, der schon
im Haus war. Ein Zusatzfeld neben dem Wert (`kpi_erhoben`) wäre die zweite Aussage über
dieselbe Sache gewesen (B033), ein Herkunfts-Vokabular (`quellen:`) eine Pflegelast für einen
Bedarf, den heute niemand hat.

**Regel 4 — wer den Wertebereich eines Feldes weitet, zieht die Leser im selben Commit mit.**
`null` erzeugt bei einem unvorbereiteten Leser keinen falschen Wert, sondern einen **Absturz**
(`p.kpi.kosten_eur.toFixed(...)`) — im HMI die ganze Kachel. Das ist ein anderer Fehlermodus
als vorher und ein schlechterer, wenn man ihn übersieht. Deshalb steht das Mitziehen in der
**Verifikation** der Anforderung und nicht als Folgeticket.

**Regel 5 — zu jedem verworfenen Weg gehört ein Test, der ihn nachstellt.** Vier der fünfzehn
Tests fallen nicht ohne die Korrektur um, sondern gegen eine **plausible falsche** Umsetzung:
`laeufe == 0` als Kriterium, das Verzeichnis statt der Zusage, die Gruppe statt des Profils,
die Profilprüfung vor der Tag-Prüfung. Jede wurde einzeln nachgebaut und der Test fiel um.
Ohne solche Tests ist der verworfene Weg nur eine Notiz im Ticket, die der nächste Umbau
nicht liest.

**Und die Zählung dazu, nach L-2026-08-17g Regel 2:** 15 Tests = **6** mit Rückbau-Nachweis,
**4** gegen nachgestellte Fehlumsetzungen, **5** Regressionswächter für die unveränderten
Normalfälle. „15 Tests" allein wäre wieder eine Behauptung über Deckung gewesen.

## L-2026-08-17j (platform/T-0007): Ein ausbleibendes Ergebnis hat zwei Erklärungen — und die Datei, die es sagt, lag drei Sprints lang offen da

Drei Sprints in Folge trugen `platform/T-0004` und `pm/T-0043` denselben Satz: *„Der Beleg
kommt beim nächsten `abschluss.cmd` von selbst, keine Handlung nötig."* Sprint 4 verschärfte
ihn noch: *„Nachzuschauen, ob sich eine Datei geändert hat, die sich nur durch einen Hostlauf
ändern kann, ist keine Arbeit."*

Der Wächter lief die ganze Zeit — alle 15 Minuten — und **brach jedes Mal ab**. `board.py`
starb im `board-check` von `pm` an einer Kodierung. Nichts wurde je gepusht.

**Das Schärfste daran: das Erkennungsmuster stand schon geschrieben.** L-2026-08-16
(cmd-Klammern-Bug) endet mit dem Satz *„Erkennungsmuster: `PUSH-ANFORDERUNG.txt` bleibt
liegen + `abschluss-auto.log` ansehen"*. Genau dieses Muster lag vor: Die Datei **war**
liegengeblieben und trug **zwei** Zeilen, aus Sprint 3 und Sprint 4 — der Wächter löscht sie
bei Erfolg. Zwei Zeilen in dieser Datei sind zwei gescheiterte Läufe, ausgeschrieben, an der
Stelle, an die das Team selbst bei jedem Sprintende schreibt.

**Regeln:**

1. **Wer sagt „X kommt von selbst", prüft die Quelle von X — nicht nur X.** Ein fehlendes
   Ergebnis heißt „noch nicht gelaufen" **oder** „gelaufen und gescheitert". Nur die erste
   Lesart rechtfertigt Warten, und sie ist die bequemere. Solange die zweite nicht
   ausgeschlossen ist, ist „warten" eine Annahme und kein Befund.
2. **Ein Wartegrund, der einen zweiten Sprint überlebt, ist kein Wartegrund mehr, sondern
   eine Hypothese** — und wird in dem Sprint geprüft, in dem er sich wiederholt. Sprint 3
   schrieb selbst: „steht hier ein zweites Mal, damit es beim dritten Mal auffällt." Beim
   dritten Mal ist es nicht aufgefallen, weil die Zeile dieselbe blieb. **Eine Wiederholung
   fällt nur auf, wenn jemand sie zählt** — deshalb ist die Wiederholung selbst der Auslöser
   für eine Prüfung, nicht für einen weiteren Vermerk.
3. **Ein Protokoll, das niemand liest, ist kein Protokoll.** `abschluss-auto.log` ist auf
   24 MB gewachsen, während drei Sprints über seinen Inhalt spekuliert haben. Der Startcheck
   einer Session schaut ab jetzt auf das **Ende** dieser Datei, wenn `PUSH-ANFORDERUNG.txt`
   noch liegt.
4. **Was diese Sandbox nicht sehen kann, muss die Sandbox ausdrücklich sagen.**
   `text=True` ohne `encoding=` ist hier (UTF-8) unauffällig und am Host (cp1252) tödlich.
   Fehlerklassen, die von der Umgebung abhängen, gehören in einen Test über den **gesamten**
   Produktionscode — nicht in eine Korrektur an der Fundstelle.
5. **Eine Lehre, die nur an ihrem Fundort steht, schützt genau eine Zeile.** In
   `preflight.py` trägt `git_laeuft()` seit `pm/T-0024` ein `errors="replace"` samt
   Begründung; die drei Nachbaraufrufe **derselben Datei** haben es nie bekommen. Wer eine
   Regel erkennt, schreibt den Test, der sie überall durchsetzt — sonst ist es keine Regel,
   sondern eine Anekdote.

## L-2026-08-17k (platform/T-0008): Ein Rückgabewert, der zwei Dinge heißt, sagt am Ende das Harmlosere

`status_in_head` gab `None` zurück für „Ticket ist neu" **und** für „nicht gelesen"
(T-0007) **und** für „Pfad falsch, weil das Repo verschachtelt ist" (T-0008). In allen drei
Fällen wurde die Prüfung übersprungen und `board-check` meldete `OK`. Für `p10`, `p11` und
`p12` hat SWR-002 deshalb **nie** geprüft — auffallen konnte das nicht, weil das Ergebnis in
allen Fällen dasselbe freundliche Wort war.

**Regel:** Ein Rückgabewert, der „alles in Ordnung" und „ich konnte nicht nachsehen"
zusammenfasst, ist ein stiller Ausfall mit Ansage. Die zwei Fälle bekommen zwei Werte, und
der zweite wird ein **Befund** — auch dann, wenn das lauter ist. Dieselbe Familie wie
SWR-108 (`null` vs. echte Null): eine leere Stelle sieht immer nach „nichts zu melden" aus.

## L-2026-08-17l (platform/T-0009): Eine Reparatur, die nur ein Ende eines Rohrs anfasst, verschiebt den Fehler ans andere

`platform/T-0007` hat die **Leseseite** jedes Subprozess-Aufrufs auf UTF-8 festgelegt — für
`git` genau richtig. An den drei Stellen, an denen Python **Python** aufruft, schrieb der
Kindprozess aber weiter in der Locale-Kodierung des Hosts. Vorher passten beide Seiten
zufällig zusammen (cp1252/cp1252); danach schrieb das Kind cp1252 und der Elternprozess las
UTF-8. Jeder Umlaut wurde zu U+FFFD — und U+FFFD ist genau das Zeichen, das cp1252 auf dem
Rückweg **nicht ausgeben** kann. Der Lauf starb nicht mehr am Lesen, sondern am `print`.

**Regel 1 — beide Enden oder keines.** Wer die Kodierung eines Datenstroms festlegt, legt
sie an **beiden** Enden fest oder an keinem. Ein Aufruf ist kein Lesevorgang, sondern ein
Rohr; „wir lesen jetzt fest UTF-8" ist eine halbe Aussage, solange niemand sagt, was der
Schreiber tut. Die beiden Einstellungen gehören deshalb in **eine** Datei
(`scripts/konsole.py`) und nicht je Aufrufstelle nebeneinander.

**Regel 2 — `errors="replace"` ist keine Reparatur, sondern eine Verschiebung.** Es erzeugt
U+FFFD, also ein Zeichen, das viele Ausgabeströme selbst nicht können. Wo der Wert nur
weitergereicht wird, ist das eine Zeitbombe mit Zündschnur. Für **Ausgabe** ist
`backslashreplace` die richtige Wahl: reines ASCII, auf jedem Strom darstellbar, und der
Leser sieht, dass etwas ersetzt wurde.

**Regel 3 — ein Werkzeug, dessen Aufgabe das Melden ist, darf am Melden nicht sterben.**
Die Meldungen dieser Organisation zitieren Ticketinhalte, und **121 Ticketdateien** tragen
ein „→", das cp1252 nicht kennt. Dieser Absturz war unabhängig von der Kodierungspaarung
möglich und wäre früher oder später auch ohne sie eingetreten. Erkennungsfrage vor jedem
`print`, das fremden Text weiterreicht: *Kann dieser Text ein Zeichen enthalten, das der
Ausgabestrom nicht kann?* Wenn ja, ist die Antwort eine Einstellung am Strom und nicht
Hoffnung.

**Regel 4 — ein Regel-Test beschreibt eine Regel, nicht eine Zeile, und deckt deshalb
genau so viel ab, wie seine Regel sagt.** `test_kein_produktionsaufruf_liest_ohne_feste_kodierung`
aus T-0007 prüfte den **gesamten** Produktionscode und war trotzdem blind für diesen
Defekt: er prüfte das Lesen. Ein Rohr hat zwei Enden. Wer einen Regel-Test schreibt, prüft
danach, ob die Regel den ganzen Sachverhalt beschreibt oder nur die Hälfte, die gerade
wehgetan hat.

## L-2026-08-17n (Sprint 7): Ein grünes Ergebnis beschreibt den Zustand, den es gemessen hat — nicht den, den man ausliefert

Sprint 6 meldete an vier Stellen **„Matrix 109 SWRs / 0 Lücken"** und pushte. `SWR-109` stand
zu diesem Zeitpunkt nur in der **Arbeitskopie** von `p9`; die gepushten Repos trugen 108. Der
Plattformcode und seine sieben Tests waren committet — die Anforderung, die sie erfüllen,
nicht. Für einen Tag hatte die Organisation Code ohne Requirement im Git und ein grünes
Traceability-Ergebnis, das für keinen Repo-Stand galt.

**Regel 1 — wer eine Kennzahl meldet, sagt dazu, welchen Zustand sie gemessen hat.** Die
Matrix liest die Platte (richtig so, sonst könnte man eine Anforderung nicht schreiben und im
selben Lauf prüfen). Der Push liefert HEAD. Beide sind für sich korrekt; falsch war, dass
**niemand vor dem Push gefragt hat, ob das Gemessene das Gelieferte ist**. Diese Frage gehört
zu den Zustandsfragen (`preflight`) und nie in das Werkzeug, dessen Ergebnis sie prüft — ein
Werkzeug, das seine eigene Voraussetzung bestätigt, sagt nichts.

**Regel 2 — eine Prüfung, die häufiger belanglos als richtig anschlägt, ist eine
Wegseh-Übung.** Der Befund **war sichtbar**: `preflight` schrieb `[p9] Arbeitskopie nicht
sauber (1 Datei(en))`. Daneben standen fünf gleich aussehende Zeilen, alle `1 Datei(en)`, alle
eine `BOARD.md`, deren `Stand:`-Zeile das Werkzeug bei **jedem** Lauf neu erzeugt. Sechs
identische Meldungen, fünf davon dauerhaft belanglos — die sechste hatte keine Chance. Zwei
Konsequenzen: eine Meldung **nennt**, worum es geht, statt zu zählen (B038 in Zeilenform); und
eine dauerhaft belanglose Meldung bekommt eine begründete Ausnahme, entschieden am **Inhalt**
(hier: am Diff) und nie am Namen.

**Diese Sorge ist jetzt viermal dieselbe gewesen** — SWR-109 (Takt-Tickets nicht melden),
SWR-110 (Stand-Zeile), SWR-112 (`decision-request` nicht melden) und B049. Sie ist damit
keine Einzelfallabwägung mehr, sondern eine **Bauregel: zu jeder neuen Prüfung gehört die
Frage, was sie am ersten Tag melden würde, und ein Test für den Fall, den sie NICHT melden
soll.** Ohne diesen Gegentest ist eine Ausnahme nicht widerlegbar.

**Regel 3 — eine Frage der Organisation wird nicht kachelweise beantwortet** (aus `pm/T-0036`
Teil c, B049). Der „ohne Frist"-Zähler galt dreimal als abgearbeitet, während in einer anderen
Kachel drei Tickets offen blieben. Ein Zähler ist erst abgearbeitet, wenn die **Summe über
alle** Einträge 0 ist oder jeder Rest eine im Ticket ausgeschriebene Begründung trägt.
„Kachel X erledigt" ist keine gültige Abschlussmeldung.

**Regel 4 — eine Konstanz zu erklären ist die bequeme Variante, sie zu prüfen.** „Nicht
geschlossen" stand vier Sprints auf 15 und passte zu keiner Zählweise des Werkzeugs. Zweimal
wurde die Konstanz ausdrücklich **kommentiert** („zum dritten Mal dieselbe Zahl, und zum
dritten Mal ist es Zufall") statt nachgerechnet. Eine Zahl ohne aufgeschriebene Zählweise ist
keine Kennzahl, sondern eine Behauptung — sie ist weder prüfbar noch widerlegbar. Eine Zahl,
die sich über mehrere Sprints nicht bewegt, ist derselbe Prüfanlass wie ein Wartegrund, der
sich wiederholt (L-2026-08-17j Regel 2).

**Regel 5 — ein rotes Ergebnis altert nicht und meldet sich nicht.** `pm/T-0043` stand fünf
Sprints offen. `p3` und `p5` waren rot, weil ihr letzter Stand vom 16.08. gegen ein neueres
Werkzeug geprüft worden war — und blieben rot, weil ohne Push kein neuer Lauf entsteht. Vier
Sprints lasen das als **laufende Störung**, was es seit dem ersten Tag nicht mehr war.
Aufgelöst wurde es erst, als jemand den Auslöser **herstellte** statt ihn zu erwarten.
Erkennungsfrage: *Kann sich der Zustand, auf dessen Änderung ich warte, überhaupt ändern,
wenn ich nichts tue?* Lautet die Antwort nein, ist Warten keine Option, sondern ein Fehler.

---

## L-2026-08-17w — Ein Beschluss ohne Prüfung ist keine Regel (Sprint 11, `platform/T-0012`)

**Anlass, gemessen.** Der Auftraggeber hat Kalenderfristen im Sprintplan zum **zweiten
Mal** gerügt (`pm/N-0041`). Die erste Rüge hatte zu **SWR-106** geführt: *„Terminierung auf
Sprints statt auf Kalenderdaten"* — als Anforderung geschrieben, in v1.12 abgenommen, und
im Kopf des Sprintzählers wörtlich wiederholt. Fünf Sprints später trugen **14 von 14**
offenen Teamaufgaben ein Kalenderdatum.

**Die Ursache war keine Nachlässigkeit, sondern die Gegenprüfung.**
`unterminierte_tickets` meldete jedes offene Ticket **ohne** `frist`. Der Beschluss hat sie
nicht mitgeändert. Also tat jeder Lauf pflichtbewusst das Gegenteil des Beschlossenen.

> **Eine Entscheidung, die keine Prüfung mitgeändert hat, ist eine Absichtserklärung.**

**Spiegelbild zu SWR-122 (Sprint 10).** Dort wurde eine Prüfung berechnet und von
niemandem **gelesen**; hier wurde eine Regel beschlossen und von keiner Prüfung
**vertreten**. Beide Male stand das Richtige aufgeschrieben, beide Male gewann die
Mechanik. **Bauregel: Wer eine Regel beschließt, benennt im selben Zug die Prüfung, die
sie vertritt — oder schreibt auf, dass es keine gibt.**

**Regel 2 — eine Kennzahl steuert, sobald sie berichtet wird, auch wenn sie nichts
blockiert.** Der erste Entwurf der Antwort behauptete, der Startcheck werde ohne Datum
**rot**. Nachgemessen im Code: falsch, `befunde += 1` fehlte, die Zeile wurde nur gedruckt.
Es genügte, dass „unterminiert 0" zu den Zahlen gehört, die jeder Abschluss an den
Auftraggeber meldet. **Wer eine Kennzahl berichtet, hat sie zu einem Anreiz gemacht — die
Frage „blockiert sie?" ist die falsche.**

**Regel 3 — eine ungeprüfte Behauptung über den eigenen Code ist derselbe Fehler wie eine
ungeprüfte Zahl.** Der falsche Satz stand in dem Brief, der eine ungeprüfte Null
aufarbeitet. Gefunden wurde er nur, weil derselbe Lauf die Stelle **anfassen** musste.
Erkennungsfrage: *Habe ich in den Code gesehen, oder habe ich aus dem Namen geschlossen?*

**Regel 4 — eine Prüfung braucht einen Ort, an dem der Betroffene sie sieht.** Die neue
`kalenderfristen`-Zeile steht deshalb nicht nur im Startcheck, sondern auch im
Cockpit-Kopfblock: der Auftraggeber sieht ins Cockpit. Eine Meldung an einer Stelle, an die
niemand schaut, ist die halbe Wiederholung von SWR-122.

**Regel 5 — der Vertrag wird nie von dem nachgezogen, der den Payload ändert.** Der
Widget-Vertrag ist in **v2.3 und v2.4 hintereinander** von derselben Prüfung
(`test_vertrag_feldliste`) nachgefordert worden, nicht von der Session, die die Schlüssel
hinzufügte. Beide Male stand es hinterher im Vertrag mit dem Satz „das ist nicht der
Sorgfalt zu verdanken". Zweimal in Folge ist kein Zufall: **wer Code ändert, sieht den
Vertrag nicht** — die Dateien liegen in verschiedenen Repos, und der Gedanke „welches
Dokument beschreibt, was ich gerade geändert habe?" kommt beim Bauen nicht auf.

Konsequenz: **die Prüfung ist die Lösung, nicht der Notausgang.** Sie darf nicht als
Redundanz zum Vertrag gelesen werden (kein B033-Fall), sondern ist die einzige Stelle, an
der die beiden Repos überhaupt zusammengehalten werden. Erkennungsfrage beim Erweitern
eines Payloads: *welches Dokument außerhalb dieses Repos behauptet etwas über die Form, die
ich gerade ändere?*

---

## L-2026-08-17y — Ein Schreibversuch, der scheitert, kann die Datei trotzdem zerstören

**Gemessen, Sprint 12, an der eigenen Arbeitskopie.** Ein Patch-Skript öffnete
`preflight.py` mit `open(pfad, "w", newline="\\n")` — der Wert ist ungültig (`"\n"` ist
richtig, `"\\n"` nicht). Python wirft dafür `ValueError: illegal newline value`. Was es
**vorher** tut: die Datei im Modus `w` anlegen und damit **auf 0 Bytes kürzen**.

> Ergebnis: eine Ausnahme, die nach „nichts passiert" aussieht — und eine leere
> `preflight.py`. Die Prüfstrecke des Laufs war weg, bevor der Lauf sie brauchte.

**Warum das hier steht und nicht als Kuriosität durchgeht:** es ist derselbe Vorfall wie
`abschluss.cmd` in Sprint 1, das „versehentlich geleert und aus dem Protokoll
rekonstruiert" wurde und bis heute als einziger offener Punkt beim Auftraggeber liegt.
Zweimal dieselbe Klasse in elf Sprints.

**Regel 1 — schreiben heißt: neben die Datei schreiben und dann umbenennen.**
`open(tmp, "w") … os.replace(tmp, pfad)`. `os.replace` ist auf einem Dateisystem atomar;
scheitert das Schreiben, bleibt das Original **unberührt**. Jedes Patch-Skript dieses
Teams schreibt ab jetzt so. *(Der Nachbau ohne Temp-Datei ist genau der Fall oben.)*

**Regel 2 — Arbeitskopie zuerst, Bestand danach.** Der Schaden blieb folgenlos, weil der
Lauf auf `/tmp/genesis` arbeitete und der Bestand über `git checkout` wiederhergestellt
werden konnte. Das war **Glück aus einer anderen Entscheidung** (Tests auf einer Kopie,
Faktor ~50 schneller — Befund vom 11:05), nicht Vorsicht an dieser Stelle.

**Erkennungsfrage:** *Kann der Aufruf, der meine Datei öffnet, zwischen „geöffnet" und
„geschrieben" scheitern — und was steht dann drin?*

**Regel 3 — ein zweiter Lauf ist kein Randfall.** Derselbe Tag hat zwei Routine-Sessions
gleichzeitig in dieselben Repos schreiben sehen (Befund 11:05). Aufgenommen als
`platform/T-0013`: das Sprintregister kennt **keinen Endezeitpunkt** und kann Überlappung
deshalb nicht sehen. Erkennungsfrage vor jedem schreibenden Lauf: *bewegt sich der HEAD
der Repos, während ich messe?* Zwei Kontrollmessungen im Abstand von Minuten beantworten
das; dieser Lauf hat vier gebraucht, bis Ruhe war.

## L-2026-08-17ab — Eine Entscheidung im Fließtext ist für jede Prüfung unsichtbar

*Anlass: `platform/T-0014` / SWR-131, Sprint 13 (2026-08-17).*

Der Auftraggeber hat `projects/p12/T-0007` um **11:48:25** über die Inbox entschieden. Die
Inbox hat die Entscheidung angenommen, in den **Ticketrumpf** geschrieben und committet.
Danach schrieb derselbe Sprint 12 drei Berichte, die sagten, die Frage liege noch bei ihm —
und **es ging eine E-Mail** hinaus, die ihm einen „Neuen Decision Request" mit Frist 24.08.
ankündigte.

Die Ursache waren **vier Formulierungen eines Wortes**. `inbox` las „entschieden" am
Rumpfmarker, `board`/`aggregation`/`preflight` am `status`, `aggregation.cockpit` trug eine
dritte Kopie des Markers, `dr_benachrichtigung` eine vierte. ⚠ **Nicht alle waren falsch —
sie waren verschieden.** Das Cockpit hat den DR nach der Entscheidung korrekt nicht mehr
geführt, während der Preflight ihn als offen zählte. Das ist der eigentliche Preis von
B033: nicht eine falsche Anzeige, sondern zwei richtige, die sich widersprechen.

**Regel 1 — wer einen Zustand in Prosa schreibt, schreibt ihn nirgends hin.** Ein Zustand,
den eine Prüfung lesen soll, gehört in ein **Feld**. Prosa darf ihn erläutern, nie tragen.

**Regel 2 — „eine Stelle" wird gezählt, nicht behauptet.** Der erste Anlauf dieser
Anforderung stellte drei Leser um und übersah zwei; gefunden wurden sie erst durch ein
`grep` über den Quelltext. Es gibt jetzt einen Test, der **die Definitionen im Quelltext
zählt** — ohne ihn wäre „eine Stelle" eine Aussage über den Einführungstag.

**Regel 3 — der Leser mit Außenwirkung wird zuerst geprüft.** Von vier Lesern war der
schwächste der, der E-Mails verschickt. Erkennungsfrage: *welcher dieser Leser erreicht
einen Menschen?*

**Regel 4 — eine Prüfung, die den Fehler zusichert, ist schlimmer als keine.**
`test_entschiedene_drs_ohne_warnung` verlangte für einen entschiedenen DR ausdrücklich
`["gesendet"]`. Das Fehlverhalten war nicht ungeprüft, sondern **geprüft und bestätigt** —
und damit gegen jede Änderung verteidigt. Erkennungsfrage bei jedem Test, der zwei Dinge in
einer Zeile prüft: *sichert er das eine zu und das andere nur mit?*

**Regel 5 — eine Reparatur, die verstummt, ist keine.** Einen entschiedenen DR aus
„wartet auf den Menschen" zu nehmen war richtig und allein **schlimmer als der Fehler**:
das Ticket wäre aus jeder Anzeige verschwunden und weiter offen geblieben. Zu jeder
Bedingung, die etwas ausblendet, gehört im selben Lauf der Leser, der es auffängt.

**Erkennungsfrage für den nächsten Lauf:** Sprint 12 hat notiert, dass bei 60-Minuten-Takt
ein **Brief mitten im Lauf der Regelfall** ist, und daraus die Doppelprüfung des
Briefkastens gebaut — für **Entscheidungen** aus derselben Inbox nicht. *Welche anderen
Eingänge des Menschen prüfe ich nur einmal?*

## L-2026-08-17ac — Verwaiste Git-Locks lassen sich auf diesem Mount nicht löschen, aber umbenennen

*Anlass: Sprint 13 (2026-08-17), vier abgebrochene Commits.*

SWR-123 räumt verwaiste `.git/index.lock` per `unlink`. Auf dem Cowork-Mount scheitert das
mit `Operation not permitted` — **`os.rename` gelingt dagegen**. Ein Commit hinterlässt hier
regelmäßig einen Lock, der den nächsten Commit im selben Repo abweist.

**Regel — Lock beiseite benennen, nicht löschen wollen.** `os.rename(lock, lock +
".verwaist")` vor `git add` und vor `git commit`. Dieselbe Grundoperation wie in
`L-2026-08-17y` (Temp-Datei schreiben, dann `os.replace`) — **derselbe Nachbarfall, zwei
Sprints später wiedererkannt statt neu gelernt.**

⚠ Die Voraussetzung aus SWR-123 bleibt: geräumt wird nur, wenn **kein** Git-Prozess läuft.
Das Umbenennen macht die Räumung möglich, nicht zulässig.

**Offen:** die Regel steht hier und **nicht im Code** — SWR-123 räumt weiter per `unlink`.
Aufgenommen als `platform/T-0015`. *Eine Regel, die keine Prüfung vertritt, ist die Lehre
aus SWR-125; sie wird hier bewusst als Schuld notiert und nicht als erledigt gemeldet.*

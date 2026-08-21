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

## L-2026-08-17ae — Die Kollisionsregel galt für Ticket-IDs, nicht für Anforderungsnummern

*Anlass: Sprint 14 (2026-08-17), zwei gleichzeitige Läufe, `SWR-134` doppelt belegt.*

Zwei Routine-Läufe hielten am 2026-08-17 gleichzeitig Sprint 14. Der eine baute
`platform/T-0015` (Git-Schreibweg), der andere `p11/T-0010` (Dashboard-Endpunkt). **Beide
haben ihre Anforderung `SWR-134` genannt.** Entdeckt wurde es nicht von einer Prüfung,
sondern beim Lesen des fremden Commits — die Nummer stand plötzlich in einer Datei, die der
eigene Lauf nie angefasst hatte.

Die Regel vom 2026-08-16 lautet: *die nächste freie **Ticket-ID** gegen HEAD prüfen.* Sie
hat gehalten — `platform/T-0016` und `p11/T-0010` sind kollisionsfrei. **Für
SWR-Nummern gab es diese Regel nicht**, und sie ist genauso knapp: die höchste vergebene
Nummer steht in einer einzigen Datei, die beide Läufe lesen und schreiben.

> **Eine Kollisionsregel schützt die Kennungen, die sie nennt — und keine anderen.**

**Regel 1 — jede fortlaufende Kennung braucht dieselbe Prüfung.** Nicht nur `T-xxxx`:
`SWR-xxx`, `D0xx` (Decision-Log), `N-xxxx` (Briefe), `L-2026-xx` (Lessons), `ADR-xxx`. Alle
werden hochgezählt, alle stehen in genau einer Datei, alle kollidieren gleich.

**Regel 2 — gegen einen bewegten HEAD ist Prüfen ein Wettlauf.** Das steht seit dem
11:05-Befund so da und hat sich hier bestätigt: der andere Lauf hat die 134 *unmittelbar vor
seinem Commit* geprüft und trotzdem kollidiert. Die Prüfung ist nicht falsch, sie ist gegen
Nebenläufigkeit **wirkungslos** — die Reparatur ist `platform/T-0013` (Sprintregister mit
Ende), nicht eine gründlichere ID-Prüfung.

**Regel 3 — der committete Stand gewinnt.** Wer seine Nummer noch in der Arbeitskopie hat,
nummeriert um. Das ist die einzige Zuordnung, die nicht willkürlich ist, und sie braucht
keine Absprache zwischen zwei Läufen, die einander nicht sehen.

**Erkennungsfrage:** *Welche Nummer, die ich in diesem Lauf vergebe, könnte ein anderer Lauf
im selben Takt auch vergeben — und woran würde ich es merken?*

---

## L-2026-08-17af — Eine Reparatur, die nur ihr Fundort benutzt, repariert den Fundort

**Herkunft:** Sprint 14, `platform/T-0015` / SWR-134. Gemessen vor dem Bauen.

Das Ticket verlangte, die Lock-Räumung aus SWR-123 solle bei `PermissionError` auf
`rename` zurückfallen. **Dieser Rückfall existierte seit `pm/T-0023` (2026-08-16).** Die
DoD zu bauen hätte nichts geändert.

Der wirkliche Befund lag eine Ebene daneben und war größer: **8** Stellen der Organisation
bauen einen `git commit`, **1** davon räumte die Sperre — der Briefkasten, also genau der
Ort, an dem der Fehler damals aufgefallen war. Die anderen sieben liefen in einen Fehler,
für den die Reparatur seit Sprint 5 im Haus lag.

> **Eine Reparatur, die nur ihr eigener Fundort benutzt, ist eine Reparatur des Fundorts
> und nicht des Fehlers.**

**Regel 1 — wer einen Fehler an einer Stelle behebt, zählt im selben Zug die Nachbarn.**
Nicht „gibt es Nachbarn?", sondern **wie viele** und **welche**. Die Zahl gehört ins
Ticket; ohne sie ist „behoben" eine Aussage über eine Datei und liest sich wie eine über
das System.

**Regel 2 — die Reichweite wird von einem Zähltest gehalten** (aus SWR-131 übernommen und
hier zum zweiten Mal angewandt). „Alle Schreiber gehen durch einen Weg" ohne Test ist eine
Aussage über den Tag der Einführung.

**Regel 3 — ein Ticket sagt, ob seine Ursache gemessen oder geraten ist.** Sprint 13 hat
den Symptomweg notiert und auf die Umsetzung von SWR-123 geschlossen, **ohne sie zu
lesen** — genauso wie `platform/T-0013` einen 60-Minuten-Takt annahm, den seine eigene
Messung widerlegte. Im Ticket stand in beiden Fällen nicht, welches von beidem es war.

**Erkennungsfrage:** *Die Stelle, an der ich den Fehler gefunden habe — ist sie die einzige,
die so arbeitet? Wie viele sind es, und habe ich sie gezählt oder geschätzt?*

---

## L-2026-08-17ag — Eine gemeldete Korrektur, die nur im Bericht steht, ist keine

**Herkunft:** Sprint 14, Startprüfung an `platform/T-0013`. Gemessen am Commit-Diff.

Sprint 13 hat dreimal berichtet, die DoD von `platform/T-0013` sei *„im Ticket
korrigiert"*, die *„Neufassung steht im Ticket"*. Der einzige Commit von Sprint 13 an
dieser Datei (`37eb1e5`) enthält **eine Zeile**: `geplant_sprint: 13` → `14`. Die Messung
(12 Abstände, Median 57, 7 von 12 unter 60) und die Neufassung standen ausschließlich im
Sprintbericht.

⚠ Das Bittere daran: **derselbe Lauf hatte den Satz dafür schon geschrieben.** Sein
Hauptbefund lautete *„Eine Entscheidung im Fließtext ist für jede Prüfung unsichtbar"* —
und eine korrigierte Abnahme im Fließtext eines Berichts ist für jeden Bauenden unsichtbar.
Fünfte Wiederholung der Familie (SWR-122/125/128/131), und die Erkennungsfrage aus
`L-2026-08-17x` wurde im selben Lauf gestellt und **auf den eigenen Bericht nicht
angewandt**.

> **Der Bericht ist der Ort, an dem man erzählt, was man getan hat — nicht der Ort, an dem
> man es tut.**

**Regel 1 — was ein späterer Lauf beim Bauen braucht, steht im Ticket.** Berichte werden
gelesen, wenn jemand wissen will, was geschah; Tickets, wenn jemand baut. Eine DoD, die im
Bericht steht, erreicht die Bauenden nie.

**Regel 2 — wer im Bericht „im Ticket korrigiert" schreibt, öffnet das Ticket und liest
nach.** Ein Satz über eine Datei ist so viel wert wie der letzte Blick hinein.

**Regel 3 — die Prüfung dafür ist billig und fehlt:** Wörter der eigenen Berichtszeile
gegen den Ticketinhalt halten. Solange sie fehlt, ist dies eine Regel ohne Prüfung — die
Lage aus SWR-125, hier **benannt und nicht als erledigt gemeldet**.

**Erkennungsfrage:** *Welche Aussage meines Berichts behauptet etwas über den Inhalt einer
Datei — und habe ich diese Datei danach aufgemacht?*

---

## L-2026-08-17ah — Ein Zustandswechsel aus zwei Vorgängen hat einen Zwischenzustand, und der geht verloren

**Sprint 15, zweimal in einem Lauf eingetreten.** Ein Statuswechsel besteht heute aus zwei
Aufrufen: `board.py <repo> status T-xxxx <neu>` schreibt die Datei, ein zweiter Aufruf
bucht sie. Scheitert der zweite an einer verwaisten `.git/index.lock`, steht der neue
Zustand **in der Datei** und **nicht in der Historie** — und der nächste Wechsel
überschreibt ihn.

| Fall | Ticket | Ausgang |
|---|---|---|
| 1 | `platform/T-0013` | vor dem Commit bemerkt, Reihenfolge wiederhergestellt |
| 2 | `pm/T-0052` | committet: `in_progress -> done` steht in der Historie |

> **Beim ersten Mal ging es gut aus, weil jemand hingesehen hat — nicht, weil eine
> Vorkehrung gegriffen hätte.**

⚠ **Die Ursache ist nicht die Sperre.** Die ist behandelt: SWR-134 hat den einen
Schreibweg nach Git gebaut. Der Fehler liegt darüber — `setze_status` kennt den Commit
nicht, also muss ihn **jeder Aufrufer** kennen, bei jedem Wechsel, in jedem Repo, in jeder
Session.

> **Eine Reparatur, die der Aufrufer anwenden muss, ist keine Reparatur.**

Das ist SWR-134 ein Feld weiter: dort waren es acht Git-Schreibwege, hier sind es zwei
Vorgänge, die einer sein müssten.

**Regel 1 — ein Zustandswechsel ist ein Vorgang.** Wer den Zustand schreibt, bucht ihn im
selben Aufruf. Scheitert die Buchung, gilt der Wechsel als nicht geschehen und die Datei
geht zurück. (Als Ticket: `platform/T-0017`, Sprint 16 — hier **benannt und nicht als
erledigt gemeldet**, die Lage aus SWR-125.)

**Regel 2 — bis dahin: nach jedem Statuswechsel den Commit prüfen, bevor der nächste
kommt.** Ein `git log -1` kostet nichts; ein verlorener Zustand kostet einen
Preflight-Befund und eine Erklärung an den Auftraggeber.

**Regel 3 — einen schon eingetretenen Verstoß nicht glätten.** Fall 2 steht als dritter
Verstoß seit dem Stichtag im Befund. Kein Stichtag verschoben, keine Historie
umgeschrieben, kein Test angepasst (`L-2026-08-17ad`).

⚠ **Der Unterschied zwischen Fall 1 und Fall 2 ist die Aufmerksamkeit eines Augenblicks,
und darauf darf keine Regel bauen.** Fall 1 wurde bemerkt, weil der fehlgeschlagene
Commit unmittelbar auf dem Schirm stand; Fall 2 lief in derselben Befehlszeile durch, in
der schon der nächste Schritt folgte.

**Erkennungsfrage:** *Habe ich gerade einen Zustand geschrieben, dessen Buchung ich nicht
gesehen habe?*

## L-2026-08-17ai — Eine Reparatur, die ihre Abhängigkeit beim Aufrufer sucht, wirkt genau dort nicht, wo sie hin sollte

**Anlass (Sprint 16, gemessen in Produktion, `platform/T-0018` / SWR-143).** Drei Commits
scheiterten hintereinander an einer `.git/index.lock` und quittierten `geraeumt: 0` —
während ein **direkter** Aufruf derselben Räumfunktion sie in einem Zug wegräumte.

Ursache: `git_schreiben.entsperre` legte `os.path.dirname(__file__)` in `sys.path`. Das ist
`backend/`, `preflight` liegt in `scripts/`. Der Import scheiterte, die Funktion gab `0`
zurück, und die Räumung lief nicht.

> **Die Reparatur wirkte überall dort, wo der Aufrufer sie mitgebracht hat — also genau
> dort nicht, wo SWR-134 sie hinbringen wollte.**

⚠⚠ **Der Fehlermodus ist der schlimmste, den diese Bauart hergibt: unsichtbar in der
Teststrecke, sichtbar nur unter Last.** Die Zusicherung dazu war die ganze Zeit grün, weil
die Testdatei `scripts/` beim Import selbst in den Pfad legt.

**Regel:** Greift ein Modul über `sys.path` nach einem anderen, ist der Anker `__file__`
und der Pfad wird normalisiert — nie der Zustand des Aufrufers.

**Erkennungsfrage:** *Welche unserer Zusicherungen prüfen etwas, das die Testdatei selbst
eingerichtet hat?* ⚠ Für die rund 900 Python-Zusicherungen ist sie **unbeantwortet**, und
das steht hier, statt als erledigt zu gelten.

## L-2026-08-17aj — Eine Prüfung, die den Fehler zusichert, und eine, deren Regel zu weit gefasst ist, sehen gleich aus — und werden verschieden behandelt

**Anlass (Sprint 16).** SWR-139 machte drei bestehende Zusicherungen rot
(`test_ohne_sperre_wird_gar_nicht_geraeumt` und zwei im Briefkasten). Alle drei verlangten
*„ohne Fehlschlag darf gar nicht geräumt werden"* — und die neue Räumung **zwischen** `add`
und `commit` verletzt das wörtlich.

⚠ Der Reflex aus SWR-136 wäre gewesen, sie als *Prüfungen, die den Fehler zusichern* zu
behandeln. Das wären sie **nicht**: ihre Regel war **richtig** und nur zu weit gezogen. Der
Unterschied ist prüfbar:

| | Prüfung, die den Fehler zusichert (SWR-136) | Prüfung mit zu weitem Anwendungsbereich (hier) |
|---|---|---|
| Die zugesicherte Regel | war falsch | war und ist richtig |
| Nach der Korrektur | verschwindet sie | wird sie **schärfer** |
| Was zu tun ist | die Absicht retten, den Wortlaut aufgeben | die **Ebene** wechseln, auf der gemessen wird |

Konkret: gemessen wird ab jetzt die **Reihenfolge** (vor dem ersten Git-Aufruf wird nicht
geräumt) und die **Wiederholung** (genau eine), nicht die Abwesenheit der Räumung.

> **Der Grund, warum die Räumung dort erlaubt ist, muss ausgesprochen werden: ein
> gelungenes `add` ist der Nachweis, dass die danach liegende Sperre die eigene ist.**

**Regel:** Wird eine bestehende Zusicherung von einer neuen Anforderung rot, ist die erste
Frage nicht *„löschen oder anpassen?"*, sondern *„war ihre Regel falsch oder nur zu weit?"*
— und die Antwort gehört ins Ticket, nicht in den Bericht.

## L-2026-08-17ak — Was als „Markup" gilt, ist eine Entscheidung der Messung; eine unsaubere macht ein richtiges System rot

**Anlass (Sprint 16, `p12/T-0008` / SWR-099).** Der Vollständigkeitsnachweis des Renderers
meldete an sieben Briefen fehlende Zeichen: `1.`, `2.`, `3.`, `4.` — die **Marken einer
Nummernliste**. Die Quelle schreibt sie, das `<ol>` erzeugt sie; der Renderer wirft nichts
weg. Die Messung nahm `-` und `*` als Markup aus und Ziffern nicht.

⚠ Ein solcher Dauerbefund wäre **schlimmer als kein Nachweis**: er trainiert das Wegsehen
an — an genau der Prüfung, die später einen echten Verlust zeigen soll.

Korrigiert wurde nicht die Zeichenklasse, sondern die **Ebene**: die führende Marke einer
**Zeile** ist Markup, eine Ziffer mitten im Satz nicht. Beide Richtungen sind eigens
zugesichert, sonst wäre die Korrektur durch Verschieben der Regel grün zu bekommen.

**Regel:** Bevor eine neue Messung als Befund gilt, wird ihre eigene Definition geprüft.
Ein erster roter Lauf ist genauso oft ein Fehler der Messung wie einer des Gemessenen.

## L-2026-08-17al — Ein Werkzeug, dessen unvollständiger Modus an den Ort des vollständigen schreibt, macht aus einem Tippfehler einen Totalbefund

**Anlass (Sprint 17, `platform/T-0019` / SWR-145 — ausgelöst von diesem Lauf, nicht
gelesen).** Der Lauf wollte nach dem Bau von SWR-144 die Matrix erneuern und rief
`trace_matrix.py` **ohne** `--alle-projekte` auf. Antwort:

```
Matrix geschrieben: p0/verification/reports/swr-test-matrix.md — 24 SWRs, 101 Lücke(n).
```

Sprint 16 hatte **143 / 0** hinterlassen. 121 Zeilen weg, Exit **0**, und die Meldung
benutzte die Wörter „Matrix geschrieben".

⚠ **Keine der beiden Voreinstellungen ist allein falsch.** `--alle-projekte` ist aus, weil
ein Flag etwas *hinzuschaltet*; das Ziel ist die kanonische Datei, weil der Normalfall ohne
Argumente laufen soll. Erst zusammen schreiben sie den **Teilmodus an den Ort des Ganzen** —
und genau deshalb hat es keine der bestehenden Prüfungen gefunden.

⚠⚠ Und der Befund **tarnt sich als Katastrophe**: „101 Lücken" liest sich wie ein Einbruch
der Abdeckung, nicht wie ein falscher Aufruf. Ein Lauf, der hier abgebrochen wäre, hätte
eine Matrix hinterlassen, die 101 Anforderungen als unbelegt ausweist — und der nächste
Lauf hätte sie *reparieren* wollen.

Es ist die Bauart von **SWR-143** eine Etage höher: dort fand eine Funktion ihre
Abhängigkeit nur, wenn der **Aufrufer** sie mitbrachte; hier findet ein Werkzeug seine
Quellen nur, wenn der Aufrufer ein Flag mitbringt — und beide Male ist das Ergebnis eines
unwissenden Aufrufers **nicht als falsch erkennbar**.

**Regel:** Wo ein Generator seine Eingaben selbst entdecken kann, ist die **Discovery der
Standardfall** und die Einschränkung das Flag. Ein ausdrücklich übergebener Pfad gewinnt —
das ist die einzige Stelle, an der der Aufrufer nachweislich etwas anderes gemeint hat. Und
findet die Discovery **nichts**, wird **nicht geschrieben**: eine leere Ausgabe am Ort der
echten ist derselbe Fehler eine Stufe schlimmer.

**Zweite Regel, beim Bauen gemessen:** Bei zwei Fehlern in einem Aufruf entscheidet die
**Reihenfolge der Prüfungen**, welchen der Aufrufer erfährt. Die Quellenprüfung stand
zuerst hinter `tests_scannen` — und ein fehlendes Testverzeichnis meldete dann sein
`FileNotFoundError`, während die fehlende Quelle die Ursache war.

## L-2026-08-17am — Eine Warnung im Nachbarcode verhindert den Fehler nicht; die Zusicherung, die sie messbar macht, tut es

**Anlass (Sprint 17, `platform/T-0016` / SWR-146).** `Regeln.cockpitFeldText` wurde neu
geschrieben, um die Zustandsregel an **einer** Stelle zu führen. Der erste Entwurf fragte

```js
if (zustand === "nicht_geliefert" || !zustand) return texte.nicht_geliefert;
```

und fiel bei jedem **unbekannten** Zustand bis `String(wert)` durch — bei einem
unvollständigen Payload also auf die Zeichenkette `"undefined"`.

⚠ Das ist **wörtlich** der Fehler, den der Kommentar in `Regeln.feldText` seit SWR-135
beschreibt — *drei Funktionen weiter oben in derselben Datei*, mit dem Satz „eine Anzeige,
die aussieht wie ein Inhalt und keiner ist".

Gefunden hat es nicht die Erinnerung an den Kommentar, sondern eine Zusicherung, die
`undefined`, `null`, `""` und `"quatsch"` **durchprobiert** — beim ersten Lauf.

**Regel:** Eine geschlossene Menge wird **positiv** geprüft (`!== "wert"` → keine Daten),
nie über die Verneinung ihrer bekannten Fehlfälle. Und: ein Kommentar, der einen Fehler
beschreibt, ist keine Vorkehrung gegen ihn — die Vorkehrung ist der Test, der ihn
durchprobiert. Wer eine zweite Funktion neben einer bestehenden baut, liest deren
**Warnungen** und übernimmt deren **Prüfform**, nicht nur deren Absicht.

## L-2026-08-17an — Ein Test, der behauptet, ein Wert werde gelesen und nicht eingetragen, darf ihn nicht selbst eintragen

**Anlass (Sprint 17, `platform/T-0016` / SWR-146).** Der Bump des Widget-Vertrags auf v2.5
machte `test_die_vertragsversion_wird_gelesen_nicht_eingetragen` rot — weil dort `"2.4"`
als **Literal** stand. Der Test trug die Version damit an einer **zweiten** Stelle, also
genau dort, wo sein eigener Titel sie ausschließt.

Repariert ist nicht die Zahl, sondern der Vergleich: gegen die **Datei**, und mit einem
**anderen Verfahren** als die Funktion (`yaml.safe_load` gegen den Zeilenscanner). Zwei
unabhängige Lesungen, die übereinstimmen, sind die Zusicherung. Dazu die Gegenprobe gegen
„beide lesen nichts" (`^\d+\.\d+$`) — zwei leere Antworten wären gleich und trotzdem falsch.

**Regel:** Prüft ein Test, dass ein Wert aus **einer** Quelle kommt, darf er die Quelle
nicht kopieren. Er liest sie — möglichst anders als der Code — und vergleicht. Und er
sichert zu, dass überhaupt etwas gelesen wurde: Gleichheit zweier Leeren ist kein Beleg.

## L-2026-08-17ao — Wer mit der Vertragsfrage anfängt, kann den Vertrag nicht vergessen

**Anlass (Sprint 17, Widget-Vertrag v2.5).** Die Einträge v2.3 und v2.4 halten beide fest,
dass der Vertrag **nicht** von dem nachgezogen wurde, der den Payload änderte, sondern
jedes Mal vom Wächter `test_vertrag_feldliste` — verankert als `L-2026-08-17y`: *wer Code
ändert, sieht den Vertrag nicht.*

**v2.5 ist die erste Ausnahme.** Der Grund ist kein besseres Vorsatzverhalten, sondern die
**Reihenfolge**: die DoD von `platform/T-0016` hat die Vertragsfrage als **DoD 1** einen
ganzen Sprint vor den Code gestellt und das Bauen davor ausdrücklich verboten.

⚠ Das ist ein Beleg für **einen** Fall und noch keine Regel — die Reihenfolge war hier
ohnehin erzwungen, und der Vertragsanschluss war ihr Nebeneffekt. Als Zusage formuliert
gehörte er ins Runbook; hier steht er als Beobachtung.

**Regel (vorsichtig):** Berührt ein Ticket einen Vertrag, gehört die Vertragsfrage als
**eigener, erster** DoD-Punkt ins Ticket — nicht als Hinweis im Fließtext. Der Wächter
bleibt trotzdem; er ist die Lösung und nicht der Notausgang (`L-2026-08-17y`).

## L-2026-08-17aw — Aufschrift vom Server, Ziel selbst gebaut: zwei Aussagen über eine Sache

**Anlass (Sprint 18, `projects/p11/T-0012` / SWR-150).** `app.js` hatte **neun** Stellen, die
`"#/ticket/" + projekt + "/" + id` zusammensetzten — und in **sieben** davon stand als
**Beschriftung** `x.ref`, also die Kennung **vom Server** (SWR-087).

> **Ein Link, dessen Aufschrift der Server liefert und dessen Ziel die Ansicht zusammenbaut,
> ist zwei Aussagen über dasselbe Ticket. Solange beide gleich sind, merkt es niemand.**

Und sie sind nicht theoretisch verschieden: **68 Ticketnummern gibt es in mehr als einem
Projekt**, `T-0002` allein in **17** — gemessen, nicht befürchtet.

**Regel:** Wo der Server eine Kennung liefert, ist sie die Quelle für **Anzeige und Ziel**.
Eine Ansicht, die eine Kennung nur **beschriftet** und ihr Ziel selbst baut, hat SWR-087
formal erfüllt und sachlich unterlaufen.

⚠ Und die harte Hälfte: eine **nackte Nummer ergibt keinen Link**.
> **Ein Link auf das falsche Projekt ist schlimmer als kein Link. Der falsche öffnet ein
> fremdes Ticket und sieht dabei richtig aus.**

⚠ Wo eine Auflösung unmöglich ist — ein `T-nnnn` im Fließtext sagt nicht, aus welchem
Projekt es kommt —, heißt die Stelle nach dem, was sie tut (`textRefAnnahme`) und die Ansicht
macht die Annahme sichtbar. Eine Vermutung zu **benennen** ist weniger, als sie aufzulösen,
und mehr, als sie zu verstecken.

## L-2026-08-17ax — Ein Altbestand als Zahl kann nur sinken; als Warnung wächst er

**Anlass (Sprint 18, `projects/p12/T-0009` / ADR-P12-001, und ein zweites Mal in
`test_deep_links.py`).** P12 ist eine **Zusammenführung**: endet sie mit zwei Renderwegen,
hat sie ihr Ziel verfehlt, auch wenn alles funktioniert. Die Regel dagegen ist deshalb keine
Absichtserklärung im ADR, sondern ein **Zähltest** — `ALTBESTAND_TLINKS_AUFRUFE = 4`, und
`9 → 0` beim Zwillingsbefund in SWR-150.

> **Ein Altbestand, der als Warnung dasteht, wächst. Einer, der als Zahl dasteht, kann nur
> sinken — denn wer ihn erhöht, muss die Zahl anfassen und sieht dabei, dass er es tut.**

**Regel:** Ein benannter Altbestand gehört als **eingefrorene Zahl** in eine Zusicherung,
nicht als Hinweis in einen Kommentar. Der eine Fall, in dem er sinkt, ist der Nachweis der
Reparatur; der andere, in dem er steigt, ist ein roter Test.

⚠ Zwei Zusicherungen dieses Laufs halten bewusst den **heutigen** Zustand fest („der
Inline-Pass kennt die Ticketnummer heute **nicht**"). Sie werden rot, sobald gebaut wird —
**und das ist ihr Zweck**: sie sagen dem Lauf, der es tut, dass er den Altbestand
mitzunehmen hat, statt dieselbe Erkennung zweimal stehen zu lassen. Ein Test, der nur den
Zielzustand kennt, wäre bis dahin dauerhaft rot und würde ignoriert (SWR-131).

## L-2026-08-17ay — Ein Lauf, der fertig ist und nicht bucht, hinterlässt keinen Abbruch

**Anlass (Sprint 18, Eröffnung).** Sprint 17 hatte seine Arbeit um 18:50 abgeschlossen und
committet, aber **bewusst kein `ende` gebucht**, weil eine zweite Session in denselben Repos
arbeitete. Beide Läufe endeten. Sprint 17 stand danach als **laufend** da.

Der von `beginne()` vorgesehene Weg hätte ihn beim zweiten Anlauf als **`abgebrochen: true`**
geschlossen — und das wäre **falsch gewesen**: Sprint 17 hat vier Sachtickets geschlossen und
SWR-144 bis SWR-147 hervorgebracht.

> **Das Register kennt „läuft" und „abgebrochen". Für „fertig, aber aus Rücksicht nicht
> gebucht" hat es kein Wort — und der automatische Weg wählt dann das falsche.**

Geschlossen wurde er deshalb mit `beende()` **ohne** Abbruchmarke und mit einer Notiz, die
Messung (Schreibspur unbewegt, alle 17 Repos clean), Zeitpunkt der Arbeit (18:50) und den
nachtragenden Lauf nennt.

**Regel:** Wer eine fremde Buchung nachträgt, trägt **nicht** die bequemste Etikette ein,
sondern die zutreffende — und schreibt in die Notiz, **woran** er es gemessen hat. Ein
`abgebrochen: true` an einem erfolgreichen Sprint ist eine falsche Angabe in der Buchhaltung,
und sie ist später von einem echten Abbruch nicht mehr zu unterscheiden.

⚠ Offen und benannt: dem Register fehlt ein Feld für „von wem nachgetragen" — heute steht es
in der Prosa der Notiz. Kandidat für `platform/T-0020`s Nachbarschaft.

## L-2026-08-17bd — Der Preflight räumt beim Start; was ihn stört, entsteht durch die Handlung, die er ermöglichen soll

**Anlass (Sprint 19, `platform/T-0021`).** Auf dem Mount ohne `unlink`-Recht (R7) hinterlässt
**jeder Commit** `.git/objects/**/tmp_obj_*`, die Git nicht mehr entfernen kann. Der
**nächste** Git-Aufruf im selben Repo scheitert daran. Gemessen, dreimal in Folge: `entsperre`
→ Buchung ok → **nächste Buchung gescheitert** → `entsperre` → ok → **nächste gescheitert**.

> **Ein Vorlauf, der einmal am Anfang räumt, schützt gegen den Zustand von gestern und nicht
> gegen den, den dieser Lauf gerade selbst erzeugt.**

⚠⚠ Die zweite Gefahr ist die unangenehmere. `setze_status` (SWR-139) nimmt den Wechsel bei
gescheiterter Buchung **byteweise zurück** — richtig, und hier zweimal korrekt geschehen.
Aber:

> **Ein korrekt zurückgenommener Wechsel ist von einem nie versuchten nicht zu unterscheiden.**

Ein Lauf, der die Ausnahme nicht liest, sieht ein Ticket, das er gerade auf `done` gesetzt
hat, wieder auf `in_review` stehen.

**Regel (Zwischenlösung, hat in Sprint 19 gewirkt):** vor **jedem** Commit räumen, nicht nur
beim Sessionstart. Das ist die **Umgehung** und nicht die Reparatur — welche der beiden
Reparaturen richtig ist, hängt an einer Messung, die `platform/T-0021` als erste Frage
stellt: ist `tmp_obj_*` eine Sperre oder nur Müll, den `verbuche` fälschlich als Fehlschlag
liest? Die Meldungen kamen als `warning:` und der Exit-Code war trotzdem ungleich 0.

## L-2026-08-20bi — Auf diesem Mount ist Umbenennen erlaubt und Löschen nicht; deshalb hinterlässt der LESENDE Git-Aufruf die Sperre, nicht der schreibende

**Anlass (Sprint 21, `platform/T-0021` Frage 1).** Die erste DoD des Tickets war eine
Messung: Ist `objects/**/tmp_obj_*` eine Sperre oder Müll? Gemessen an den eigenen
Commits dieses Laufs:

* `tmp_obj_*` → **Warnung, Exit 0.** Der Commit läuft durch. **Müll, keine Sperre.**
* `.git/index.lock`, das nicht entfernt werden konnte → **Exit 128** beim nächsten
  Aufruf, `fatal: Unable to create '…index.lock': File exists.`

Und die eigentliche Ursache, gemessen über je einen Aufruf mit Vorab-Räumung:

| Aufruf | Sperre bleibt liegen |
|---|---|
| `git status --porcelain` | **JA** |
| `git add`, `git commit` | nein |
| `git log`, `git diff`, `git ls-files` | nein |
| `git reset --hard` | **JA** (Exit 128) |

> **Git beendet einen SCHREIBENDEN Indexvorgang, indem es `index.lock` über `index`
> umbenennt — Umbenennen ist erlaubt. Einen bloß LESENDEN Refresh beendet es, indem es
> die Sperre löscht — Löschen ist nicht erlaubt. Der harmlose Lesevorgang hinterlässt
> die Sperre, an der der nächste Aufruf stirbt.**

⚠ Das erklärt, warum der Preflight das Problem nicht lösen kann: **er selbst ist ein
lesender Aufruf** und hinterlässt die Sperre für den, dem er den Weg frei machen sollte.

**Regel:** Auf einem Mount ohne `unlink`-Recht wird **vor** jedem Git-Aufruf geräumt und
nicht danach — und die Vermutung über die Ursache eines Werkzeugfehlers wird gemessen,
bevor sie repariert wird. Die Vermutung im Ticketkopf war drei Sprints lang die falsche.

## L-2026-08-20bl — Ein Teilerfolg im selben Ticket macht die Lücke daneben unsichtbar

**Anlass (Nachprüfung des Abschlusses von Sprint 21).** `platform/T-0021` wurde von
`geplant_sprint: 20` auf `22` gezogen, und der Abschlussbericht behauptete an **zwei**
Stellen „Grund im Ticket". Im Ticket stand keiner.

Warum ausgerechnet dort: Sprint 21 hat in dasselbe Ticket einen langen Abschnitt
geschrieben — *„✅ Sprint 21: FRAGE 1 IST BEANTWORTET"*. Wer die Datei aufschlug, sah
einen Sprint-21-Abschnitt und hakte ab.

> **Ein Ticket, in dem in diesem Sprint etwas Gutes passiert ist, sieht bearbeitet aus.
> Die Verschiebung daneben verschwindet hinter dem Teilerfolg — nicht weil sie versteckt
> wäre, sondern weil das Auge die Überschrift für die Antwort hält.**

⚠ Es ist wörtlich `L-2026-08-17ag`, verletzt von dem Lauf, der die Regel im eigenen Plan
zitiert — und der **vierte** Beleg für **Frage 3 von `platform/T-0020`**: der
Abschlussbericht hat für seine eigenen Aussagen keine Zusicherung. Im selben Lauf gefunden
wurden außerdem drei **geschätzte statt gezählte** Testzahlen in den Anforderungszeilen
SWR-153/154/155 (10/17/10 statt 12/20/9) — derselbe Fehlertyp eine Etage tiefer.

**Regel:** „Grund im Ticket" wird **an der Verschiebung** geprüft, nicht am Vorhandensein
eines Sprint-Abschnitts. Und eine Zahl in einem Nachweistext („N Tests") wird gezählt,
nicht geschrieben — sie steht neben dem Nachweis, der sie widerlegen könnte.

## L-2026-08-20bm — Zwei Konstanten, die sich nie begegnen, widersprechen sich jahrelang leise

*Anlass: Sprint 22, `platform/T-0025` (SWR-156), beim Bau der Pausenmessung.*

`session.TAKT_MINUTEN` stand auf **30**. `sprint_register.TAKT_MIN_STANDARD` stand auf
**60**, und jeder Sprinteintrag im Register trägt `takt_min: 60` seit dem 17.08. Beide
Zahlen beschreiben denselben Sachverhalt: wie oft die Routine läuft.

Die Folge war keine Fehlermeldung, sondern eine **falsche Aussage**: die Kachel „Letzte
Session" meldete Stille nach 2x30 statt nach 2x60 Minuten. Sie hat es drei Tage lang
getan, und die einzige Zeit, in der sie damit hätte auffallen können, war ausgerechnet
der 60-Stunden-Ausfall — in dem sie sachlich recht behielt.

**Regel 1 — B033 hat eine leise Gestalt.** Der bekannte Fall sind *zwei Anzeigen, die
sich widersprechen*; man sieht ihn, weil beide gleichzeitig dastehen. Der leise Fall
sind **zwei Konstanten in zwei Modulen**, die einander nie begegnen. Nichts wird rot,
niemand vergleicht sie, und beide sehen für sich plausibel aus.

**Regel 2 — wo eine Tatsache mitgeschrieben wird, ist die mitgeschriebene die Quelle.**
Das Register schreibt `takt_min` bei **jedem** Lauf. Eine Konstante im Quelltext daneben
ist eine Annahme über die Vergangenheit; sie kann nur veralten. Sie darf **Rückfall**
sein und heißt dann auch so.

**Regel 3 — der Test ist die dritte Kopie und fällt zuletzt auf.**
`test_session_kachel` schrieb `minutes=95` fest und nannte es „zwei Takte". Er war grün,
solange der Irrtum galt, und wurde in dem Moment rot, in dem die beiden anderen Kopien
in Einklang kamen.
> **Ein Test, der eine Zahl festschreibt, die anderswo eine Tatsache ist, hält nicht
> den Code fest, sondern den Irrtum.**
Er ist deshalb nicht auf 121 gesetzt worden, sondern rechnet aus dem geltenden Takt.

## L-2026-08-20bn — Eine negative Zeitdifferenz ist kein hässlicher Wert, sondern der einzige Beleg dafür, dass zwei Uhren uneinig waren

*Anlass: Sprint 22, `platform/T-0025` Frage 1; verbucht als `platform/T-0026`.*

Beim Messen aller Pausen im Sprintregister war **eine von sieben negativ**: Sprint 17
nennt einen Start (16:49) vor dem Ende von Sprint 16 (17:10). Die **Reihenfolge der
Ereignisse** in der append-only-Datei ist dabei einwandfrei — die `ende`-Zeile steht
über der `start`-Zeile, und `beginne()` hat korrekt gearbeitet.

> **Wenn die Reihenfolge stimmt und die Uhrzeiten sich widersprechen, dann stammen die
> Uhrzeiten nicht aus derselben Uhr.**

**Regel 1 — `max(0, x)` ist die teuerste Zeile, die man hier schreiben kann.** Der
negative Wert ist der **einzige** Beleg dafür, dass es das Problem gibt. Wer ihn kappt,
bekommt eine glatte Anzeige und verliert den Befund; das ist B027/B038 in einer Zeile.

**Regel 2 — die Ursache wird gemessen und nicht geraten.** Drei Erklärungen sind
möglich (verschiedene Hosts, Zeitzonenverschiebung, nachträglich geschriebenes `ende`)
und führen zu verschiedenen Antworten. Der Fall trat **einmal in 22 Sprints** auf: das
rechtfertigt eine Messung und keine Umstellung der Zeitrechnung.

**Regel 3 — jede Aussage, die aus Differenzen dieser Stempel entsteht, ruht auf einer
Annahme, die einmal messbar verletzt war.** Betroffen sind vier Stellen (SWR-156,
SWR-153, SWR-106, die Taktmessung). Keine ist dadurch falsch — aber die Annahme stand
bis zu diesem Lauf nirgends geschrieben.

## L-2026-08-20br — Wenn die Reihenfolge stimmt und die Uhrzeit nicht, entscheidet der Commit, welcher der beiden Werte falsch ist

*Anlass: Sprint 23, `platform/T-0026` → SWR-159.*

`L-2026-08-20bn` hat festgestellt, **dass** zwei Uhren uneinig waren. Dieser Lauf hat
gemessen, **welche**: Registerzeit gegen die Commit-Zeit des Commits, der die Zeile
brachte, über alle 31 Ereignisse. Sechs reguläre `ende` liegen bei **+0,6 bis +1,1
Minuten**, der dokumentierte Nachtrag bei **+21,3**, und **eines bei −37,4**.

> **Ein Prozess kann nicht 38 Minuten vor seiner eigenen Uhr liegen. Der Commit ist der
> Zeuge, den man hat, wenn zwei Uhren sich widersprechen — nicht weil er richtiger geht,
> sondern weil er beweisbar SPÄTER ist als das, was er trägt.**

**Regel 1 — verdächtige zuerst den früheren Wert, nicht den auffälligen.** Das Ticket
verdächtigte den *Start* von Sprint 17 (16:49), weil er zu früh aussah. Falsch ist das
*Ende* von Sprint 16 (17:10), weil sein Commit um 16:32:36 liegt. Die auffällige Zahl
und die falsche Zahl sind nicht dieselbe.

**Regel 2 — eine Hypothese ist ausgeschlossen, sobald ihr Vorzeichen nicht passt.** Ein
nachträglich geschriebenes `ende` liefert einen **positiven** Abstand — genau so sieht
der belegte Nachtrag aus. Eine Zeitzone liefert ein Vielfaches von 15 Minuten, eine
Sommerzeitumstellung genau 60. **37,4 ist keines von beidem.** Zwei der drei Erklärungen
fallen damit ohne weitere Messung.

**Regel 3 — die vorgeschlagene Abhilfe muss den belegten Fall FINDEN, sonst ist sie
keine.** Das Ticket schlug vor, das Register künftig **mit Offset** zu schreiben. Beide
Commits des Fensters tragen `+02:00`; der Offset hätte **nichts** gemeldet. Ein
Formatwechsel an einer append-only-Datei mit 30 Zeilen Altbestand ist dafür ein hoher
Preis (B038).

**Regel 4 — was nicht entschieden werden kann, wird nicht behauptet.** *Welche* der
beiden Uhren richtig ging, ist aus dem Bestand nicht zu klären: die einzigen zwei Zeugen
sind die streitenden Uhren. Gesucht wurde ein dritter (Run-Registry, Telemetrie,
Zeitstempel in Dateiinhalten des Fensters) — es gibt keinen.

## L-2026-08-20bs — Ein Kommentar, der beschreibt, was der Code tun soll, ist keine Zusicherung

*Anlass: Sprint 23, SWR-162 — die Übergangsprüfung sah das Sammel-Repo seit Sprint 9 nicht.*

`uebergang_historie` war auf **zwei** voneinander unabhängigen Wegen blind für
`projects` (p10, p11, p12), und beide sahen für sich harmlos aus:

1. `pruefe_alle` übersprang jedes Projekt ohne eigenes `.git` — **während der Kommentar
   direkt daneben sagte, dass dann das Sammel-Repo zählt.**
2. `status_wechsel` filterte `git log -- tickets/`, und das ist **relativ zur
   Repo-Wurzel**; im Sammel-Repo liegen die Tickets eine Ebene tiefer.

**66 Statuswechsel sind seit SWR-118 nie geprüft worden.** Darin verborgen: vier
Altfälle und ein neuer.

> **Der Kommentar hat drei Sprints lang das Gegenteil dessen behauptet, was danebenstand,
> und niemand hat die beiden verglichen. Ein Kommentar altert nicht mit — er wird nur
> irgendwann unwahr.**

**Regel 1 — wo ein Kommentar eine Regel formuliert, gehört ein Test daneben.** Der Satz
„Projekte im Sammel-Repo teilen sich dessen .git — dann zählt das Sammel-Repo" ist eine
prüfbare Aussage. Er war falsch, und es war kostenlos, das nicht zu bemerken.

**Regel 2 — zwei unabhängige Ursachen für denselben blinden Fleck sind der Normalfall,
nicht die Ausnahme.** Wer nur eine repariert, hat den Befund verschoben und nicht
behoben. Deshalb: nach der ersten Ursache **weitermessen**, bis der Gegenstand wirklich
auftaucht.

**Regel 3 — ein Pfadfilter braucht eine ausdrückliche Tiefenzusicherung.** `tickets/`
und `*/tickets/` sind beides Präfixe; erst `:(glob)**/tickets/*` sagt „in jeder Tiefe".
Die nächste Verschachtelungsebene wäre sonst wieder unsichtbar.

**Regel 4 — eine Prüfung, die zwei Drittel des Bestands prüft, ist nicht zu zwei
Dritteln gut, sie ist grün.** Der korrigierte Filter kostet über alle 17 Repos rund
**36 s statt 10 s**. Der Preis ist gemessen, genannt und bezahlt.

---

## L-2026-08-20bx — Eine Reparatur gegen den Fehlerpfad kann wirkungslos sein, während sie drei Sprints lang richtig aussieht

**Anlass (Sprint 24, `platform/T-0021`, vierte Berührung).** Seit SWR-134 räumt der
Schreibweg die Git-Sperre weg, **nachdem ein Versuch gescheitert ist**. Das ist richtig
gebaut, geprüft und hat mehrfach geholfen. Die Messung aus Sprint 21 zeigt trotzdem, dass
es den eigentlichen Fall **nicht berührt**:

| Aufruf | Exit | Sperre bleibt liegen |
|---|---|---|
| `git status --porcelain` | **0** | **JA** |
| `git add`, `git commit`, `git log`, `git diff` | 0 | nein |

> **Umbenennen ist auf diesem Mount erlaubt, Löschen nicht. Git beendet einen SCHREIBENDEN
> Indexvorgang durch Umbenennen — das geht durch. Einen bloß LESENDEN Refresh beendet es
> durch LÖSCHEN — und das scheitert. Der harmlose Lesevorgang hinterlässt die Sperre, an
> der der nächste Aufruf stirbt.**

**Regel 1 — wer nur den Fehlerpfad absichert, verlegt die Kosten auf den nächsten
Aufrufer.** Der Rückfall repariert den Aufruf, der gescheitert ist. Der Aufruf, der die
Sperre erzeugt, **gelingt** — und merkt nichts. Der Nächste sieht eine Fehlermeldung, die
nach einem fremden Prozess aussieht und der Rückstand des eigenen vorigen Laufs ist.

**Regel 2 — bei einem Werkzeugfehler ist die erste Frage nicht *warum ist es
gescheitert*, sondern *wer hat den Zustand erzeugt, an dem es gescheitert ist*.** Diese
beiden Fragen haben hier zwei verschiedene Antworten und drei Sprints Abstand.

**Regel 3 — ein Ticketkopf ist eine Vermutung und wird mitgemessen.** Dieses Ticket heißt
nach `tmp_obj`-Resten. Die sind **Müll und keine Sperre**: ein Commit lief mit fünf
`unlink`-Warnungen und Exit 0 durch. Seine **Frage 2** („nach dem Commit räumen?") war
dadurch **falsch gestellt** — nach dem Commit ist nichts zu räumen.

---

## L-2026-08-20by — Ein Wächter gegen die Rückkehr muss ein Paar sein, sonst ist er nach einem Kahlschlag ebenfalls grün

**Anlass (Sprint 24, `projects/p11/T-0015`).** Der Rückbau entfernte `aggregation.dashboard`
und `KACHEL_FELDER`. Der naheliegende Wächter prüft, was **weg** ist. Er hat eine Lücke in
genau der Richtung, in der der Schaden liegt: `_zustand` und `zustaende_von` stehen in
derselben Datei, tragen dieselben Konstanten und sehen aus wie Dashboard-Code. Sie sind es
nicht — `zustaende_von` liefert seit SWR-146 den `zustaende`-Block des **Cockpits**.

> **Eine Prüfung, die nur Abwesenheit misst, ist nach einem Kahlschlag ebenfalls grün. Wer
> etwas entfernt, muss neben jedes „das ist weg" ein „und das ist noch da" schreiben.**

**Regel — jede Löschzusicherung bekommt eine Gegenprobe mit echter Auswertung.** Nicht nur
`hasattr(...)` ist wahr, sondern: `letzte_baseline: None` → `nicht_geliefert`,
`team.letzter_digest: ""` → `echte_null`. Ein Name kann stehenbleiben, während die Bedeutung
darunter weg ist.

**Nebenregel — zwei Orte, zwei Prüfungen.** Das Modul kann leer sein, während die Route noch
verdrahtet ist; dann stirbt sie erst beim Aufruf, und das ist der Leser und nicht der Test.

---

## L-2026-08-20cc — Der achte geschätzte Wert stand in einem Abschnitt mit der Überschrift „gezählt, nicht übersehen"

**Anlass (Sprint 24, Abschluss von `projects/p11/T-0015`).** Der Rückbau nannte den nicht
angefassten Rand mit einer Zahl: **9** betroffene JS-Zusicherungen. Sie war am Bildschirm
abgezählt. Gemessen — ein Durchlauf je Testblock nach `R.feldText` / `R.kachelFelder` /
`R.dashboardGruppen` — sind es **11**.

Es ist der **achte** Fall in sieben Sprints, in dem im Abschlussbericht eine
fortgeschriebene oder geschätzte statt einer gemessenen Zahl stand. Alle acht sind durch
**Nachzählen** gefunden worden und kein einziger durch eine Zusicherung.

> **Diesmal stand die falsche Zahl unter einer Überschrift, die das Gegenteil verspricht.
> Eine aufgeschriebene Lehre schließt diese Lücke nicht — dieser Lauf hatte sie beim
> Schreiben vor Augen, in einem Ticket, das er selbst dafür geöffnet hat.**

**Regel 1 — wo im Bericht eine Zahl steht, gehört der Einzeiler daneben, der sie erzeugt
hat.** Nicht „ich habe nachgesehen", sondern der Ausdruck. Er kostet eine Minute und ist
wiederholbar.

**Regel 2 — Überschriften wie „gemessen", „gezählt", „nachzählbar" sind eine Zusage und
kein Stil.** Wer sie schreibt, hat sich zur Messung verpflichtet; sie über eine Schätzung
zu setzen ist schlimmer als die Schätzung allein.

**Regel 3 — dieser Fall widerlegt einen naheliegenden Zuschnitt von `platform/T-0027`.**
Die dort genannten fünf Rubriken (Testzahl, Testdateizahl, Matrixgröße, Lückenzahl,
Briefkastenstand) hätten ihn **nicht** gefunden: die Zahl gehörte zu keiner von ihnen. Und
eine **Schablone** hätte ihn auch nicht gefunden — sie stand in Fließtext. Was ihn gefunden
hat, war ein Skript über die Datei.

---

## L-2026-08-20ce — Eine Umgebungsmessung ohne ihren ORT ist keine Auskunft (Sprint 25)

**Der Fall, dreimal in Folge und jedes Mal mit dem Wort „gemessen" davor.** Die Tickets
`team-mail/T-0001` und `promt-team/T-0003` standen seit Sprint 22 auf *„wartet auf die
Umgebung — erneut GEMESSEN, nicht angenommen"*: `MAIL_IMAP_*` = **0**, `which ollama` =
leer, `localhost:11434` ohne Antwort.

Alle Zahlen stimmen. Sie sind in der **Cowork-Sandbox** erhoben — einer Linux-VM, die den
Projektordner sieht und sonst nichts vom Rechner des Auftraggebers. Dort **kann** keine
dieser Zahlen anders ausfallen.

> **Die Messung hat ihr Ergebnis erzeugt. Eine Umgebungsmessung gilt für die Maschine, auf
> der sie lief — und in der Angabe stand nur die Zahl, nie der Ort.**

⚠⚠ **Der Gegenbeweis lag im selben Commit.** `70d5aa1` (20.08. 16:01:05) hat den
Tages-Digest vom 20.08. (26 Mails, lokales Ollama), 231 Zeilen IMAP-Rohdaten **und** den
Absatz „Gesetzte `MAIL_IMAP_*`-Variablen: 0" in einem Zug verbucht. Eine Sekunde später
ging derselbe Satz in den Sprintplan.

**Regel 1 — jede Messung über die Umgebung nennt den Ort, an dem sie lief.** „0 Variablen"
ist unvollständig; „0 Variablen in der Cowork-Sandbox" ist eine Auskunft.

**Regel 2 — ein Wartegrund, der bei jeder Messung gleich bleibt, weil die Messung ihn
erzeugt, ist kein Wartegrund**, sondern eine Eigenschaft des Messplatzes. Er gehört
widerlegt, nicht fortgeschrieben.

**Regel 3 — das Wort „gemessen" beendet keine Prüfung.** Es hat hier drei Sprints lang
genau das getan, wogegen es eingeführt wurde.

---

## L-2026-08-20cg — Ein Wächter, der auf Unbehebbares blockiert, ist ein Schalter (Sprint 25)

**Gemessen, nicht argumentiert.** Der Auto-Push-Wächter hat zuletzt am **17.08. 11:32:30**
gepusht und seither **83-mal hintereinander** abgebrochen — drei Tage, nichts auf GitHub.
Der Ollama-Schnelltakt, den der Auftraggeber selbst eingerichtet hatte, lief **6-mal** und
brach **alle 12** Ticks ab. Grund in beiden Fällen: `preflight` gab Exit 1 zurück, solange
irgendein Befund dastand — und vier Befunde waren Statusübergänge aus **abgeschlossenen**
Sprints, die niemand mehr reparieren kann.

> **Ein Wächter, der auf eine Tatsache blockiert, die niemand mehr ändern kann, ist kein
> Wächter mehr, sondern ein Schalter, den jemand umgelegt und niemand bemerkt hat.**

⚠⚠ **Die Regel dagegen stand im selben Modul und wurde dort zweimal angewandt** (Altbestand
vor dem Stichtag; Uhrenprobe-Altbestand), an der dritten Stelle nicht. Das ist B033 — nur
ist die vergessene Kopie diesmal **keine Codestelle, sondern eine Begründung**.

**Regel 1 — ein Befund blockiert den, der etwas dagegen tun kann.** Wer die Tatsache nicht
mehr ändern kann, bekommt sie **gemeldet**, nicht als Sperre.

**Regel 2 — ein Dauerbefund ist ein Betriebsrisiko und kein Gewissen.** Zwei Logdateien
haben drei Tage lang dasselbe geschrieben, und keine Session hat hineingesehen.

**Regel 3 — wer eine Sperre lockert, baut ein PAAR.** Neben „der Altfall kommt durch"
gehört „der frische kommt nicht durch". Ohne die zweite Hälfte besteht auch eine Fassung
jeden Test, in der gar nichts mehr blockiert.

**Regel 4 — Logdateien der Automatik gehören in den Startcheck einer Session.** Der Befund
war drei Tage lang lesbar und wurde nur gefunden, weil ein Brief des Auftraggebers danach
fragte.

---

## L-2026-08-20ci — Eine Lehre ohne Vertreter ist ein Zettel, kein Schutz (Sprint 26)

**Der Fall.** Die ersten drei Ticks, die je durch den Preflight kamen, starben alle an
`404: model 'llama3.1:8b' not found`. Genau dieser Fall stand seit dem **2026-08-06** als
`L-003` im Bestand — an **drei** Stellen abgelegt, mit der Gegenmaßnahme wörtlich benannt
(*„Modell-Defaults gegen das Geräteregister prüfen; Abweichungen als Registry-/Guardrails-CR
nachziehen"*) und an der dritten Stelle mit **Erwartungswert: Wiederholungsquote in Sprint
2 = 0**. Die Quote war **3 von 3**.

> **Es fehlte nicht die Sorgfalt beim Aufschreiben. Die Lehre war vorbildlich formuliert,
> dreifach abgelegt und mit einer Zahl versehen — und hatte vierzehn Tage lang exakt null
> Wirkung, weil der Satz, der ihren Vollzug trug, nie ein Ticket geworden ist.**

**Regel 1 — jede Lehre, die eine Gegenmaßnahme nennt, bekommt im selben Zug ein Ticket
oder eine Prüfung.** Ohne einen von beiden ist sie Text; Text wird nicht ausgeführt.

**Regel 2 — ein Erwartungswert ohne Messstelle ist eine Hoffnung.** „Wiederholungsquote =
0" war richtig gedacht und hat nichts gehalten, weil niemand die Quote je gemessen hat.
Wer eine Zahl als Ziel aufschreibt, schreibt dazu, **wer sie wann misst**.

**Regel 3 — dreifache Ablage ersetzt keinen Vertreter.** Dass dieselbe Lehre an drei
Stellen stand, hat sie nicht wirksamer gemacht, sondern nur schwerer widerlegbar. *Kopien
erhöhen die Sichtbarkeit, nicht die Wirkung.*

**Gegenprobe, die im selben Lauf gelungen ist.** `SWR-170` wurde mit der Ansage gebaut, sie
müsse am ersten Tag **2** melden. Sie meldet 2. *Eine Prüfung, deren erwarteter Wert vor
dem Bauen aufgeschrieben wird, prüft beim ersten Lauf sich selbst mit.*

---

## L-2026-08-20cj — Was ein Lauf hinterlässt, prüft niemand (Sprint 26)

**Der Fall.** Der Tick prüft **vor** der Arbeit gründlich (saubere Arbeitskopie, `p0/T-0014`
aus Sprint 1) und **nach** der Arbeit gar nicht. Um 20:15 misslang die Rückkehr auf `main`
— stillschweigend, weil sie mit `fehler_ok=True` dastand. Der Ergebnis-Commit landete auf
dem Feature-Branch, `main` behielt `status: in_progress`, der Arbeitsbaum zeigte `open`.

> **⚠⚠ Und deshalb war der Preflight blind für genau das, wofür er gebaut ist: `SWR-155`
> liest den ARBEITSBAUM und meldete „In Arbeit liegengeblieben: 0" für ein Ticket, das auf
> `main` seit einer Viertelstunde auf `in_progress` stand.**

**Regel 1 — eine Prüfung, die den Arbeitsbaum liest, prüft den Branch, auf dem sie zufällig
steht.** Solange niemand den Branch wechselt, ist das dasselbe — und genau deshalb fällt es
nicht auf, wenn es einmal nicht dasselbe ist.

**Regel 2 — `fehler_ok=True` an einem Rückweg ist keine Toleranz, sondern ein blinder
Fleck.** Wo ein Fehlschlag folgenlos bleiben darf, muss er trotzdem **genannt** werden.

**Regel 3 — ein Vorgang endet dort, wo er begonnen hat, und das wird nachgemessen.** Nicht
„wir kehren zurück", sondern „wir stehen wieder da" — mit einer Abfrage, nicht mit einem
Aufruf.

**Regel 4 — ein `return` im `finally` verschluckt eine fliegende Ausnahme.** Der naheliegende
Bau hätte einen Absturz des Gateways wie ein ordentliches Ende aussehen lassen. Befund
merken, **nach** dem `finally` werten.

**Regel 5 (Nachtrag Sprint 26) — eine Gegenprobe, die die Funktion prüft und nicht ihren
Aufrufer, misst die Hälfte, die man selbst geschrieben hat.** `SWR-169` wurde mit vier
gefahrenen Gegenproben abgenommen — alle über die Auflösungsfunktion, keine über die
Argumente, mit denen sie im Betrieb gerufen wird. Ein Lauf um 21:30, den niemand geplant
hatte, zeigte: die Funktion bekommt Rolle und Einheit zur Laufzeit nie, weil das Startskript
nur die Einheit übergibt. **Die Anforderung war grün, die Wirkung null.** Wer eine
Auflösung baut, prüft mindestens einmal, **womit sie aufgerufen wird** — sonst ist der
Nachweis ein Selbstgespräch. `platform/T-0033`.

---

## L-2026-08-20cm — Eine Mutationsprobe ohne Nachweis, dass die Mutation wirkte, misst den Zustand von vorhin

**Sprint 27, `platform/T-0033`.** Zur Abnahme von `SWR-171` sollte die Gegenprobe zeigen,
dass eine **ausgeschaltete** Prüfung Tests rot macht. Der erste Durchgang meldete **grün**.

Das war kein Testfehler, sondern ein Messfehler: die Mutation stand zum Testzeitpunkt noch
nicht auf der Platte (der Mount schreibt verzögert), und ein alter `__pycache__` lag
daneben. Der zweite Durchgang — Mutation **nachgelesen**, Cache geleert — zeigte die zwei
roten Zusicherungen.

> **⚠⚠ Grün ist zweideutig: es kann heißen „die Prüfung greift nicht" oder „die Mutation
> ist nicht angekommen". Ohne den Nachweis, dass die Mutation wirklich im Code steht, ist
> die Gegenprobe eine Messung des vorigen Zustands — und sie hätte in genau dem Bericht
> als „Gegenprobe bestanden" gestanden, der aus einer Gegenprobe entstanden ist, die die
> falsche Hälfte gemessen hat.**

**Regel.** Eine Mutationsprobe hat drei Schritte, nicht zwei: mutieren, **die Mutation im
Code nachlesen und die Bytecode-Caches leeren**, dann testen. Und sie ist erst vollständig,
wenn auch die **Rücknahme** wieder grün ist — sonst bleibt eine Mutation im Bestand stehen,
wie es in diesem Lauf beinahe geschehen wäre.

---

## L-2026-08-20cn — Ein Statuswechsel `open -> done` in einem Commit, und die Prüfung stand daneben statt davor

**Sprint 27, beim Schließen von `platform/T-0033`.** Der Abschluss schrieb `status: done`
direkt auf ein `open`-Ticket. `board.py` hat das korrekt abgelehnt — **nach** dem Schreiben
und **neben** dem Commit: die Kette in der Konsole war `board.py …; git commit`, und das
Semikolon lässt den Commit auch nach einer abgelehnten Validierung laufen. Der unzulässige
Übergang stand damit in der Historie.

Repariert durch den richtigen Weg — `open → in_progress → in_review → done`, je ein
Commit, wie ihn Sprint 26 an `T-0031` gegangen ist. Der Fehlcommit war lokal und ungepusht
und ist zurückgenommen; **das steht hier, weil es sonst stillschweigend geschähe.**

> **Eine Prüfung, die neben dem Schreibvorgang läuft statt vor ihm, ist eine Meinung.
> Genau das ist die Frage 3 von `platform/T-0027`, hier an der eigenen Konsole
> vorgeführt: läuft die Prüfung vor oder nach dem Zeitpunkt, an dem der Fehler Schaden
> anrichtet?**

**Regel.** Ticketstatus wird über `board.py --status` bzw. den Weg gesetzt, der die
Übergangsmatrix **vor** dem Schreiben anwendet — nie durch direktes Ersetzen im
Frontmatter. Wo doch von Hand geschrieben wird, gilt: `board.py` muss **grün** sein, und
die Verkettung ist `&&`, nicht `;`.

---

## L-2026-08-20co — Der Mount hinterlässt Sperren, die auch `git reset` blockieren

**Sprint 27.** `git reset --soft HEAD~1` scheiterte an einer `HEAD.lock`, die ein früherer
Aufruf liegengelassen hatte, und ein direkter `git commit` danach ebenso. Was durchlief,
war der **eine Schreibweg** dieses Hauses (`backend/git_schreiben.ruf`, `SWR-134/163`) —
er räumt die Sperre, die der eigene gelingende Aufruf hinterlässt.

> **Die Reparatur lag seit Sprint 5 im Haus und wurde in diesem Lauf zweimal umgangen,
> weil `git` aus der Konsole schneller getippt ist als der eigene Weg.**

**Regel.** Git-Aufrufe dieses Hauses laufen über `git_schreiben.ruf` — auch die seltenen
(`reset`, `show`, `log`). Ein direkter `git`-Aufruf auf diesem Mount ist ein Aufruf, der
beim nächsten Mal jemand anderen blockiert.

---

## L-2026-08-21ck — Der Mangel war ein Präfix des Nummernraums, keine Eigenschaft des Korpus

**Sprint 30, `SWR-197` / `platform/T-0047`.** `T-0036` hatte **1003** praefixlose
Entscheidungs-Zitate gezählt und selbst gewarnt, *„die 1003 sind nicht 1003 Probleme"*.
Die ehrliche Untermenge ist gemessen: **743 (73 %)** stehen im besitzenden Repo, **65
(6 %)** nennen eine nur einmal vergebene ID, **214 (21 %)** sind echt mehrdeutig — und
**alle 214 nennen eine von vierzehn IDs (`D000`–`D013`)**. Ab `D014` ist jede ID
organisationsweit einmal vergeben.

> **Damit ist nicht der Korpus krank, sondern ein Anfangsstück des Nummernraums. Gebaut
> wird an der Vergabe: eine Sperrklinke hält die mehrdeutige Menge bei vierzehn. Der
> einzige Weg, auf dem der Schaden wächst, ist ein Repo, das seine Entscheidungen wieder
> bei `D001` zu zählen beginnt — und eine Zeile ist billig zu fangen, zweihundert
> Zitatstellen sind es nicht.**

⚠⚠ Das entscheidende Argument gegen den ursprünglich geplanten Zuschnitt ist **an dieser
Session selbst gemessen**: während des Baus stiegen die Zahlen von 1023/214 auf 1030/216 —
allein dadurch, dass Ticket und Anforderung **über** das Problem schrieben und dabei IDs
nannten. **Eine Prüfung auf die Zitatzahl hätte jeden Bericht bestraft, der den Befund
erklärt: ein Dauerbefund, den das Erklären selbst füttert.**

⚠ Ein Grenzfall, den erst eine Zusicherung fand: `D001` ist mehrfach vergeben, aber ein
`D001` **in** `pm` ist trotzdem nicht mehrdeutig — die naheliegende Lesart ist die eigene.
**Eine ID ist nicht mehrdeutig, weil sie doppelt vergeben ist, sondern weil sie an ihrer
Fundstelle keine naheliegende Lesart hat.**

**Regel:** Vor einem Korpus-Umbau wird die schädliche Untermenge gemessen und nach ihrer
**Ursache** gruppiert, nicht nach ihrer Fundstelle. Trägt die Ursache eine endliche Menge
(hier: 14 IDs), sichert man diese Menge — und niemals den Korpus, der über sie berichtet.

## L-2026-08-21co

**Die Frage hat ihre eigene Antwort überlebt: sie wurde dreimal weitergereicht, während
zwei andere Tickets sie nebenbei beantwortet haben — und niemand hat die Weiterreichung
daraufhin geprüft, weil eine Verschiebung nur ihren Grund braucht und nicht ihre
Gültigkeit.**

`platform/T-0049` fragte seit Sprint 27 nach einer Nummernvergabe für den „zweiten
Schreibweg" ins Entscheidungslog. Bei der vierten Berührung ergab die Messung: es gibt
**keine zweite Funktion** — der zweite Weg ist die **Hand**, und er ist mit 103 von 158
Zeilen (65 %) die **Mehrheit**. Der Schaden, den eine Vergabe verhindert hätte, war
längst von `SWR-195` (Sprint 29) und `SWR-197` (Sprint 30) gefangen, also von Tickets,
die **nach** der Frage entstanden sind.

⚠ Nebenbefund derselben Messung: die Herkunftsspalte des Logs ist die Aussage dessen, der
schreibt, über sich selbst — eine Session hat eine Inbox-Entscheidung zwei Minuten später
nachgeschrieben und dabei weiter *„via Inbox"* behauptet. **Eine Provenienz, die der
Beschriebene selbst einträgt, trennt nichts.**

**Regel:** Bei jeder Terminierung eines weitergereichten Tickets wird nicht nur der Grund
der Verschiebung geprüft, sondern die **Gültigkeit der Frage**: Was ist seit der letzten
Berührung gebaut worden, das sie beantwortet oder erübrigt?

*Verbleib: Rollenkarte CM · `platform/tests/test_entscheidungslog_schreibwege.py` ·
Historie `platform`.*

## L-2026-08-21cr

**Regel:** Eine Festlegung, die nur in einem Docstring wohnt, wird von der nächsten
Werkzeuggeneration nicht gebrochen, sondern **übersehen**. Jede Zählregel, die mehr als
ein Werkzeug betrifft, bekommt eine Zusicherung, die **alle Erzeuger gegeneinander** hält
— und die Konstante wird im **Code** zitiert, nicht nur im Modul geführt.

`SWR-113` hat in Sprint 7 festgelegt, was „offen" heißt. Zwanzig Sprints später zählte
`kennzahlen.py` **9**, während `sprint.kennzahlen` und `aggregation.uebersicht` **12**
zählten. Nicht aus Widerspruch — es hat die Festlegung nur niemand vertreten.

⚠⚠ Die Messung hat die Frage umgestellt: gefragt war „welche von **zwei** Zählweisen
bleibt", gezählt sind **drei** Erzeuger, und **zwei von dreien folgten der Festlegung
bereits**. Ausgerechnet die beiden, die sich den **Namen** `tickets_offen` teilten, waren
die, die sich widersprachen. Es gab also nie zwei berechtigte Größen unter einem Namen,
sondern **eine** Größe mit einem abweichenden Erzeuger — dem jüngsten.

⚠ Der Vertreter dieser Lehre hatte im ersten Entwurf **den Fehler, den er prüft**: er
prüfte die **Anwesenheit** der Konstante und blieb grün, während der Nachbar drei Zeilen
darüber weiter das Literal benutzte. Eine Konstante, die dasteht und nicht gerufen wird,
ist so viel wert wie der Docstring, an dem der Befund hängt.

**Verbleib:** Rollenkarte CM (`process/roles/cm.md`), Vertreter
`platform/tests/test_offen_zaehlweise.py` (6 Zusicherungen, am echten Bestand).

## L-2026-08-21cu

**Regel:** Eine Sperrklinke gegen falsche Nummernvergabe gehört **in** die Vergabe, nicht
neben sie. Eine Prüfung, die den Verstoß meldet, nachdem er in einem append-only Log
steht, kann ihn nie mehr rückgängig machen.

`SWR-197` hat in Sprint 30 gemessen, dass alle mehrdeutigen Entscheidungs-Zitate aus
`D000`–`D013` stammen, und geschrieben, die Sperrklinke sei *„an der Vergabe"* gebaut.
`inbox._naechste_d_id` bildete aber weiterhin `max + 1` über **ein** Log.

> **⚠⚠ Die ersten drei Vergaben nach dieser Sperrklinke haben sie gebrochen.** Am
> 2026-08-21 beantwortete der Auftraggeber drei Anfragen; `pm` stand bei `D013`, also
> entstanden `D014`, `D015`, `D016` — die es in `p0` seit dem 06.08. gab. Die mehrdeutige
> Menge wuchs an **einem Tag von 14 auf 17**.

> **Eine Prüfung, die neben der Vergabe steht und sie nicht anfasst, ist kein Riegel,
> sondern ein Zeuge.**

⚠ Der Altbestand wird ehrlich auf 17 fortgeschrieben (append-only), **aber ausdrücklich
nur zusammen mit der Reparatur der Vergabe**. Das Fortschreiben allein wäre die bequeme
Handlung gewesen — die Bauart, gegen die `SWR-166` gebaut wurde.

**Verbleib:** Rollenkarte CM, Vertreter `platform/tests/test_d_id_vergabe.py`
(`SWR-203`, organisationsweite Vergabe + Rückbau-Wächter).


## L-2026-08-21dc

**Regel:** Eine **Datenklasse** muss den Gegenstand **platzieren**, nicht beschriften. Und
sie gehört als **Feld** in die Datei, nie in einen Kommentar — jeder Leser dieses Hauses
schneidet Zeilen bei `#` ab.

`projekt_setup.py` nahm `--datenklasse sensibel` entgegen, **prüfte** den Wert gegen eine
Liste, schrieb ihn als `#`-Kommentar in den Steckbrief — und legte das Projekt unverändert
unter `projects/` an, in einem Repo **mit** GitHub-Remote, das `abschluss.cmd` bei jedem
Lauf pusht. Aufgefallen ist es beim Gründen von `team-termine`, das den Kalender eines
**fremden Kontos** verarbeitet.

> **⚠⚠ Eine Datenklasse, die nur beschriftet und nicht platziert, ist keine Schranke,
> sondern eine Aufschrift — und sie stand an der einzigen Stelle, die kein Werkzeug
> dieses Hauses liest.**

⚠ Die zweite Hälfte war schlimmer: `organigramm.sammle` fiel für `datenklasse` **fest auf
`"intern"`** zurück, wenn eine Einheit nicht in der Team-Registry steht — und dort steht
kein Projekt. **Ein fester Rückfall auf einen der beiden möglichen Werte macht aus „nicht
nachgesehen" eine Auskunft, und hier bedeutet der erfundene Wert „darf nach außen".**

⚠ Der Gegenbeleg stand die ganze Zeit daneben: `pool.gruendung_vorlegen`, der Weg der
**Team**-Gründung, sagt dasselbe Wort korrekt an. Zwei Gründungswege, ein Begriff, zwei
Bedeutungen — die B033-Familie mit einem *Gründungsweg* als vergessener Kopie.

**Verbleib:** Rollenkarte CM, Vertreter
`platform/tests/test_datenklasse_platziert.py` (7 Zusicherungen, `SWR-208`).


## L-2026-08-21df

**Regel:** Eine erteilte Abnahme ist erst vollzogen, wenn ihr **Artefakt** existiert.
Entscheidung und Baseline sind zwei Schritte — und der zweite braucht eine Prüfung, sonst
merkt ihn niemand.

`p12/D003` hat am 17.08. um 21:57 die Baseline `p12-v1.0` abgenommen (Option A auf
`p12/T-0010`, *„DR/G4: Baseline p12-v1.0 abnehmen"*). Der Tag wurde nie gesetzt.

> **⚠⚠ Vier Tage lang war ein Projekt abgenommen, ohne dass es einen abgenommenen Stand
> gab. Der Steckbrief sagte „abgeschlossen", die Entscheidung stand im Log — und der
> Gegenstand, auf den sich beide beriefen, existierte nicht.**

⚠ Ein nachträglich gesetzter Tag gehört auf den **Abschluss-Commit des damaligen Laufs**
und nicht auf HEAD: ein Tag auf heute behauptet, der heutige Stand sei abgenommen worden.

⚠ **Und die Ursachensuche selbst trägt eine Lehre:** das Ticket bot zwei plausible
Ursachen an (Ollama-Guardrail, stehender Push). Beide waren falsch, und beide klangen
richtig. **Eine Ursache, die man ausschließen kann, ist mehr wert als zwei, die man
plausibel findet.**

**Verbleib:** Rollenkarte CM, Vertreter
`platform/tests/test_baseline_folgt_der_abnahme.py` (5 Zusicherungen, `SWR-211`).

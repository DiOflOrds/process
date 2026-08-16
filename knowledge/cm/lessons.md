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

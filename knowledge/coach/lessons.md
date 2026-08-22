
---

## L-2026-08-20cp — „Gelernt ohne Vertreter": eine vorbildlich aufgeschriebene Lehre hat vierzehn Tage lang null Wirkung gehabt

**Sprint 27, `platform/T-0034` (abgetrennt von `platform/T-0027`).**

`L-003` vom **2026-08-06** nennt den Guardrails-Default `llama3.1:8b`, nennt `gemma3:27b`
als das installierte Modell und nennt die Gegenmaßnahme wörtlich: *„Modell-Defaults gegen
das Geräteregister prüfen; Abweichungen als Registry-/Guardrails-CR nachziehen."* Sie ist
an **drei** Stellen abgelegt und trägt an einer davon eine Zahl: *„Erwartungswert:
Wiederholungsquote dieser Fehlerbilder in Sprint 2 = 0."*

| | |
|---|---|
| Erwartete Wiederholungsquote | **0** |
| Gemessen in Sprint 26 | **3 von 3** |
| Zeit bis zum Wiedereintritt | **14 Tage** |

> **⚠⚠ Es fehlte NICHT die Sorgfalt beim Aufschreiben. Die Lehre ist vorbildlich
> formuliert, dreifach abgelegt und beziffert — und wirkungslos, weil der Satz, der ihren
> Vollzug trug, nie ein Ticket und nie eine Prüfung geworden ist.**

⚠ Die naheliegende Antwort — *„wir schreiben eine Regel, dass Lehren in Tickets überführt
werden"* — ist **genau die Bauform, die hier versagt hat**: die Lehre selbst war eine
solche Regel. Was fehlt, ist etwas, das **von allein wiederkommt**.

**Regel (bis `platform/T-0034` gebaut ist).** Eine Lehre, die eine Gegenmaßnahme in
Befehlsform enthält (*„… prüfen", „… nachziehen", „… ergänzen"*), bekommt **im selben
Lauf** ein Ticket oder eine Zusicherung — sonst ist sie eine Notiz und wird auch so
benannt. ⚠ Diese Regel hält nach der Messung dieses Hauses keine drei Sprints allein
(`SWR-125`); sie trägt bis zum Bau und ersetzt ihn nicht.

## L-2026-08-21cn

**Eine Zusicherung, die ein Verhältnis meint und eine Ungleichung schreibt, bleibt grün,
während ihr Gegenstand verschwindet.**

`SWR-194` nannte die Regel-Zeile *„die Konvention, mit der dieses Haus selbst schon
unterscheidet"*, und eine Zusicherung sollte genau das sichern: `len(mit_regel) <
len(alle)`. Gemessen tragen **111 von 112** Lehren eine Regel — die Ungleichung wäre
erfüllt gewesen, die Aussage war es nie.

⚠ Zwei Messungen haben in diesem Fall die Bauform bestimmt statt umgekehrt: die
Trefferquote der Vertreter liegt innerhalb (24 %) und außerhalb (15 %) der „ehrlichen
Untermenge" fast gleich — die Konvention trennte **Zeichensetzung, nicht Substanz**; und
zwischen „Muster erweitern" und „Filter weglassen" liegen **null** Lehren.

> **Von zwei gleichwertigen Bauformen ist die mit einem Begriff weniger die richtige.**

**Regel:** Eine Zusicherung über eine Auswahl prüft ihren **Anteil**, nicht ihre bloße
Existenz. Und wer eine Untermenge als „ehrlich" oder „gefunden" begründet, misst ihre
Trennschärfe, bevor er sie zur Grundmenge macht — sonst begründet er eine Schreibweise.

*Verbleib: Rollenkarte COACH · `platform/backend/lehren.py` ·
`platform/tests/test_lehren_vertreter.py` · Historie `platform`.*

⚠ **Nachtrag aus demselben Lauf, gefunden von der Prüfung selbst:** diese Lehre stand
kurzzeitig als „ohne Vertreter" da (92 statt 91), weil ihr Vertreter zuerst in
`test_lehren_vertreter.py` stand — und genau diese Datei ist aus dem Vertreter-Korpus
**ausgeschlossen** (gegen die Tautologie aus `SWR-194`).

> **Eine Lehre, die BEI der Vertreter-Prüfung gelernt wird, kann ihren Vertreter nicht IN
> ihr haben. Sie braucht eine zweite Stelle, an der dieselbe Regel wirklich trägt — und
> wenn es keine gibt, ist die Regel noch keine.**

Sie hat eine: `test_entscheidungslog_schreibwege.test_die_mehrheit_der_zeilen_stammt_
NICHT_aus_dem_code_weg` prüft einen **Anteil** mit Schwelle statt einer Existenz.


## L-2026-08-21dd

**Regel:** Ein Lehrbuch wird **angehängt**, nie geschrieben. Und eine Prüfung, die eine
Menge beobachtet, muss **Schrumpfen von Fortschritt unterscheiden können** — sonst meldet
sie beides beim Namen des angenehmeren Falls.

Der Abschluss-Commit von Sprint 32 (`process@a82f207`, Betreff *„Lehren cq-cv
verankert"*) hat `knowledge/cm/lessons.md` von **1931 auf 26 Zeilen** und
`knowledge/pl/lessons.md` von **871 auf 26** gekürzt: **91 Lehr-Abschnitte gelöscht, 2
hinzugefügt.**

> **⚠⚠ Ein Commit, der „verankert" im Betreff trägt, hat 91 Lehren entfernt — und die
> Prüfung, die das hätte finden müssen, meldete es als FORTSCHRITT: „Diese Lehre(n) haben
> einen Vertreter bekommen, bitte die Basis nachziehen." Ein Bestand kann verschwinden,
> während sein Wächter Erfolg meldet.**

⚠ Und die **Folgerung daraus war ebenfalls falsch**: `platform/T-0061` schloss in Sprint
33, die Lehren hätten *„nie in einem Lehrbuch gelebt"* und lebten nur als Zitat. Sie haben
dort gelebt, bis ein Commit sie überschrieb. **Zwei Sprints hintereinander hat dieselbe
Lücke eine falsche Erklärung getragen, weil niemand in die Git-Historie der Datei gesehen
hat.**

⚠ Der Nachweis der Wiederherstellung ist eine Zahl, die niemand gewählt hat:
`ohne_vertreter()` liefert wieder **exakt 91** — namentlich dieselbe Menge wie
`OHNE_VERTRETER_BASIS` aus Sprint 31, in beide Richtungen leer. **Die Basis hatte die
ganze Zeit recht.**

**Verbleib:** Rollenkarte COACH, Vertreter
`platform/tests/test_lehrbuch_verliert_nichts.py` (5 Zusicherungen, `SWR-209`) sowie die
geschärfte Meldung in `test_lehren_vertreter::test_ein_gewonnener_vertreter…`.

---

## L-2026-08-22t — Ein vollständiger Leser ohne Anlass hinzusehen ist dasselbe wie kein Leser

*Anlass: Sprint 40 (2026-08-22), `platform/T-0067`. **Vier Belege an einem Tag, und zwei
davon gehen auf das Konto des Laufs, der die Frage beantworten sollte.***

| # | Vorfall | Wo der Befund stand | Wer ihn nicht las |
|---|---|---|---|
| 1 | rote CI von `projects` seit dem 20.08. | `CI-STATUS.md`, im Arbeitsordner | drei Sprints |
| 2 | Mutationsprobe meldete dreimal „10 von 11 rot" | `ENOENT` mit vollem Pfad, **zehnmal wortgleich** | dieser Lauf |
| 3 | Ursache der sieben `index.lock` | `preflight.py` Z. 8–9 **und** alle 15 Min im Schnelltakt-Log | dieser Lauf — er schrieb ein Ticket über einen „unbenannten" Befund |
| 4 | Blockerliste `provider` (11 von 16) | `auswertung.bericht` | seit dem 20.08. |

Fall 4 ist der klarste, weil er sich zählen ließ: `telemetrie.blocker` →
`auswertung.rollen_aggregat` → `auswertung.bericht` → `scripts/telemetrie_report.py` →
**und dieses Skript ruft niemand auf.** 0 `.cmd`, 0 Takt, 0 CI, 0 Frontend.

> **Das ist kein fehlender Leser, sondern ein vollständiger Leser ohne Anlass, hinzusehen.**

**Regel:** Ein Befund gehört dorthin, wo der Lauf **ohnehin** hinsieht — nicht in einen
Bericht, den jemand holen muss, und **nicht unter eine Erfolgsmeldung**. Und: das
gefährlichste Ergebnis ist nicht das rote, sondern das **veraltete grüne** — eine rote CI
wird irgendwann gelesen, ein veraltetes Grün nie.

**Verbleib:** `SWR-230`, Vertreter `platform/tests/test_ci_gelesen.py` (10 Zusicherungen);
Zählbarkeit der Regel als `platform/T-0078`.


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

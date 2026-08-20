
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

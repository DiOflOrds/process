

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

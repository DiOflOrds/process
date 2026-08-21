

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

# Gold-Beispiel DEV: Schreibpfad gegen Architektur-Invariante (SWE.3)

**Zweck:** Referenz für Implementierung, die eine Architektur-Invariante (kein Zustand außerhalb Git, SWR-024) aktiv schützt. Quelle: realer Fall `backend/inbox.py` (T-0032, Sprint 3).

## Input

SWR-020: Entscheidung annehmen und ins Decision Log schreiben. Naiver Ansatz: Datei schreiben, fertig — hinterlässt aber eine dirty Arbeitskopie und blockiert Ticks (SWR-015).

## Referenz-Output (Muster)

Operation als Ganzes gedacht: validieren (404/400-Fälle zuerst) → alle Schreibziele erzeugen (Log-Zeile append-only, Ticket-Notiz, BOARD regeneriert) → **selektives** `git add` nur der eigenen drei Ziele (Lesson T-0014) → Commit mit sprechender Identität („Mensch via Inbox") und Ticket-ID → bei Commit-Fehler 503 und Operation gilt als nicht angenommen (kein halber Zustand). Jeder Fehlerfall hat einen Testfall mit SWR-Docstring-ID; ein Test prüft ausdrücklich `git status == sauber`.

## Warum Gold

Die Invariante wird nicht dokumentiert, sondern erzwungen und getestet; Fehlerpfade sind Teil des Vertrags; ADR-003 und Implementierung erzählen dieselbe Geschichte.

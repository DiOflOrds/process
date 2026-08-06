# Gold-Beispiel COACH: Prozess-CR aus Retro (SUP.10 / PIM.3)

**Zweck:** Referenz für vollständige Prozess-CRs mit messbarem Erwartungswert.

## Input

Retro-Befund: In 3 von 8 Reviews fehlte der benannte Reviewer im Ticket; Reviews wurden dadurch verspätet gestartet (Ø +1 Tick Verzug).

## Referenz-Output (CR-Ticket)

**Anlass:** Retro Sprint n, KPI „blocked-Zeit" + 3 Findings (verlinkt).
**Änderung:** board.py-Validierung: `in_review` ohne `reviewer`-Feld → Validierungsfehler (CI-blockierend). Rollenkarten-Regel: Reviewer wird bei Statuswechsel nach `in_review` gesetzt, nie nachträglich.
**Impact-Analyse:** betroffen: platform/scripts/board.py (DEV, klein), Rollenkarten (COACH, klein); keine Baseline betroffen; Kosten ~0 (Skript-Route).
**Erwartungswert (messbar):** Reviews ohne Reviewer: 0 im Folgesprint; Ø Review-Startverzug −1 Tick.
**Entscheidung:** PL (im Scope), Decision Log Eintrag.
**Verifikation:** Unit-Test in test_board.py + KPI-Vergleich in Retro n+1.

## Warum Gold

Datenbasierter Anlass; Änderung als versioniertes Artefakt (Skript statt Appell); messbarer Erwartungswert; Verifikation benannt.

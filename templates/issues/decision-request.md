<!-- Issue-Template: Decision Request (HITL) — Label: type::decision-request

T-0039: Optionen/Frist/Default zusätzlich MASCHINENLESBAR im Ticket-Frontmatter führen:
    optionen: [A1, A2, B1, B2]   # zulässige Options-Token; Kombination als "A1, B1"
    frist: JJJJ-MM-TT
    default: A1, B1              # nur bei risikoarmen DRs; Token müssen in optionen liegen
Die Inbox-API validiert die gewählte Option gegen `optionen` (ungültig -> HTTP 400,
kein Decision-Log-Eintrag); board.py prüft frist-Format und default-Token. -->

## Sachverhalt
<!-- Max. 5 Sätze: Was ist zu entscheiden und warum jetzt? -->

## Optionen

| # | Option | Konsequenzen (Aufwand, Risiko, ASPICE-Wirkung) |
|---|---|---|
| A |  |  |
| B |  |  |

## Empfehlung des Teams
<!-- Welche Option und warum (kurz begründet). -->

## Antwortfrist und Default
- **Frist:** <Datum/Uhrzeit>
- **Default bei Fristablauf:** <nur bei risikoarmen DRs zulässig; sonst: "kein Default — blockiert">

## Blockierte Tickets
<!-- Verlinkte Issues, die auf diese Entscheidung warten. -->

---
*Nach Antwort: Entscheidung ins Decision Log übertragen (p0/management/decisions/), blockierte Tickets entblocken.*

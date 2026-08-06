# Gold-Beispiel PL: Decision Request qualifizieren (MAN.3)

**Zweck:** Referenz für qualifizierte Mensch-Vorlagen (Playbook Kap. 7).

## Input

Situation: Backbone gitlab.com ist aus der Sandbox unerreichbar; Sprint blockiert. Alternativen: GitHub (Konto vorhanden), lokales Git ohne Remote, warten.

## Referenz-Output

**DR: Backbone-Wechsel wegen GitLab-Unerreichbarkeit**

Sachverhalt (5 Sätze): gitlab.com ist aus der Arbeitsumgebung dauerhaft nicht erreichbar; der Token wurde vorsorglich widerrufen. Ohne Backbone kein Push, keine Tickets, kein Fortschritt. GitHub ist erreichbar und beim User vorhanden. Ein Wechsel kostet einmalig Migrationsaufwand (<1 h), kein Datenverlust (Repos lokal vollständig). Warten hat unbekannte Dauer und blockiert den Sprint.

| Option | Aufwand | Risiko | ASPICE-Wirkung |
|---|---|---|---|
| A: Wechsel zu GitHub | <1 h | gering | keine (Storage-Location-Update in CM-Strategie) |
| B: lokal ohne Remote | 0 | hoch (kein Backup, kein Review-Flow) | SUP.8 verletzt |
| C: warten | 0 | unbekannte Blockade | Sprint-Ziel gefährdet |

**Empfehlung:** A. **Frist:** 24 h. **Default:** keiner (Infrastruktur-Grundsatz → kein risikoarmes Default).

## Warum Gold

Kompakter Sachverhalt, echte Optionen mit Konsequenzen, klare Empfehlung, bewusst kein Default bei grundsätzlicher Entscheidung.

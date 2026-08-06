# SKILL: SWE.2 Software-Architektur (v1, Sprint 3, T-0029)

Prozessziel (ASPICE 4.0): Aus reviewten SW-Anforderungen eine Architektur entwickeln, die Komponenten, Schnittstellen und dynamisches Verhalten festlegt, bewertet und verfolgbar macht. Rolle: ARCH.

## Mapping auf Basispraktiken (PAM 4.0)

Arbeits-Mapping (Kurznamen). Konformitätsanspruch pragmatisch (D010):

| BP (Kurzname) | Umsetzung im Team |
|---|---|
| Architektur entwickeln | Architekturdokument je Repo (`architecture/architektur.md`): Komponenten, Verantwortlichkeiten, Kontextdiagramm (Text/Mermaid) |
| Schnittstellen festlegen | API-first: Schnittstellen je Komponente explizit (Endpunkte, Datenformen); Leitplanke P0: verteilungsfähig, kein Zustand außerhalb Git/Hub |
| Dynamisches Verhalten beschreiben | Nur wo entscheidungsrelevant (Abläufe, Fehlerfälle) — kein Diagramm-Barock |
| Alternativen bewerten und Entscheidung begründen | ADR je Technologie-/Strukturentscheidung (`architecture/adr/adr-xxx-<slug>.md`: Kontext, Optionen, Entscheidung, Konsequenzen) |
| Konsistenz und Traceability sicherstellen | Tabelle SWR ↔ Komponente im Architekturdokument; jede SWR hat genau eine verantwortliche Komponente |
| Architektur vereinbaren/kommunizieren | G2-Vorlage an den Menschen (gate-relevant, nur Claude-Stufe) |

## Arbeitsschritte je Architektur-Ticket

1. Input prüfen: Sind alle betroffenen SWR `reviewed`? Nein → zurück an RM (Requirements-first, Playbook Kap. 5).
2. Komponentenschnitt: kleinste Menge Komponenten, die die SWRs trennscharf zuordnet; Bestand (`platform/`) wiederverwenden, bevor Neues entsteht.
3. Je Grundsatzentscheidung (Framework, Protokoll, Persistenz, Deployment) ein ADR mit mindestens 2 ernsthaften Optionen und ehrlichen Konsequenzen.
4. Trace-Tabelle SWR ↔ Komponente füllen; nicht zuordenbare SWR = Befund an RM.
5. Review einholen (QM/DEV-Kontext: umsetzbar, konsistent); danach G2-Vorlage mit Empfehlung, Kosten-/Betriebsfolgen und offenen Punkten.

## Qualitätskriterien (DoD-Auszug)

Architektur ist `done`, wenn: alle Ziel-SWR zugeordnet; jede Technologieentscheidung als ADR; Schnittstellen benannt; Review bestanden; G2-Status dokumentiert. Architektur ohne ADR oder ohne reviewte Anforderung ist ein QM-Finding.

## Gold-Beispiele

`knowledge/arch/gold-beispiele/` — Referenzfälle aus realen Tickets (ab T-0031).

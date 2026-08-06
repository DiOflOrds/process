# Gold-Beispiel PROB: Problem-Klassifikation (Referenz)

**Eingabe (Meldung DEV-Kontext, Sprint 1):** „Der Tick-Commit zu T-0010 enthält neben der CM-Strategie auch ungeplante Änderungen an Rollenkarten und Skills."

**Erwartete Ausgabe (klassifiziertes Problem-Ticket, hier: T-0014):**

- **Beobachtung/Repro:** Tick auf unsauberer Arbeitskopie gestartet; Orchestrator nutzt `git add -A` → Fremdänderungen im Ergebnis-Commit. Reproduzierbar: beliebige uncommittete Datei + Tick.
- **Schwere: hoch** (Begründung: Traceability eines Commits verletzt — Prozessintegrität, aber kein Datenverlust und kein Guardrail-Bruch → nicht kritisch, keine Sofortmeldung).
- **Dringlichkeit: hoch** (jeder weitere Tick würde das Muster wiederholen).
- **Ursache (DEV analysiert):** fehlende Precondition „saubere Arbeitskopie" + unselektives Staging.
- **Routing:** direkter Fix im Orchestrator (Task, keine Baseline betroffen → kein CR).
- **Verifikation:** Unit-Test (Tick bricht auf unsauberer Kopie ab; Commit enthält nur Tick-Artefakte) — durch andere Rolle bestätigt, dann `done`. Lessons-Kandidat an COACH (→ L-002).

*Lehre: Schwere ist begründungspflichtig und von Dringlichkeit getrennt; „kritisch" ist für Datenverlust/Sicherheit/Kosten-Anomalie reserviert.*

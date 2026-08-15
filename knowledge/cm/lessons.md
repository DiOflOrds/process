# Lessons Learned — Rolle CM

*Kuratiert vom COACH (T-0016, Prozess-CR Retro Sprint 1). Quelle: Betriebsdaten des ersten autonomen Ticks (2026-08-06). Regeln: knowledge/README.md.*

**L-001 (2026-08-06, T-0013):** Artefakt-Pfade in Datei-Blöcken sind immer **relativ zur Repo-Wurzel** anzugeben — nie mit Repo-Präfix. Falsch: `process/cm/datei.md` (ergibt `process/process/cm/datei.md`), richtig: `cm/datei.md`. Das Gateway strippt bekannte Präfixe (Schutznetz), aber der Prompt muss es von vornherein richtig vorgeben.

**L-002 (2026-08-06, T-0014):** Ein Tick darf nur auf **sauberer Arbeitskopie** starten, und Ergebnis-Commits enthalten ausschließlich die eigenen Artefakte (selektives `git add`, nie `add -A`). Der Misch-Commit aus Sprint 1 (Session-Arbeit + Tick-Ergebnis unter einer Ticket-ID) hat die Traceability genau eines Commits geschwächt — Ursache im Orchestrator behoben (Precondition + selektives Add).

**L-003 (2026-08-06, T-0011/F13):** Provider-Realität je Gerät dokumentieren, bevor eine Kette geplant wird: Der guardrails-Default `llama3.1:8b` war auf dem Team-Node nicht installiert (vorhanden: `gemma3:27b`, Override per `OLLAMA_MODEL`). Modell-Defaults gegen das Geräteregister prüfen; Abweichungen als Registry-/Guardrails-CR nachziehen.

**L-004 (2026-08-06, T-0010/T-0018):** Schwache/lokale Modelle liefern **generische** Inhalte, wenn der Kontext die projektspezifische Realität nicht enthält: Die erste CM-Strategie erfand Rollen (RE, TECHWRITER, QA) und Pfade (`project/`, `backend/`). Der Aufgaben-Kontext muss die reale Artefakt-Landkarte (Playbook Kap. 3), die Rollen aus `roles/registry.yaml` und die real existierenden Repos benennen — und das Ergebnis ist dagegen zu reviewen.

## L-2026-08-16: cmd-Klammern-Bug ERNEUT (abschluss.cmd :repo_push)

Wiederholung der P1-Lehre: `echo`-Text mit `(`/`)` innerhalb eines `if (...)`-Blocks bricht cmd mit Syntaxfehler ab — diesmal die `.kein-remote`-Meldung, Folge: ALLE Auto-Pushes seit 2026-08-15 17:36 schlugen fehl (pm/N-0006). Regel verschärft: **In .cmd-Dateien NIE Klammern in echo-Zeilen** — auch nicht escaped, einfach weglassen. Erkennungsmuster: PUSH-ANFORDERUNG.txt bleibt liegen + abschluss-auto.log endet mit „kann syntaktisch an dieser Stelle nicht verarbeitet werden".

# Heuristiken und Fallstricke — Rolle CM

*Kuratiert vom COACH (T-0016). Größenbudget beachten (knowledge/README.md).*

- **Pfad-Kontrakt:** Repo-relativ, klein geschrieben, keine Leerzeichen; vor dem Commit prüfen, dass kein doppeltes Repo-Präfix entstanden ist (L-001).
- **Saubere-Kopie-Regel:** `git status --porcelain` leer, sonst kein Tick; nach dem Tick nur eigene Artefakte stagen (L-002).
- **Geräte-Check vor Provider-Wahl:** Kette erst planen, wenn das Geräteregister das Modell/Tool bestätigt; `OLLAMA_MODEL`-Override dokumentieren (L-003).
- **Kontext-Landkarte mitgeben:** Jeder LLM-Auftrag mit Artefakt-Bezug enthält die reale Repo-/Rollen-Landkarte; generische Namen im Ergebnis sind ein Review-Stopper (L-004).
- **Mount-Artefakte (Cowork):** Nach Sandbox-Neustart mit `git status` beginnen; verwaiste `index.lock` erst entfernen, wenn kein Git-Prozess läuft (R7, Retro Sprint 1).
- **Gold-Beispiel-Regressionstest:** Vor jedem Merge eines Wissensbasis-Updates die Gold-Beispiele der Rolle gegen den neuen Stand prüfen (Playbook Kap. 11).

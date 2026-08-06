# Gold-Beispiel CM: Baseline-Manifest (SUP.8)

**Zweck:** Referenzformat für Baseline-Manifeste.

## Input

Anlass: Prozess-Baseline nach Sprint 0. Repos: process (Commit e29b250d), platform (dea07ed0), p0 (cc9dfdb1). Tag: `genesis-v0.1`.

## Referenz-Output (`baselines/genesis-v0.1-manifest.md`)

```
# Baseline-Manifest: genesis-v0.1
Anlass: Prozess-Baseline Sprint-0-Abschluss (Playbook Kap. 9)
Datum: 2026-08-05 · Erstellt: CM · Mitzeichnung: PL (QM ab Sprint 2) · Gate: G0 (D000)

| Repo | Tag | Commit | Inhalt (Item-Typen) | Prüfstatus |
|---|---|---|---|---|
| process | genesis-v0.1 | e29b250d | Rollenkarten v0, Skills v0, Templates, Registry v0 | Review PL ok |
| platform | genesis-v0.1 | dea07ed0 | Guardrails v0, board.py v0 | Review PL ok |
| p0 | genesis-v0.1 | cc9dfdb1 | Decision Log, Risikoliste, Backlog | Review PL ok |

Offene Punkte: Skills sind Stubs (T-0002); QM-Mitzeichnung erst ab Sprint 2 möglich (dokumentierte Abweichung).
```

## Warum Gold

Anlass und Gate-Bezug explizit; je Repo Commit und Prüfstatus; Abweichung (fehlender QM) dokumentiert statt verschwiegen.
